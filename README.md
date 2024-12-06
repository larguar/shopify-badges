# Shopify Product Badges
The Shopify Combine theme[^1] has a built-in badge feature, with settings to add custom badges that will appear if a product has a specific tag. What I needed for our product badges was a bit more involved, so rather than mess with the scema, I worked with what was there and wrote my own code for the more complex scenarios.

![Shopify Liquid Badge](https://img.shields.io/badge/-Shopify%20Liquid-750460)


## Table of Contents 
* [Code](#code)    
* [Questions](#questions) 
* [Donate](#donate)
* [Notes](#notes)


## Code

:file_folder: **snippets/product-item.liquid**

Search for `<div class="product-item__badges">` and replace[^2] all contents with it:
```
{% comment %} [LG] Custom Badges {% endcomment %}
{%- if product.tags contains "clearance" -%}
  {%- unless request.path == "/" or request.path == "/collections/last-chance" -%}
    <span class="product-item__badge cle" style="background-color: #C6CC3D; color: #1E1E1E">Last Chance</span>
  {%- endunless -%}
{%- endif -%}

{% for variant in product.variants %}
  {% for type in variant.metafields.custom.type.value %}  
    {% if type == "Glow in the Dark" or type == "Lenticular" %}  
      <span class="product-item__badge {{ type | slice: 0, 3 | downcase }}" style="background-color: #8ED6CF; color: #1E1E1E">{{ type }}</span>
    {% endif %}
  {% endfor %}
{% endfor %}

{%- if product.tags contains "new" -%}
  {%- unless request.path == "/" or request.path == "/collections/new" -%}
  <span class="product-item__badge new" style="background-color: #DDC7E8; color: #1E1E1E">New</span>
  {%- endunless -%}
{%- endif -%}

{%- assign qty = product.selected_or_first_available_variant.inventory_quantity -%}
{%- if product.available == false or qty > 0 -%}
  {%- unless qty > 5 -%}
    <span class="product-item__badge {% if product.available == false %}out{% endif %}{% if product.available %}qty{% endif %}" style="background-color: {% if product.available == false %}#FFF{% endif %}{% if product.available %}#FEFF86{% endif %}; color: #1E1E1E">
    {%- if product.available == false -%}        
      {%- if product.tags contains "clearance" -%}
        Gone for Good
      {%- elsif product.selected_or_first_available_variant.incoming -%}
        More On The Way!
      {%- else -%}
        Temporarily Out
      {%- endif -%}
    {%- elsif qty <= 5 and product.available == true -%}
      Only {{ qty }} left!
    {%- endif -%}
    </span>
  {%- endunless -%}
{%- endif -%}
{% comment %} [LG] End Custom Badges {% endcomment %}
```

Search for `<div id="product-item-{{ product.id }}"` and add to classes (keep a space between the if statement and previous classes):
```
 {% if product.available == false -%}out{%- endif %}
```

:file_folder: **assets/style.css.liquid**[^3]

Add anywhere in your custom CSS stylesheet:
```
.product-item.out > a {
  opacity: 0.5;
}
```

## Questions
If you have any questions, feel free to find me at:
* Email: laurenguardala@gmail.com
* Website: [larguar.com](https://larguar.com)
* Github: [@larguar](https://github.com/larguar)


## Donate
Appreciate this code? Say thanks with a coffee:

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/W7W21YVJJ)


## Notes
[^1]: This code is specific to the [Combine](https://themes.shopify.com/themes/combine/styles/objects) theme in Shopify, but should work for other themes with some adjustments. File names will likely be different across themes.
[^2]: Replacing the contents means the settings on the Customize end will no longer work (sold out badge, discount badge, and custom badges).
[^3]: This is the file I created for my shop's custom CSS styles. If you have your own file already created, you can add this snippet to that file. If you need to create a new file, just make sure you link to it in your `layout/theme.liquid` file.
