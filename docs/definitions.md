---
layout: page
title: Definitions
---

For clarity, several of the terms used within the Small Sewer Hydraulic Analysis (SSHA) tool are defined here.

<hr>
{% for def in site.data.definitions %}
### {{ def.term }}
{{ def.definition }}  
{% endfor %}

<!-- ### Drainage Area Index
: Drainage area feature class that is a copy of the data within the Small_Sewer_Drainage_Areas feature class for a given Project_ID.

### Project_ID
: Identifying code given to a particular set of studied sewers. This almost always should correspond to the work numbers assigned by LAMP or other.   -->
