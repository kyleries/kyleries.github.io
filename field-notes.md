---
layout: page
title: Field Notes
permalink: /field-notes/
description: Retrospectives on data platform, governance, cost, and leadership work - each written after the fact and labeled with the period it covers.
---

These are retrospectives. Each one was written after the work was done, and each carries two dates: the **period it covers** and the **date it was written**. I list the period first because a reader doing their homework on me deserves the chronology, and I list the writing date so it is clear the piece was written later.

I name public programs and past employers where the work itself was public. My current employer appears as *a 3,500-person global manufacturer*, and the figures I use there are the ones I've already put on the record elsewhere. Where I haven't put a figure on the record, I use ratios and orders of magnitude.

They read best in the order below - oldest period first - which is also the order of the [work timeline](/work/).

{% assign notes = site.posts | where: "series", "field-notes" | sort: "period_sort" %}
<ul class="series-list">
{%- for post in notes %}
  <li>
    <span class="series-period">{{ post.period }}{% if post.employer_label %} &middot; {{ post.employer_label }}{% endif %}</span><br>
    <a href="{{ post.url | relative_url }}"><strong>{{ post.title | escape }}</strong></a><br>
    {{ post.description }}
  </li>
{%- endfor %}
</ul>
