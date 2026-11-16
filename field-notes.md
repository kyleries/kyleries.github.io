---
layout: page
title: Field Notes
permalink: /field-notes/
description: Retrospectives on data platform, governance, cost, and leadership work - each written after the fact and labeled with the period it covers.
---

Each of the pieces below is a retrospective, written after the work it describes was done, and so each carries two dates: the period it covers and the date it was written. I show the period so you can place each piece in the career, and the writing date so it is clear the piece was written later.

I name public programs and past employers where the work itself was public. My current employer appears as *a 3,500-person global manufacturer*, and the figures I use there are the ones I've already put on the record elsewhere. Where I haven't put a figure on the record, I use ratios and orders of magnitude.

I'd encourage you to read them in the order below. The first group is the work that faced Finance and the business directly. The second, as it fills in, is the platform and program work underneath it, which is there so you can see that I build the foundation as well as what sits on it. The [work timeline](/work/) lists the same pieces by role, newest first.

## Finance and the business

{% assign finance = site.posts | where: "series", "field-notes" | where: "spine", "finance" | sort: "period_sort" %}
<ul class="series-list">
{%- for post in finance %}
  <li>
    <span class="series-period">{{ post.period }}{% if post.employer_label %} &middot; {{ post.employer_label }}{% endif %}</span><br>
    <a href="{{ post.url | relative_url }}"><strong>{{ post.title | escape }}</strong></a><br>
    {{ post.description }}
  </li>
{%- endfor %}
</ul>

{% assign foundations = site.posts | where: "series", "field-notes" | where: "spine", "foundations" | sort: "period_sort" %}
{% if foundations.size > 0 %}
## Platforms and programs

<ul class="series-list">
{%- for post in foundations %}
  <li>
    <span class="series-period">{{ post.period }}{% if post.employer_label %} &middot; {{ post.employer_label }}{% endif %}</span><br>
    <a href="{{ post.url | relative_url }}"><strong>{{ post.title | escape }}</strong></a><br>
    {{ post.description }}
  </li>
{%- endfor %}
</ul>
{% endif %}

If you'd rather start with a single piece, start with [causal business models](/posts/causal-business-models/). It is the closest thing to a summary of how I work.
