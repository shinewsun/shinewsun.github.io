---
layout: home
---

<img src="/assets/shine_sun_headshot.jpg" alt="Portrait" width="200px" align="right" style="margin: 20px"/>
# About Me

I graduated from the University of Washington in December 2020 with a B.S. in Mathematics.

I'm always passionate about learning new things and teaching others.

I'm looking to work as a software engineer in the Seattle area. A few languages I'm proficient in:
- Ruby
- C#
- JavaScript
- Python

# What I'm working on:
{% assign sections = site.collections | where_exp: 'section', 'section.label != "posts"' %}
{% for section in sections %}
## <a href="{{ section.label }}">{{ section.label }}</a>
{% endfor %}
