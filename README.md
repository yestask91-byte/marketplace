# 🧾 API Documentation

## 📦 Base URL
```
base_url/api/v1
```

---

## 🛒 **1. Get Marketplace Products**

**Endpoint:**  
```
POST /marketplace/products
```

**Headers:**
| Name | Type | Required | Description |
|------|------|-----------|-------------|
| API-key | string | ✅ | API kaliti |
| Content-Type | string | ✅ | `application/json` |

**Request Body:**
```json
{}
```

**✅ Success Response:**
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

**❌ Error Responses:**
```json
{
  "error": "Har 1 daqiqada faqat 1 ta so‘rov yuborish mumkin. Qolgan vaqt: 19s"
}
```
```json
{
  "error": "API-Key header topilmadi"
}
```

---

## 🧾 **2. Create Marketplace Order**

**Endpoint:**  
```
POST /marketplace/order
```

**Headers:**
| Name | Type | Required | Description |
|------|------|-----------|-------------|
| API-Key | string | ✅ | API kaliti |
| AppName | string | ✅ | Ilova nomi |
| Content-Type | string | ✅ | `application/json` |

**Request Body:**
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

**✅ Success Response:**
```json
{
  "code": 0,
  "message": "OK",
  "data": "Successfully created!"
}
```

---

## 🏬 **3. Branch List**

**Endpoint:**  
```
POST /branch/list
```

**Headers:**
| Name | Type | Required | Description |
|------|------|-----------|-------------|
| Content-Type | string | ✅ | `application/json` |

**Request Body:**
```json
{}
```

**✅ Success Response:**
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

**❌ Error Response:**
```json
{
  "error": "Har 1 daqiqada faqat 1 ta so‘rov yuborish mumkin. Qolgan vaqt: 57s"
}
```

---
```
