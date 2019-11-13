---
layout: branch.njk
title: Home
---

<ns-panel>
  <div class="splash">
    <h1>Nucleus prototype</h1>
  </div>
  <div class="splosh">
    <nav>
      <ul class="ul-bullet">
        Components
        {%- for component in collections.components %}
        <li><a href="{{ component.url }}">{{ component.data.title }}</a></li>
        {%- endfor -%}
      </ul>
      <ul class="ul-bullet">
        Pages
        {%- for page in collections.pages %}
        <li><a href="{{ page.url }}">{{ page.data.title }}</a></li>
        {%- endfor -%}
      </ul>
    </nav>
  </div>
</ns-panel>
