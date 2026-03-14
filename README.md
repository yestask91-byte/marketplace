# Marketplace API Documentation

## Base URL

```text
base_url/api/v1
```

## 1. Get Marketplace Products

Endpoint:

```text
POST /marketplace/products
```

Headers:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| API-Key | string | Yes | API kaliti |
| Content-Type | string | Yes | `application/json` |

Request body:

```json
{}
```

Rate limit:

- Scope: `API-Key`
- Window: `1 minute`
- Limit: `15 requests`

Cache:

- Scope: `API-Key`
- TTL: `1 minute`

Success response:

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    {
      "id": 2,
      "parent": 0,
      "name": "Shirinliklar",
      "image": "",
      "description": "",
      "products": [
        {
          "id": 38,
          "category": 2,
          "name": "Tort",
          "sku": "4",
          "image": "",
          "description": ""
        }
      ]
    }
  ]
}
```

Error responses:

```json
{
  "error": "Har 1 daqiqada faqat 15 ta so‘rov yuborish mumkin. Qolgan vaqt: 19s"
}
```

```json
{
  "error": "API-Key header topilmadi"
}
```

## 2. Get Marketplace Products Info

Endpoint:

```text
POST /marketplace/products/info
```

Headers:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| API-Key | string | Yes | API kaliti |
| Branch | string | Yes | Branch yoki object qiymati |

Rate limit:

- Scope: `API-Key`
- Window: `1 minute`
- Limit: `10 requests`


Success response:

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    {
      "product_id": 101,
      "price": 25000,
      "stock": 12,
      "low_stock": 3
    },
    {
      "product_id": 102,
      "price": 0,
      "stock": 4,
      "low_stock": 1
    }
  ]
}
```

Response fields:

- `product_id`: product ID
- `price`: narx; topilmasa yoki o‘chirilgan bo‘lsa `0`
- `stock`: joriy qoldiq; topilmasa `0`
- `low_stock`: minimal qoldiq; topilmasa `0`

Request example:

```bash
curl -X POST 'http://localhost:8080/api/v1/marketplace/products/info' \
  -H 'API-Key: <TOKEN>' \
  -H 'Branch: 1'
```

Error responses:

```json
{
  "error": "API-Key header topilmadi"
}
```

```json
{
  "error": "Branch header topilmadi"
}
```

```json
{
  "error": "Noto‘g‘ri API-Key"
}
```

```json
{
  "error": "Har 1 daqiqada faqat 10 ta so‘rov yuborish mumkin. Qolgan vaqt: 49s"
}
```

## 3. Create Marketplace Order

Endpoint:

```text
POST /marketplace/order
```

Headers:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| API-Key | string | Yes | Partner tomonidan berilgan API kaliti |
| AppName | string | Yes | Ilova nomi, masalan `YesPOS` |
| Content-Type | string | Yes | `application/json` |

Request body:

```json
{
  "type": 1,
  "note": "test eslatman",
  "delivery_info": {
    "scheduled_time": "",
    "distance": 1200,
    "delivery_fee": 15000
  },
  "items": [
    {
      "item": 2,
      "qty": 2,
      "price": 15000
    }
  ],
  "payments": [
    {
      "payment_id": 1,
      "value": 30000
    }
  ]
}
```

Body parametr izohi:

| Field | Type | Required | Description |
|------|------|----------|-------------|
| `type` | int | Yes | `1` order, `2` refund |
| `note` | string | No | Buyurtma eslatmasi |
| `delivery_info` | object or null | No | Yetkazib berish ma'lumoti |
| `delivery_info.scheduled_time` | string | No | Rejalashtirilgan vaqt |
| `delivery_info.distance` | number | No | Masofa, metrda |
| `delivery_info.delivery_fee` | number | No | Yetkazib berish summasi |
| `items` | array | Yes | Mahsulotlar ro‘yxati |
| `items[].item` | int | Yes | Mahsulot ID |
| `items[].qty` | number | Yes | Mahsulot soni |
| `items[].price` | number | Yes | Bir dona narx |
| `payments` | array | Yes | To‘lovlar ro‘yxati |
| `payments[].payment_id` | int | Yes | To‘lov turi ID |
| `payments[].value` | number | Yes | To‘lov summasi |

Business qoidalar:

1. `delivery_info` kerak bo‘lmasa `null` yoki umuman yubormaslik mumkin.
2. `type = 1` order, `type = 2` refund.
3. `items` ichidagi umumiy summa `payments` yig‘indisiga mos bo‘lishi kerak.
4. Xatolik bo‘lsa server JSON qaytaradi.

Current throttle behavior:

- Endpoint ketma-ket so‘rovlarni cheklaydi.
- Error matni hozir `Har 1 sekundda faqat 1 ta so‘rov yuborish mumkin...` ko‘rinishida qaytadi.

Success response:

```json
{
  "code": 0,
  "message": "OK",
  "data": "Successfully created!"
}
```

Error response:

```json
{
  "error": "Har 1 sekundda faqat 1 ta so‘rov yuborish mumkin. Qolgan vaqt: 1s"
}
```

```json
{
  "error": "API-Key header topilmadi"
}
```

Curl example:

```bash
curl --location 'https://base_url/api/v1/marketplace/order' \
  --header 'API-Key: demo-token' \
  --header 'AppName: YesPOS' \
  --header 'Content-Type: application/json' \
  --data '{
    "type": 1,
    "note": "test eslatman",
    "delivery_info": null,
    "items": [
      { "item": 2, "qty": 2, "price": 15000 }
    ],
    "payments": [
      { "payment_id": 1, "value": 30000 }
    ]
  }'
```

## 4. Branch List

Endpoint:

```text
POST /branch/list
```

Headers:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| API-Key | string | Yes | API kaliti |
| Content-Type | string | Yes | `application/json` |

Request body:

```json
{}
```

Rate limit:

- Scope: `API-Key`
- Window: `1 minute`
- Limit: `15 requests`

Cache:

- Scope: `API-Key + org_id`
- TTL: `1 minute`

Success response:

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    {
      "id": 1,
      "name": "Yespos 1-Fillial",
      "comment": "",
      "address": "",
      "phone": ""
    },
    {
      "id": 2,
      "name": "Yespos 2-Fillial",
      "comment": "",
      "address": "",
      "phone": ""
    }
  ]
}
```

Error response:

```json
{
  "error": "Har 1 daqiqada faqat 15 ta so‘rov yuborish mumkin. Qolgan vaqt: 57s"
}
```

## 5. Webhook: Stock and Price Update

Current status:

- This repository does not expose a webhook endpoint for this flow.
- The contract below is kept as an external integration format for partner webhook receivers.

Endpoint:

```text
POST {partner_webhook_url}
```

Headers:

| Name | Value |
|------|-------|
| Authorization | `Basic <base64(username:password)>` |
| Content-Type | `application/json` |

Request body:

```json
{
  "api_key": "demo-token",
  "branch": 1,
  "products": [
    {
      "product_id": 1,
      "price": 1000,
      "stock": 2
    }
  ]
}
```

Success response:

```json
{
  "status": "success",
  "message": "Webhook received successfully"
}
```

Error response:

```json
{
  "error": "Invalid API key"
}
```
