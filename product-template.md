---
title: Product template test
---

{% for org_hash in site.data.products %}
{% assign product = org_hash[1] %}





  
{{ product.gtin }}






  
  
{% endfor %}
