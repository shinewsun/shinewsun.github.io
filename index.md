---
layout: home
---

<img src="/assets/shine_sun_headshot.jpg" alt="Portrait" width="200px" align="right" style="margin: 20px"/>
# About Me

I graduated from the University of Washington in December 2020 with a B.S. in Mathematics. My favorite subject was topology.

I'm always passionate about learning new things and teaching others. I also love making things with my own hands and solving problems with other people. I'm not satisfied with a product until it exceeds expectations.

A few languages I'm proficient in:
- Ruby
- C#
- JavaScript
- Python

# What I'm working on:
{% for section in site.data.sections %}
## <a href="{{ section.label }}">{{ section.label }}</a>
{% endfor %}
