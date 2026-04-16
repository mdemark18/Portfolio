# Miller's Games
**Web Programming Course — Spring 2025**  
**Author:** Miller DeMark | **Live Site:** [millersgames.free.nf](http://millersgames.free.nf/)

---

## Overview

Miller's Games is a full-stack e-commerce web application built for a Web Programming course in Spring 2025. The site functions as a fully operational game marketplace, modeled after retailers like GameStop or Best Buy. Users can browse a catalog of 17 products across multiple categories, manage an account, build a cart and wishlist, and simulate placing an order complete with a receipt. Everything works as a real storefront would, short of actual payment processing and order fulfillment.

The site is hosted live on InfinityFree and is backed by a MySQL relational database.

---

## Technology Stack

- **Frontend:** HTML5, CSS3, JavaScript, Bootstrap 5
- **Backend:** PHP with server-side session management
- **Database:** MySQL via mysqli
- **Hosting:** InfinityFree
- **Other:** Python utility script for database population

---

## Features

### Product Catalog
The catalog contains 17 products across four categories: Games, Consoles, Controllers, and Accessories. Each product has a dedicated page with its image, description, and price. The homepage dynamically pulls a randomized selection of featured and popular products on each load, keeping the front page fresh. Users can also filter the catalog by category.

### User Accounts
Users can register with an email and password, which is hashed using PHP's `password_hash()` before being stored. Login is session-based, with the session persisting across pages until the user logs out. Authenticated users see a personalized greeting in the nav with a dropdown for account options. Unauthenticated users see login and register links instead. Users can also change their password from their profile page.

### Shopping Cart
The cart is persistent and tied to the user's session. Products can be added directly from the catalog or homepage, and the cart page displays a running total. From the cart, users proceed through a two-step checkout: entering a shipping address, then confirming the order.

### Wishlist
Users can add products to a wishlist separate from the cart, accessible from the nav dropdown. This allows saving items for later without committing to a purchase.

### Order History and Receipts
When a user completes checkout, the order is recorded in the database with a timestamp, itemized products, quantities, and a total price. A receipt is generated immediately after purchase. All past orders are viewable from the Order History page, giving a full record of everything the user has placed.

---

## Database Schema

The application uses five relational tables:

**users** stores registered accounts with hashed passwords and first names.

**products** stores all 17 catalog items with name, description, price, category, and image path.

**orders** records each completed order with a user ID, timestamp, and total price.

**order_items** breaks each order into individual line items, storing the product ID, quantity, and per-item price at the time of purchase.

**wishlist** (managed via wishlist.php) links users to saved products outside of the cart flow.

Foreign key relationships link orders to users and order items to both orders and products, keeping data consistent across the application.

---

## Security

- Passwords are hashed with `password_hash()` and never stored in plaintext
- Output is sanitized with `htmlspecialchars()` throughout to prevent XSS
- User-specific data (cart, orders, wishlist) is only accessible to authenticated sessions

---

## Project Notes

- Payments are simulated. No real payment processor is integrated.
- The site is hosted on InfinityFree under a default domain name. All functionality is as shown in the source code.
- A Python script (`update.py`) is included for database seeding and management outside of the web interface.
- Full source code is available in the [repository](https://github.com/mdemark18/WebProgramming25).