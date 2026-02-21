# OpsAgent API Integration Guide (n8n <-> Backend)

> **For the Team:** This document details how the n8n workflow interacts with our Backend API. Use this to ensure the Frontend displays the data correctly and the Backend endpoints remain compatible.

---

## 1. High-Level Architecture
**n8n** acts as the **Intelligence Middleware**. It does not store data itself; it processes incoming messages and pushes structured data to the **FastAPI Backend**, which then updates **Supabase**.

1.  **Source**: WhatsApp / Gmail (Webhook)
2.  **Processor**: Gemini AI (files JSON)
3.  **Destination**: OpsAgent Backend (`http://localhost:8000/api/...`)

---

## 2. API Endpoints Used by n8n (The Contracts)

### A. Customer Management (`/api/customers`)
*Used by Order Bot to find or create customers.*

**1. Lookup Customer (GET)**
-   **URL**: `/api/customers?search={name_or_phone}`
-   **Response**: Returns list of matching customers.

**2. Create Customer (POST)**
-   **URL**: `/api/customers`
-   **Body**:
    ```json
    {
      "name": "Amit Kumar",
      "phone": "+919876543210",
      "email": "amit@example.com", // Optional
      "source": "n8n_auto"
    }
    ```
-   **Response**: Returns `{ "id": "uuid...", ... }`

---

### B. Order Processing (`/api/orders`)
*Used by Order Bot to create new pending orders.*

**Create Order (POST)**
-   **URL**: `/api/orders`
-   **Body**:
    ```json
    {
      "customer_id": "uuid-of-customer",
      "items": [
        { "name": "Blue Shirt", "quantity": 2, "price": 500 },
        { "name": "Jeans", "quantity": 1, "price": 1200 }
      ],
      "total": 0, // Backend calculates this if 0, or AI can estimate
      "status": "pending",
      "source": "n8n_ai",
      "notes": "Original Message: I want 2 blue shirts..."
    }
    ```

---

### C. Payment Logging (`/api/payments`)
*Used by Payment Bot to record transactions.*

**Log Payment (POST)**
-   **URL**: `/api/payments`
-   **Body**:
    ```json
    {
      "customer_id": "uuid-of-customer", // Optional, links to Guest if null
      "amount": 500,
      "status": "received",
      "source": "n8n_ai", // "upi", "cash"
      "method": "unknown" // AI tries to extract this
    }
    ```

---

### D. Alerts & Complaints (`/api/dashboard/alerts`)
*Used by Complaint Bot and Query Bot to notify the user.*

**Raise Alert (POST)**
-   **URL**: `/api/dashboard/alerts`
-   **Body**:
    ```json
    {
      "type": "complaint", // or "query"
      "severity": "critical", // "info" for queries
      "message": "Customer received damaged goods."
    }
    ```

---

## 3. Data Flow Example

**Scenario: User sends "Send 5 boxes of pens to VSIT"**

1.  **n8n Webhook** receives text.
2.  **Gemini** extracts:
    -   Intent: `order`
    -   Customer: `VSIT`
    -   Items: `[{ "name": "pens", "quantity": 5 }]`
3.  **n8n** calls `GET /api/customers?search=VSIT`.
    -   *Found?* Use ID.
    -   *Not Found?* Call `POST /api/customers` -> Get new ID.
4.  **n8n** calls `POST /api/orders` with that Customer ID and Items.
5.  **Backend** saves to Supabase `orders` table.
6.  **Frontend** (Dashboard) auto-refreshes (via polling or subscription) and shows the new Order.

---

## 4. Frontend Implementation Notes
-   **Polling**: The Dashboard should poll `/api/dashboard` or specific lists (`/api/orders`) every 30-60 seconds to catch n8n updates.
-   **Alerts**: The `alerts` table is the source of truth for complaints. Display these prominently.
-   **Source Tagging**: UI should show "Via WhatsApp" tag if `source == 'n8n_ai'` or `source == 'whatsapp'`.
