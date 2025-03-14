# Insight Hero PMP API Document

<font color="red">
*** **Disclaimer:** 

Please note that this API and its documentation are not final versions. They are subject to change, modification, or enhancement based on technical and business requirements from clients. Updates may occur without prior notice to accommodate evolving needs. 
</font>

## Overview
This API enables clients to interact with a negotiating (bidding) and ordering system for an e-commerce platform.  

**Key features include:** 

- **Bids**: Clients can submit bids on product variants, with a limit of 3 bids per price group (Product variants with same sale price) per user (tracked by IP). Bids can be accepted, rejected, or result in a special offer after 3 rejections. 

- **Orders**: Clients can submit orders to be stored in Insight Hero Database, with the system filtering items to include only those using valid discount codes tied to specific variants from the shop. 

- **Authentication**: All endpoints require a Bearer token (authToken) unique to the shop, provided in the Authorization header. 

The API is built with REST principles, uses JSON for request/response bodies, and is secured via shop-specific tokens. 

## API URL
The API URL will be provided upon request. Contact us for details.

## Authentication
- **Header**: `Authorization: Bearer <authToken>`
- **`<authToken>`**: Unique token for the shop, provided by Insight Hero.
