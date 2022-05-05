---
layout: essay
type: essay
title: Checkpoint 3
date: 2022-01-24
labels:
  - ITM352
  - HTML
  - Javascript
  - BLOG
---
1. Show what each page will look like. The pages do not have to be “functional” but the design should clear. 
![asst3](https://user-images.githubusercontent.com/97568424/166875420-6b6669fa-48dd-4658-8b3c-1b94850a02e8.jpg)

2. Describe your design for your site’s shopping cart. That is, will it be a separate page that the user can view and edit, or will it be integrated into the product pages? If so, describe in detail how this will work on your site. Provide several examples of using the cart.
It will be a separate page for the cart that the items get sent to. Each product page only has one add to cart button so it can be added in bulk. I also made it overwrite instead of adding the numbers together if there is already added items.

3. Explain specifically how you will use sessions to manage your shopping cart. In particular, what shopping cart data will be stored in the session, what data format will be used (NOT what data type, but the format like with the data format used for your registration data). Use code examples showing what data structures (such as arrays and their objects) you will use to manage the shopping cart data and how they will be used in a session.
The cart will be an object and the products key will be inside that object as arrays.

4.  How will you avoid access to your application when the user has not logged in or registered? What are the particular security concerns you must address?
The checkout button will send you to the login page if there isn’t a cookie. If they user is already logged in then it’ll go to the invoice. I think a security concern is the email and name being stored in the cookie and it can be accessed.

5. Upon a successful login, how do you provide personalization in your UI? Explain how you did or will do this (paste code if necessary)
I’ll take the name info saved in the cookie to personalize the invoice and email message.

6.  If you are working with partners, how will you split up the work in your team so that you are working in parallel as effectively as possible? That is, who is doing what and when?
No partners.

7. How are you approaching Assignment 3 differently than Assignment 2?
Assignment 3 is bringing everything together and there is a lot less code to write to do so. 
