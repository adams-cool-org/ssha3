---
layout: page
title: Equivalent Pipe Sizes
---

This data is provided as a quick reference for non-circular sewer geometries and their equivalent circular sizes, applicable within the Small Sewer Hydraulic Analysis (SSHA) tool.

<hr>
| Height | Width | Shape | Equivalent Diameter|
{% for sewer in site.data.non_circular_sizes_in_scope_1percent %}
| {{ sewer.Height }} | {{ sewer.Width }} | {{ sewer.PIPESHAPE }} | {{ sewer.Replacement_D }} |
{% endfor %}
