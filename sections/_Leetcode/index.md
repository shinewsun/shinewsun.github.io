---
layout: page
title: "Leetcode"
---

Here I walk through solutions of Leetcode questions.

The primary language I use is [Ruby](https://www.ruby-lang.org/en/), but I give approaches and ideas which are language-independent. [Why Ruby?](/Leetcode/whyruby/)

<ul>
{% assign posts = site['Leetcode'] | where_exp: 'post', 'post.num' | sort: 'num' %}
{% for post in posts %}
    {% if post.phony != true %}
        <li>
            <a href="{{ post.url }}">{{ post.title }}</a>
        </li>
    {% endif %}
{% endfor %}
</ul>
