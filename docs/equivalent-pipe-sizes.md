---
layout: page
title: Equivalent Sewers Sizes
---

This data is provided as a quick reference to identify the equivalent circular sewer diameters for all non-circular sewer geometries applicable within the Small Sewer Hydraulic Analysis (SSHA) tool. Here, equivalent diameters represent the minimum replacement sewer size (in inches) necessary to match or exceed the full flow capacity of each non-circular sewer geometry.

<hr>

### Egg Sewers: Equivalent Sizes

| Height (in) | Width (in) | Equivalent Diameter (in) |
| :------: | :-----: | :------------------: |{% for sewer in site.data.non_circular_sizes_in_scope_1percent %}{% if sewer.PIPESHAPE == "EGG" %}
| {{ sewer.Height }} | {{ sewer.Width }} | {{ sewer.Replacement_D }} |{% endif %}{% endfor %}

### Box Sewers: Equivalent Sizes

| Height (in) | Width (in) | Equivalent Diameter (in) |
| :------: | :-----: | :------------------: |{% for sewer in site.data.non_circular_sizes_in_scope_1percent %}{% if sewer.PIPESHAPE == "BOX" %}
| {{ sewer.Height }} | {{ sewer.Width }} | {{ sewer.Replacement_D }} |{% endif %}{% endfor %}
