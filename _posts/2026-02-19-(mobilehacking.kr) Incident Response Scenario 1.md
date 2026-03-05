---
title: (mobilehacking.kr) Incident Response Scenario 1
date: 2026-02-19 05:24:11 +09:00
last_modified_at: 2026-02-19 06:41:59 +09:00
categories: [wargame, mobilehacking.kr]
tags:
  [
    'mobilehacking.kr',
    'forensic'
  ]
---

이번에는 뭔가 악성 앱에 의한 피해를 받은 피해자의 안드로이드 폰 이미지 파일이 주어진다.<br>

문제를 풀기에 앞서 Android 이미지를 분석하는 법을 정리해보자.

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_1.PNG" width="500">

가장 먼저 보이는 파일은 data와 mnt이다.<br>
data는 앱 데이터, 시스템 설정 등이 들어있는 내부 저장소이고<br>
mnt or data/media는 다운로드, 미디어 파일이 들어있는 외부 저장소(sd card)라고 생각하면 된다.

data/data 경로로 들어가면 

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_2.PNG" width="600">

많은 앱들이 존재하고 각 앱은 UID로 보호된 전용 샌드박스를 가진다.

```
data/data/<package>/
├── databases/        ← SQLite DB (채팅, 히스토리, 카드정보 등)
├── shared_prefs/     ← XML 설정 파일 (토큰, 설정값)
├── files/            ← 앱 생성 파일
├── cache/            ← 캐시 (임시 파일, 로그)
├── no_backup/        ← 백업 제외 데이터 (WorkDB 등)
└── app_webview/      ← WebView 데이터 (쿠키, 세션, Local Storage)
```
이렇게 파일 구조들을 가지고 

**주요 분석 대상**

- `databases/*.db` → `sqlite3` 명령으로 쿼리
- `shared_prefs/*.xml` → 텍스트 파일, 직접 열람
- `app_webview/Default/Cookies` → SQLite (세션 쿠키)
- `app_webview/Default/Local Storage/leveldb/` → leveldb (웹 localStorage)
- `cache/*.log`, `cache/*.json` → 악성 앱 로그 가능

등이 주요 분석 대상이 된다.

data/app/<hash>/<package>/ 경로에서는 실제로 설치된 APK들을 볼 수 있다.<br>
앱 분석하듯 jadx를 통해서 까서 볼 수 있다.

data/system 에서는 앱 패키지 정보, 패키지명, UID, 동기화 이벤트 등을 볼 수 있다고도 한다.

### 외부 저장소

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_3.PNG" width="600">

```
Download/          ← 다운로드된 파일 (APK, txt 등)
Android/data/      ← 앱별 외부 저장소
Pictures/          ← 사진
DCIM/              ← 카메라 촬영 사진
Documents/         ← 문서
```
- `Download/` 폴더의 APK → 사이드로드 흔적
- 평문 텍스트 파일 → 민감정보 유출 증거
- `Android/data/<pkg>/files/` → 앱이 외부에 저장한 파일

등이 있고

### 브라우저 히스토리
크롬의 경우 **com.android.chrome**

```
app_chrome/Default/History           ← SQLite (방문 URL)
app_chrome/Default/Web Data          ← 자동완성, 검색어
app_chrome/Default/Cookies           ← 세션 쿠키
```

기본 브라우저는 **com.android.browser**

```
cache/browser_state.parcel    ← 마지막 방문 URL (binary, strings 명령 활용)
shared_prefs/com.android.browser_preferences.xml ← 홈페이지 등 설정
cache/WebView/Default/HTTP Cache/  ← 캐시된 HTTP 응답 (URL 포함)
```

가 있다.<br>
mmssms.db를 통해 문자 분석도 할 수 있다.

이제 문제로 돌아가서 구해야 하는 것은 
```
flag{clue1_clue2_clue3}
- clue1: 앱 설치 페이지로 유도된 URL
- clue2: 외부 저장소에 노출된 민감정보 형식
- clue3: 카드 정보를 수집한 앱
```
이다.

### clue 1

우선 설치 페이지로 유도된 것이니까 

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_4.PNG" width="500">

chrome과 브라우저의 히스토리를 봤는데 브라우저의 히스토리에서 이런 경로들이 보인다.<br>
더 자세한 것은 com.android.browser/cache/WebView/Default/HTTP Cache 에서 볼 수 있다고 한다.

```
strings * | grep "http://172.30"
```
를 통해서 긁어보면 

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_5.PNG" width="500">

뭔가 수상한 앱들을 잔뜩 받고 있기 때문에 여기라고 확정을 지어도 될 것 같다.

Clue 1 = `http://172.30.1.61:8088`

### clue 2
다음은 외부 저장소에 노출된 민감정보 형식이다.

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_6.PNG" width="500">

우선 외부 저장소의 download 폴더를 보면 분명 노출 정보 같은 것이 있다.

아마 카드번호로 보이기에 clue2는 `211250980212525` 가 될 것 같지만 어디에서 이 txt파일을 만들었을까.<br>
그 정답이 악성 앱이 될 것이다.

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_7.PNG" width="500">

그리고 이 com.card.safe안의 database에서 같은 카드 정보를 볼 수 있었다.

이 앱을 jadx로 까서 보면

<img src="/assets/img/wargame/mobilehacking.kr/IncidentScenario1_8.PNG" width="500">

이렇게 database에 넣는다는 것도 알게 되었으므로 <br>

Clue 2=`211250980212525`<br>
Clue 3=`com.card.safe` 가 된다.

그렇기에 다 합치면 정답은
```
flag{http://172.30.1.61:8088_211250980212525_com.card.safe}
```
로 구할 수 있었다.