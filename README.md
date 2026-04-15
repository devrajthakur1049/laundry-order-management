 Mini Laundry Order Management System

A lightweight, AI-first order management system for dry cleaning stores. Built with Node.js + Express and an in-memory store. Includes a full frontend UI served from the same server.

---
Setup Instructions

### Prerequisites
- Node.js v16 or later
- npm

### Install & Run

```bash
# 1. Clone / unzip the project
cd laundry-order-management

# 2. Install dependencies
npm install

# 3. Start the server
npm start
# OR for development with auto-reload:
npm run dev
```

Server runs at: **http://localhost:3000**

- **Frontend UI**: http://localhost:3000/
- **API base**: http://localhost:3000/api

---

 Features Implemented

| Feature | Status |
|---|---|
| Create Order (name, phone, garments) | ✅ |
| Auto-calculate total bill | ✅ |
| Unique Order ID (UUID) | ✅ |
| Order status lifecycle (RECEIVED → PROCESSING → READY → DELIVERED) | ✅ |
| Update order status via API | ✅ |
| List all orders | ✅ |
| Filter by status / customer name / phone | ✅ |
| Search by garment type | ✅ (bonus) |
| Dashboard: total orders, revenue, orders per status | ✅ |
| Dashboard: popular garments, average order value | ✅ (bonus) |
| Estimated delivery date (3 days from order) | ✅ (bonus) |
| Full frontend UI | ✅ (bonus) |
| Price list endpoint | ✅ |
| Proper error handling + validation | ✅ |

---

## 📡 API Endpoints

### `POST /api/orders` — Create Order
```json
// Request body
{
  "customerName": "Priya Sharma",
  "phoneNumber": "9876543210",
  "garments": [
    { "type": "shirt", "quantity": 3 },
    { "type": "saree", "quantity": 2 }
  ]
}

// Response 201
{
  "message": "Order created successfully.",
  "orderId": "uuid-here",
  "totalBill": 70,
  "estimatedDelivery": "2025-01-18",
  "order": { ... }
}
```

### `PUT /api/orders/:id/status` — Update Status
```json
// Request body
{ "status": "PROCESSING" }

// Response 200
{
  "message": "Order status updated to PROCESSING.",
  "orderId": "...",
  "status": "PROCESSING",
  "updatedAt": "..."
}
```

### `GET /api/orders` — List Orders
```
Query params (all optional):
  ?status=RECEIVED
  ?customerName=priya
  ?phoneNumber=9876
  ?garmentType=shirt
```

### `GET /api/orders/:id` — Get Single Order

### `GET /api/dashboard` — Dashboard Stats
```json
{
  "totalOrders": 10,
  "totalRevenue": 850,
  "ordersByStatus": { "RECEIVED": 3, "PROCESSING": 2, "READY": 1, "DELIVERED": 4 },
  "recentOrders": [...],
  "popularGarments": { "shirt": 12, "saree": 8 },
  "averageOrderValue": 85
}
```

### `GET /api/prices` — Price List
```json
{
  "prices": { "shirt": 10, "pants": 15, "saree": 20, ... }
}
```

---





### Sample Prompts Used

1. *"Build a complete Mini Laundry Order Management System using Node.js + Express. In-memory storage. POST /orders, PUT /orders/:id/status, GET /orders, GET /dashboard. Use UUID for order IDs. Clean folder structure: controllers/, routes/, models/, utils/."*

2. *"Add garment-type search filter to the GET /orders endpoint"*

3. *"Generate a polished dark-themed HTML frontend for this system with dashboard stats, order list with filters, and a new order form. Use Google Fonts. Single file."*

4. *"Add estimated delivery date (3 business days) and include it in order creation response"*

---

### Where AI Helped
- ✅ Scaffolded full folder structure and boilerplate instantly
- ✅ Generated validation logic for all inputs
- ✅ Wrote the dashboard aggregation logic (reduce, filter, sort)
- ✅ Built the entire frontend UI (CSS variables, dark theme, responsive grid)
- ✅ Wrote the README template

---

### What AI Got Wrong / Needed Fixes
- ❌ Initially used `req.body.garment` (singular) instead of `garments` (array) — fixed to match spec
- ❌ Status validation used `===` without `.toUpperCase()` — caused "processing" to fail validation; fixed
- ❌ Frontend fetch URL was missing `/api` prefix — corrected
- ❌ Bill preview didn't update live on garment row deletion — added `removeRow()` to call `updateBillPreviewLive()`
- ❌ Dashboard "pending" count was just `RECEIVED` — corrected to sum RECEIVED + PROCESSING + READY

---

### What Was Improved Manually
- Added backward-status warning in PUT /status (instead of blocking — store flexibility)
- Extended price list beyond the 3 required garments (jacket, suit, kurta, lehenga, blouse)
- Added `GET /api/orders/:id` single-order endpoint (not in spec, but practically needed)
- Added `GET /api/prices` endpoint for the frontend to dynamically render the garment selector
- Frontend live bill preview updates on every garment change without a separate "preview" click

---

## ⚖️ Tradeoffs

### What Was Skipped
- **Database (MongoDB/SQL)** — in-memory is sufficient for the 72hr scope; data resets on restart
- **Authentication** — no login/JWT; acceptable for a local store tool
- **Pagination** on order list — not needed at this scale
- **Unit tests** — skipped for time; would add Jest tests on controller functions next

### What Would Be Improved With More Time
- Persist data to MongoDB (just swap `orders` array for a Mongoose model)
- Add JWT auth with a simple store-owner login
- Export orders to CSV / PDF invoice
- SMS notifications when order status changes (Twilio)
- Deploy to Railway or Render with a one-click Dockerfile

---

## 📁 Folder Structure

```
laundry-order-management/
├── server.js              # Express app entry point
├── package.json
├── controllers/
│   └── orderController.js # All route handler functions
├── routes/
│   └── orderRoutes.js     # Express Router definitions
├── models/
│   └── orderStore.js      # In-memory store + PRICES + STATUSES
├── utils/
│   └── helpers.js         # calculateBill, getEstimatedDelivery, isValidPhone
└── public/
    └── index.html         # Full frontend UI (served statically)
```
