---
layout: page
title: Field Notes
permalink: /field-notes/
description: Retrospectives on data platform, governance, cost, and leadership work — each written after the fact and labeled with the period it covers.
---

These are retrospectives, not journals. Each one was written after the work was done, and each carries two dates: the **period it covers** and the **date it was written**. I've kept them in that order because a reader doing their homework on me deserves the chronology, and because I think the honest version of "backdated" is "labeled."

Employer names follow a simple rule. Public programs and past employers are named where the work itself was public. My current employer appears as *a 3,500-person global manufacturer*, and the figures I use there are the ones I've already put on the record elsewhere — ratios and orders of magnitude otherwise.

They read best in the order below — oldest period first — which is also the order of the [work timeline](/work/).

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

If you'd rather start with a single piece, start with the one on [absorbing acquisitions and retiring a dozen systems](/posts/absorbing-acquisitions-retiring-a-dozen-systems/) — it's the closest thing to a summary of how I work.
