---
title: Product template test
---

{% assign product = site.data.products[0] %}

test

{{ product.gtin }}

<ul>
{% for org_hash in site.data.products %}
{% assign org = org_hash[1] %}
  <li>
    <a href="https://github.com/{{ org.gtin }}">
      {{ org.gtin }}
    </a>
  </li>
{% endfor %}
</ul>
