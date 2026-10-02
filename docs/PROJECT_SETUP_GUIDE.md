# 🚀 Project Setup & Execution Guide

This guide provides step-by-step instructions for configuring, running, and demonstrating the **Mini E-Commerce Demo Project (MERN Stack)**.

---

## 1. Prerequisites

Ensure you have the following installed on your development machine:
- **Node.js**: `v18.x` or higher ([Download](https://nodejs.org/))
- **npm**: `v9.x` or higher (bundled with Node.js)
- **MongoDB**: Either a local instance running on `mongodb://localhost:27017` or a free cloud cluster on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
- **Git**: For version control.

---

## 2. Environment Variables Configuration

### 2.1 Backend Environment (`server/.env`)

Create a `.env` file inside the `server/` directory:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# MongoDB Connection String
MONGO_URI=mongodb://localhost:27017/mini-ecommerce

# JWT Secret Key
JWT_SECRET=super_secret_jwt_demo_key_2026_change_in_production

# Optional Initial Admin Seed Credentials
ADMIN_NAME=Admin User
ADMIN_EMAIL=admin@store.com
ADMIN_PASSWORD=adminpassword123
```

### 2.2 Frontend Environment (`client/.env`)

Create a `.env` file inside the `client/` directory:

```env
# Backend API Base URL
VITE_API_BASE_URL=http://localhost:5000/api
```

---

## 3. Installation & Local Execution

### 3.1 Backend Setup (`/server`)

1. Open your terminal and navigate to the `server/` directory:
   ```bash
   cd server
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server (with nodemon):
   ```bash
   npm run dev
   # Server will start on http://localhost:5000
   ```

---

### 3.2 Frontend Setup (`/client`)

1. In a new terminal window, navigate to the `client/` directory:
   ```bash
   cd client
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the Vite development server:
   ```bash
   npm run dev
   # Vite will start on http://localhost:5173
   ```

---

## 4. End-to-End Demo Walkthrough Checklist

Follow this checklist to verify that all requirements work end-to-end:

| Step | Action | Expected Result |
| :---: | :--- | :--- |
| **1** | **Admin Login** | Log in at `/login` with admin credentials (`admin@store.com` / `adminpassword123`). Admin dashboard options become accessible in navigation. |
| **2** | **Add Categories** | Navigate to `/admin/categories`. Click "Add Category". Create: `Electronics`, `Fashion`, and `Shoes`. |
| **3** | **Add Products** | Navigate to `/admin/products`. Click "Add Product". Create products with title, price, stock (e.g. 10), image URL, and assigned category. |
| **4** | **Check Storefront** | Navigate to `/products`. The newly created products appear immediately in the responsive grid. |
| **5** | **Filter & Search** | Click category pills (`All`, `Electronics`, `Fashion`, `Shoes`). Type in search bar to filter by product name. |
| **6** | **Register Customer** | Log out of admin, navigate to `/register`. Register a new customer (`alice@example.com` / `password123`). |
| **7** | **Add to Cart** | Open product details or click "Add to Cart". Verify that the cart badge updates. In `/cart`, verify stock limits prevent quantity > available stock. |
| **8** | **Checkout (COD)** | Navigate to `/checkout`. Fill in `Name`, `Phone`, `Address`, `City`, `Pincode`. Select Cash on Delivery. Click "Place Order". |
| **9** | **Stock Reduced** | Order is saved in MongoDB. Cart is cleared. Redirects to `/my-orders` displaying order in `Pending` status. Stock in DB is decremented. |
| **10** | **Admin Verification** | Log back in as Admin. Navigate to `/admin/orders`. The customer's order is visible with items and address. |
| **11** | **Update Status** | Change status from `Pending` ➔ `Confirmed` ➔ `Shipped` ➔ `Delivered`. Customer's `/my-orders` reflects the updated status. |

---

## 5. Troubleshooting & FAQ

- **MongoDB Connection Error**: Verify your `MONGO_URI` in `server/.env`. If using local MongoDB, ensure the service is running (`net start MongoDB` on Windows or `sudo systemctl start mongod` on Linux).
- **CORS Issues**: Ensure the backend allows requests from `http://localhost:5173`.
- **401 Unauthorized during API calls**: Ensure token is stored in `localStorage` and attached via Axios request interceptor (`Authorization: Bearer <token>`).
