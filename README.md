# 🧾 Marketplace API Documentation

## Base URL
```
base_url/api/v1/marketplace
```

---

## 🔑 Authentication

Har bir so‘rov uchun `API-key` headerda yuborilishi **majburiy**:

```bash
--header 'API-key: demo-CI6IkpXVCJ9.eyJhdXRob3'
```

---

## 🛒 1. Get Products

### Endpoint
```
POST /api/v1/marketplace/products
```

### Headers
| Key | Value |
|-----|--------|
| Content-Type | application/json |
| API-key | sizning API kalitingiz |

### Request Body
```json
{
  "org_id": "ORG_ID"
}
```

### Success Response
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

### Error Responses
#### ❌ Rate Limit
```json
{
  "error": "Har 1 daqiqada faqat 1 ta so‘rov yuborish mumkin. Qolgan vaqt: 19s"
}
```

#### ❌ API Key Not Found
```json
{
  "error": "API-Key header topilmadi"
}
```

### cURL Example
```bash
curl --location 'base_url/api/v1/marketplace/products' --header 'API-key: demo-CI6IkpXVCJ9.eyJhdXRob3' --header 'Content-Type: application/json' --data '{
    "org_id": "ORG_ID"
}'
```

---

## 🧾 2. Create Order

### Endpoint
```
POST /api/v1/marketplace/order
```

### Headers
| Key | Value |
|-----|--------|
| Content-Type | application/json |
| API-key | sizning API kalitingiz |

### Request Body
```json
{
  "org_id": "51824731",
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

### Success Response
```json
{
  "code": 0,
  "message": "OK",
  "data": "Successfully created!"
}
```

### cURL Example
```bash
curl --location 'base_url/api/v1/marketplace/order' --header 'API-key: demo-CI6IkpXVCJ9.eyJhdXRob3' --header 'Content-Type: application/json' --data '{
    "org_id": "51824731",
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
}'
```

---

## ⚠️ Error Codes

| Code | Description |
|------|--------------|
| `0` | OK |
| `400` | Invalid request or missing fields |
| `401` | API-Key missing or invalid |
| `429` | Too many requests (rate limit) |

---

## 🧠 Notes

- Har bir `org_id` uchun 1 daqiqada **faqat bitta** `/products` so‘rovi yuborilishi mumkin.  
- `API-key` xavfsiz joyda saqlanishi kerak — uni ommaga e’lon qilmang.  
- `delivery_info.distance` — metrda, `delivery_fee` — so‘mda ko‘rsatiladi.

---

**© 2025 Marketplace API – All rights reserved**
