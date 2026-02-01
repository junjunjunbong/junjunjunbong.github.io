---
layout: page
permalink: /publications/
title: publications
description: Publications in reversed chronological order.
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

<div class="publications">
{% bibliography %}
</div>

{% capture bib_content %}{% bibliography %}{% endcapture %}
{% if bib_content == "" or bib_content contains "no entries" %}
<p class="text-center mt-5">Publications coming soon...</p>
{% endif %}
