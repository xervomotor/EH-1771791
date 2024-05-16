# Meshki-interview by Kiera Tang

## Question 1 - Adding an Image to the Megamenu

### Objective

Explain how to add an image to the megamenu in Shopify's Dawn theme, using the theme settings to manage the images instead of hardcoding them.

<img width="1438" alt="Screenshot 2024-05-16 at 14 52 24" src="https://github.com/xervomotor/Meshki-interview/assets/122241297/5f3291d8-ad72-4054-b345-ef7ed5948cc0">

### General Answer

To add an image to Dawn's megamenu without hardcoding, there are 2 steps need to be done:
1. Modify theme settings in `config/settings_schema.json` to allow admin to upload image for megamenu (creating user-friendly no-code interface like other existing element)
2. Modify the header file `sections/header.liquid` to display the uploaded image in megamenu

### Detailed Implementation

### References

Most Relevant File: [header.liquid](https://github.com/Shopify/dawn/blob/main/sections/header.liquid)

Example on MESHKI’s website: [Megamenu Example](https://prnt.sc/3oW-cDRx763X)


## Question 2 - Implementing a "Load More" Button on Collection Template

### Objective
Explain how to implement a "Load More" button at the end of a list of products on Shopify's collection template. The button should load the products from page 2 onwards without refreshing the web page.

### Answer

### References

Most Relevant File:
https://github.com/Shopify/dawn/blob/main/sections/main-collection-product-grid.liquid 

Example on MESHKI’s website: https://prnt.sc/OPqbM5uYukRh
