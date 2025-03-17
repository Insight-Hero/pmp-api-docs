# Bids

## POST /api/bids

### Description
Submit a bid for a product variant. Bids are tracked per user (via encrypted IP) and shop, grouped by variant price. After 3 bids on the same price group, a special offer may be issued.

### Request Body
| Field      | Type   | Required | Description              |
|------------|--------|----------|--------------------------|
| productId  | Number | Yes      | ID of the product        |
| variantId  | Number | Yes      | ID of the variant        |
| bid        | Number | Yes      | Bid amount (e.g., 20.00)|

#### Example Request Body
```json
{
    "productId": 1001,
    "variantId": 2001,
    "bid": 20.00
}
```

### Responses
The response indicates whether the bid was accepted or rejected, provides details about any existing discounts or special offers, and tracks bidding attempts for the price group. The status code and message provide additional context for the outcome.

#### Response Fields

| Field              | Type    | Description                                                                                   |
|--------------------|---------|-----------------------------------------------------------------------------------------------|
| `value`            | Boolean | Indicates if the bid was accepted (`true`) or rejected (`false`).                             |
| `isPreviousAccepted` | Boolean | `true` if the user has an accepted bid or special offer for this price group in the past 24 hours; otherwise `false`. |
| `discountCode`     | String  | The discount code if the bid is accepted or a previous discount is available; empty string (`""`) if not applicable. |
| `discount`         | Number  | The discount amount applied (in currency units); `0` if no discount is provided.              |
| `message`          | String  | A human-readable message explaining the bid outcome (e.g., rejection reason or acceptance confirmation). |
| `statusCode`       | String  | A custom code indicating the bid status (e.g., `"260"` for rejected with chances remaining).  |
| `isSpecialOffer`   | Boolean | `true` if the response includes a special offer (e.g., after 3 rejections); otherwise `false`. |
| `blockTime`        | Number  | Timestamp (in milliseconds) when a previous discount was created; `0` if not applicable.      |
| `isSameVariant`    | Boolean | `true` if the bid is for the same variant as a previous bid; otherwise `false`.              |
| `bidsLeft`         | Number  | Number of bidding attempts remaining for this price group (e.g., `2` out of 3). Omitted if bid is accepted. |
| `bidCount`         | Number  | Total number of bids made for this price group (e.g., `1`). Omitted if bid is accepted.      |
| `target`         | Number[]  | The same price group that the discount code is issued for      |

#### Rejected Bid (Status: 200 OK)
Occurs when `bid < minPrice` (minimum acceptable bid price, set in Insight Hero Client's Products Database), with bids remaining.

```json
{
    "value": false, 
    "isPreviousAccepted": false, 
    "discountCode": "",
    "discount": 0,
    "message": "Thanks for your bid! It's too low (2 of 3 chances left for this price group).",
    "statusCode": "260",
    "isSpecialOffer": false,
    "blockTime": 0,
    "isSameVariant": true,
    "bidsLeft": 2,
    "bidCount": 1
}
```

#### Accepted Bid (Status: 200 OK)
Occurs when `bid >= minPrice`.

```json
{
  "value": true,
  "isPreviousAccepted": false,
  "discountCode": "BID-XGG01F",
  "discount": 10,
  "message": "Your bid was accepted for this price group.",
  "statusCode": "250",
  "isSpecialOffer": false,
  "blockTime": 1615820400000,
  "isSameVariant": true,
  "target": [2001, 2002]
}
```

#### 3rd Rejection, Special Offer Created (Status: 200 OK)
After 3 rejected bids on the same price group, a special offer is issued.
```json
{
    "value": true,
    "isPreviousAccepted": false,
    "discountCode": "SPECIAL456",
    "discount": 15.00,
    "message": "Special offer created for this price group!",
    "statusCode": "240",
    "isSpecialOffer": true,
    "blockTime": 1649462400000,
    "isSameVariant": true,
    "target": [2001, 2002]
}
```

#### Errors (Status: 400 Bad Request or 401 Unauthorized)

- Missing Token:
```json
{ "error": "Unauthorized: Missing or invalid Bearer token" }
```

- Invalid Bid:
```json
{ "error": "Bid value must be a valid number" }
```

- Product Not Found:
```json
{ "error": "Product not found" }
```