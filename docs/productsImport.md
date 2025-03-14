# Import Products to Insight Hero Database

Currently, we only accept product data from clients in CSV format, which we will manually import into our `Products` table in the Insight Hero Database. This document outlines the required CSV structure and field specifications.

## CSV Structure

- Clients must provide product data in a CSV file with the following columns.
- Fields marked as "Required" must be present in the CSV and contain a non-empty value (unless otherwise noted). Fields marked as "Not Required" may be omitted or left blank (`""`).

### CSV Fields

| Field          | Type    | Required | Description |
|----------------|---------|----------|-------------|
| `productId`    | Number  | Yes      | Unique product ID from the client’s system (e.g., `1001`). |
| `variantId`    | Number  | Yes      | Unique variant ID from the client’s system (e.g., `2001`). Must be unique across all products. |
| `variantName`  | String  | No       | Name of the variant (e.g., "Red"). Can be blank (`""`) or omitted.|
| `price`        | Number  | Yes       | Price of the variant (e.g., `50.00`). |
| `minPrice`     | Number  | Yes       | Minimum acceptable bid price (e.g., `40.00`). |
| `inventory`    | Number  | No       | Stock quantity (e.g., `10`). Can be blank (`""`) or omitted. |
| `name`         | String  | Yes      | Product name (e.g., "T-Shirt"). Must be provided and non-empty. |
| `productType`  | String  | No       | Type or category of the product (e.g., "Clothing"). Can be blank (`""`) or omitted. |
| `tags`         | String  | No       | Comma-separated tags (e.g., "summer, sale"). Can be blank (`""`) or omitted. |
| `bidEnabled`   | Boolean | Yes      | Whether bidding is enabled (`1` for true, `0` for false). Defaults to `0` if invalid. |
| `sku`          | String  | No       | Stock Keeping Unit (e.g., "TSHIRT-RED"). Can be blank (`""`) or omitted. |
| `vendor`       | String  | No       | Vendor name (e.g., "Acme Corp"). Can be blank (`""`) or omitted. |

### Notes
- **Header Row**: The CSV must include a header row with these exact column names (case-sensitive).
- **Numeric Fields**: Use plain numbers (e.g., `1001`, `50.00`) without quotes, except when blank (`""`).
- **Boolean Field**: `bidEnabled` should be `1` (true) or `0` (false). Invalid values default to `0`.
- **String Fields**: Use quotes only if the value contains commas or special characters (standard CSV rules apply).
- **Unique Constraint**: `variantId` must be unique across all rows in the CSV and existing database records.

### Example CSV
```csv
productId,variantId,variantName,price,minPrice,inventory,name,productType,tags,bidEnabled,sku,vendor
1001,2001,Red,50.00,40.00,10,T-Shirt,Clothing,"summer, sale",1,TSHIRT-RED,Acme Corp
1001,2002,Blue,50.00,40.00,5,T-Shirt,Clothing,,0,TSHIRT-BLUE,
1002,2003,,75.00,55.00,,"Laptop Bag",Accessories,,1,LAPBAG-001,Tech Inc
```

#### Explanation of Example CSV
**Row 1**: Fully populated row with all fields.</br>
**Row 2**: Omits `tags` and `vendor`, leaves them blank ("").</br>
**Row 3**: Omits `variantName`, `inventory`, `tags`, and `vendor`, showing optional fields can be empty or absent.


### Submission Process
1. Prepare your CSV file according to the structure above.
2. Send the file to us.
3. We will validate the CSV, map it to the appropriate shopId, and import it into the Products table manually.
4. You will receive confirmation once the import is complete, or feedback if there are issues (e.g., duplicate variantId).