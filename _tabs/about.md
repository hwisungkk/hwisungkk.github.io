---
icon: fas fa-info-circle
order: 4
---

{% assign resume_pdf_url = site.resume.pdf_url | default: '/about.pdf' %}
{% assign first_pdf_url_char = resume_pdf_url | slice: 0 %}
{% if first_pdf_url_char == '/' %}
  {% assign resume_pdf_url = resume_pdf_url | relative_url %}
{% endif %}

<script>
  window.location.replace("{{ resume_pdf_url }}");
</script>

<p>
  <a href="{{ resume_pdf_url }}">About PDF</a>
</p>
