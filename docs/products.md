# Products

## GET /api/products/:productId

### Description
Retrieve all variants of a product by its `productId`, scoped to the authenticated shop. Returns an array of product variants if found.

### Path Parameter
| Parameter              | Type   | Required | Description                          |
|------------------------|--------|----------|--------------------------------------|
| productId	             | Number | Yes	     | Unique product ID                    |

### Example Request
```
GET /api/products/1001
```

### Responses
The response includes an array of product variants matching the `productId`, or an error if no variants exist for the shop.

#### Response Fields
| Field	| Type | Description |
|-------|------|-------------|
| message |	String | A human-readable success message |
| products | Array | List of product variants (empty array if not found in error cases) |
| products[].productId | Number | Client-provided product ID |
| products[].variantId | Number | Unique variant ID |
| products[].name | String | Product name (e.g., "T-Shirt") |
| products[].variantName | String | Variant name (e.g., "Red") |
| products[].price | Number | Price of the variant |
| products[].minPrice | Number | Minimum acceptable bid price for the variant |

#### Success (Status: 200 OK)
```json
{
    "message": "Product retrieved successfully",
    "products": [
        {
            "productId": 1001,
            "variantId": 2001,
            "name": "T-Shirt",
            "variantName": "Red",
            "price": 50.00,
            "minPrice": 40.00
        },
        {
            "productId": 1001,
            "variantId": 2002,
            "name": "T-Shirt",
            "variantName": "Blue",
            "price": 50.00,
            "minPrice": 40.00
        }
    ]
}
```

#### Error (Status: 400 Bad Request)
- Invalid productId:
```json
{ "error": "Invalid productId: must be a number" }
```

#### Error (Status: 404 Not Found)
- Product Not Found:
```json
{ "error": "Product with productId 1001 not found for this shop" }
```

## GET /api/products/variant/:variantId

### Description
Retrieve a specific product variant by its `variantId`, scoped to the authenticated shop. Returns detailed information about the variant if found.

### Path Parameter
| Parameter	| Type | Required | Description |
|-----------|------|----------|-------------|
| variantId	| Number | Yes | Unique variant ID |

### Example Request
```
GET /api/products/variant/2001
```

### Responses
The response includes the product variant details if found, or an error if the variant doesn’t exist or isn’t associated with the shop.

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| message | String | A human-readable success message |
| product | Object | The product variant details (null if not found in error cases) |
| product.productId	| Number | Client-provided product ID |
| product.variantId	| Number | Unique variant ID |
| product.name | String | Product name (e.g., "T-Shirt") |
| product.variantName | String| Variant name (e.g., "Red") |
| product.price | Number | Price of the variant |
| product.minPrice | Number	| Minimum acceptable bid price for the variant |

#### Success (Status: 200 OK)
```json
{
    "message": "Product retrieved successfully",
    "product": {
        "productId": 1001,
        "variantId": 2001,
        "name": "T-Shirt",
        "variantName": "Red",
        "price": 50.00,
        "minPrice": 40.00
    }
}
```

#### Error (Status: 400 Bad Request)
- Invalid variantId:
```json
{ "error": "Invalid variantId: must be a number" }
```

#### Error (Status: 404 Not Found)
- Variant Not Found:
```json
{ "error": "Product with variantId 2001 not found for this shop" }
```
