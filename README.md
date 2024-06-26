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

Create a `assets/load-more.js` file, and include that in `layout/theme.liquid` to ensure global availibility, by
```
<script src="{{ 'load-more.js' | asset_url }}" defer="defer"></script>
```
just like including other js files in the existing code.

Then, in the `load-more.js`, we can implement AJAX functionality to fetch next page of products and append them to the list, for example

```
// run the function when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
  // set a tracker for page, and initialise that to 1
  let currentPage = 1;

  // select the load more button and the product grid by their IDs
  const loadMoreButton = document.getElementById('load-more');
  const productGrid = document.getElementById('product-grid');

  // increament page count when the load more button is clicked
  loadMoreButton.addEventListener('click', ()=> {
    currentPage++;

    // AJAX request
    fetch(`someURL/page=${currentPage}`) // should be some URL that point to the product collection for the store
      .then(response => response.text()) //parse the response
      .then(data => {
        // parse into DOM object
        const parser = new DOMParser();
        const htmlDocument = parser.parseFromString(data, 'text/html');
    
        // select all new products from the fetched response
        const newProducts = htmlDocument.querySelectorAll('.product'); // not sure what are the product class, just an indication
        newProducts.forEach(product => productGrid.appendChild(product)); // append new products to the exsiting grid
      })
      .catch(error => console.error('Error fetching products:', error));
  });
});
```

We also need to consider the edge cases such as making the button disappear when reading the final page. We can do this by setting the button display to `none` under this condition.

#### Explaination on updating without refreshing the page

In our AJAX request, we directly append the new products to the existing product grid by this line
```
newProducts.forEach(product => productGrid.appendChild(product));
```

That means the product grid is getting more children, and it should work dynamically without refreshing the page.

### References

Most Relevant File:
https://github.com/Shopify/dawn/blob/main/sections/main-collection-product-grid.liquid 

Example on MESHKI’s website: https://prnt.sc/OPqbM5uYukRh
