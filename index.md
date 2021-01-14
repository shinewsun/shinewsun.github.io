---
layout: home
---

{% assign sections = site.collections | where_exp: 'section', 'section.label != "posts"' %}
{% for section in sections %}
## <a href="{{ section.label }}">{{ section.label }}</a>
{% endfor %}
