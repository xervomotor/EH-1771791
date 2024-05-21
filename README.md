# Meshki-interview by Kiera Tang

## Question 1 - Adding an Image to the Megamenu

### Objective

Explain how to add an image to the megamenu in Shopify's Dawn theme, using the theme settings to manage the images instead of hardcoding them.

<img width="1438" alt="Screenshot 2024-05-16 at 14 52 24" src="https://github.com/xervomotor/Meshki-interview/assets/122241297/5f3291d8-ad72-4054-b345-ef7ed5948cc0">

### General Answer

To add an image to Dawn's megamenu without hardcoding, there are 2 steps need to be done:
1. Modify **theme settings** in `config/settings_schema.json` to allow admin to upload image for megamenu (creating user-friendly no-code interface like other existing element)
2. Modify the **header file** `sections/header.liquid` to display the uploaded image in megamenu

### Detailed Implementation

#### STEP 1 - Theme Settings

Firstly, in `config/settings_schema.json`, we can include the uploading image option to megamenu by adding a block of json code

```
[
  // ...
  {
    "name": "Megamenu",
    "settings": [
      {
        "type": "image_picker",
        "id": "megamenu_image",
        "label": "Megamenu Image"
      }
    ]
  }
  // ... 
]
```
where
- `type` specifies this setting is an image picker, allowing admin to upload any image
- `id` is simply the unique identifier of this element, and can be selected by that
- `label` is the texts diplayed on admin panel

This is actually following the similar structure of the exsisting elements, such as color and typography in theme settings. Take a code snippet for an example:
```
  "name": "Typography",
  "settings": [
    {
      "type": "font_picker",
      "id": "heading_font",
      "label": "Heading font"
    },
    {
      "type": "font_picker",
      "id": "body_font",
      "label": "Body font"
    }
  ]
```

This indicates our Megamenu element can be added to admin panel for customisation with image uploading.

#### STEP 2 - Liquid Template

Then, we can add a block of code in the `sections/header.liquid` file, to display the uploaded image in the megamenu, such as

```
{% if settings.megamenu_image %}
  <div class="megamenu-image">
    <img src="{{ settings.megamenu_image | img_url: 'master' }}" alt="Megamenu Image">
  </div>
{% endif %}
```

where
- `{% if settings.megamenu_image %}` checks if the megamenu image is uploaded
- `<img src="{{ settings.megamenu_image | img_url: 'master' }}" alt="Megamenu Image">` renders the image

This is also following the structure of another existing component in `header.liquid`, as the example below:

```
<a href="{{ shop.url }}" class="site-header__logo-image">
  {% if settings.logo %}
    <img src="{{ settings.logo | img_url: 'master' }}" alt="{{ shop.name }}">
  {% endif %}
</a>
```

We can also add some CSS styles to the image to ensure the position by adding a class and select that.

### References

Most Relevant File: [header.liquid](https://github.com/Shopify/dawn/blob/main/sections/header.liquid)

Example on MESHKI’s website: [Megamenu Example](https://prnt.sc/3oW-cDRx763X)


## Question 2 - Implementing a "Load More" Button on Collection Template

### Objective
Explain how to implement a "Load More" button at the end of a list of products on Shopify's collection template. The button should load the products from page 2 onwards without refreshing the web page.

<img width="1075" alt="Screenshot 2024-05-20 at 14 48 49" src="https://github.com/xervomotor/Meshki-interview/assets/122241297/287c43cb-c72f-4cb3-8759-f573753531ef">

### General Answer

### Detailed Implementation

#### STEP 1 - Add the button

Add the 'Load More' button to the end of a product list on collection template, in `sections/main-collection-product-grid.liquid` with the following code

```
<button id="load-more" type="button">Load More</button>
```

This is similar to adding the image element to header from Question 1. 

#### STEP 2 - Use AJAX to fetch additional products

Create a `assets/loadmore.js` file, and include that in `theme.liquid`


### References

Most Relevant File:
https://github.com/Shopify/dawn/blob/main/sections/main-collection-product-grid.liquid 

Example on MESHKI’s website: https://prnt.sc/OPqbM5uYukRh
