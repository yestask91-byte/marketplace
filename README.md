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
POST /api/v1/marketplace/order
```

---

### **🔖 Headers**
| Name | Type | Required | Description |
|------|------|-----------|-------------|
| API-Key | string | ✅ | Partner tomonidan berilgan API kaliti |
| AppName | string | ✅ | Ilova nomi (masalan: `YesPOS`) |
| Content-Type | string | ✅ | `application/json` |

---

### **📦 Request Body**
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

---

### **🧠 Body Parametr Izohi**
| Field | Type | Required | Description |
|--------|------|-----------|-------------|
| `type` | int | ✅ | 1 — **Order (buyurtma)**, 2 — **Refund (qaytarish)** |
| `note` | string | ❌ | Buyurtma haqida eslatma (ixtiyoriy) |
| `delivery_info` | object / null | ❌ | Yetkazib berish haqida ma’lumot (bo‘lishi shart emas) |
| `delivery_info.scheduled_time` | string | ❌ | Rejalashtirilgan vaqt (bo‘lishi mumkin yoki bo‘lmasligi mumkin) |
| `delivery_info.distance` | number | ❌ | Masofa (metrda) |
| `delivery_info.delivery_fee` | number | ❌ | Yetkazib berish summasi |
| `items` | array | ✅ | Buyurtma ichidagi mahsulotlar ro‘yxati |
| `items[].item` | int | ✅ | Mahsulot ID |
| `items[].qty` | number | ✅ | Mahsulot soni |
| `items[].price` | number | ✅ | Mahsulot bir dona narxi |
| `payments` | array | ✅ | To‘lov usullari ro‘yxati |
| `payments[].payment_id` | int | ✅ | To‘lov turi:<br>• 1 — Naqd pul<br>• 2 — Bank karta<br>• 4 — Click<br>• 5 — Payme |
| `payments[].value` | number | ✅ | To‘lov summasi |

---

### ⚠️ **Business Qoidalar**
1. Agar `delivery_info` kerak bo‘lmasa — uni `null` yoki umuman yubormaslik mumkin.  
2. `type = 1` — bu **order (buyurtma)**, `type = 2` — bu **refund (qaytarish)**.  
3. `items` ichidagi barcha `qty * price` yig‘indisi **`payments` dagi `value` lar yig‘indisiga teng** bo‘lishi **shart**.  
4. Xatolik bo‘lsa, server JSON formatda xabar yuboradi.  

---

### **✅ Success Response**
```json
{
  "code": 0,
  "message": "OK",
  "data": "Successfully created!"
}
```

---

### **❌ Error Response**
```json
{
  "error": "Har 1 daqiqada faqat 1 ta so‘rov yuborish mumkin. Qolgan vaqt: 45s"
}
```
yoki
```json
{
  "error": "API-Key header topilmadi"
}
```

---

### **🧩 Misol (curl bilan)**
```bash
curl --location 'https://base_url/api/v1/marketplace/order' --header 'API-Key: demo-CI6IkpXVCJ9.eyJhdXRob3' --header 'AppName: YesPOS' --header 'Content-Type: application/json' --data '{
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


## **4. Webhook: Qoldiq va Narx yangilanishi**

Webhook URL **partner tomonidan taqdim etiladi.**
Tizim mahsulot narxi yoki qoldig‘i o‘zgarganda, avtomatik quyidagi JSON yuboradi.

### Endpoint
`POST {partner_webhook_url}`

### Request Body
```json
{
    "api_key": "demo-CI6IkpXVCJ9.eyJhdXRob3",
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

### ✅ Success Response
```json
{
    "status": "success",
    "message": "Webhook received successfully"
}
```

### ❌ Error Response
```json
{
    "error": "Invalid API key"
}
```
