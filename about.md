---
layout: default
title:  "About"
---

<div class="author-profile">
  <h1 class="author-name">{{ site.author }}</h1>
  <div class="bio"></div>
  <div class="description"></div>
    {% assign arr = site.email | split: "[at]" %}
    <p>
      <span>Email:&nbsp;&nbsp;</span>
      <span>{{ arr[0] }}<code class="language-plaintext">[at]</code>{{ arr[1] }}</span>
    </p>
    
    <p>(Replace the <code class="language-plaintext">[at]</code>  with @ in the email addresses)</p>
    <p class="username">Don’t hesitate to contact me if you have any question.</p>
</div>


