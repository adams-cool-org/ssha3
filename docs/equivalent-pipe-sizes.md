---
layout: page
title: Equivalent Pipe Sizes
---

This data is provided as a quick reference for non-circular sewer geometries and their equivalent circular sizes, applicable within the Small Sewer Hydraulic Analysis (SSHA) tool.

<hr>

### Egg Sewers: Equivalent Sizes

| Height | Width | Equivalent Diameter |
| :------: | :-----: | :------------------: |{% for sewer in site.data.non_circular_sizes_in_scope_1percent %}{% if sewer.PIPESHAPE == "EGG" %}
| {{ sewer.Height }} | {{ sewer.Width }} | {{ sewer.Replacement_D }} |{% endif %}{% endfor %}

### Box Sewers: Equivalent Sizes

| Height | Width | Equivalent Diameter |
| :------: | :-----: | :------------------: |{% for sewer in site.data.non_circular_sizes_in_scope_1percent %}{% if sewer.PIPESHAPE == "BOX" %}
| {{ sewer.Height }} | {{ sewer.Width }} | {{ sewer.Replacement_D }} |{% endif %}{% endfor %}
