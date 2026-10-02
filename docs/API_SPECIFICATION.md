# 📡 REST API Specification

This document details the complete REST API contract for the **Mini E-Commerce Demo Project**.

---

## 1. General Conventions

- **Base URL**: `http://localhost:5000/api`
- **Content-Type**: `application/json`
- **Authentication**: Sent via standard HTTP Header:
  ```http
  Authorization: Bearer <JWT_TOKEN>
  ```
- **Standard Error Format**:
  ```json
  {
    "success": false,
    "message": "Error description message"
  }
  ```

---

## 2. Authentication Endpoints (`/api/auth`)

### 2.1 Register Customer
Register a new customer account.

- **Method**: `POST`
- **Endpoint**: `/api/auth/register`
- **Access**: Public
- **Request Body**:
  ```json
  {
    "name": "Jane Doe",
    "email": "jane@example.com",
    "password": "secretPassword123",
    "confirmPassword": "secretPassword123"
  }
  ```
- **Validation**:
  - `name`: Required, trimmed.
  - `email`: Required, valid email format, unique.
  - `password`: Minimum 6 characters.
  - `confirmPassword`: Must match `password`.
- **Response `201 Created`**:
  ```json
  {
    "success": true,
    "message": "User registered successfully",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "65b9e078a01f9c0012345678",
      "name": "Jane Doe",
      "email": "jane@example.com",
      "role": "customer"
    }
  }
  ```
- **Errors**: `400 Bad Request` (Validation errors or passwords mismatch), `409 Conflict` (Email already registered).

---

### 2.2 Login User (Customer or Admin)
Authenticate with email and password to receive a JWT token.

- **Method**: `POST`
- **Endpoint**: `/api/auth/login`
- **Access**: Public
- **Request Body**:
  ```json
  {
    "email": "jane@example.com",
    "password": "secretPassword123"
  }
  ```
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "message": "Login successful",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "_id": "65b9e078a01f9c0012345678",
      "name": "Jane Doe",
      "email": "jane@example.com",
      "role": "customer"
    }
  }
  ```
- **Errors**: `400 Bad Request` (Missing fields), `401 Unauthorized` (Invalid credentials).

---

## 3. Category Endpoints (`/api/categories`)

### 3.1 Get All Categories
Retrieve list of all categories.

- **Method**: `GET`
- **Endpoint**: `/api/categories`
- **Access**: Public
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "count": 3,
    "categories": [
      {
        "_id": "65b9e101a01f9c0012345671",
        "name": "Electronics",
        "description": "Gadgets, smartphones, and accessories",
        "createdAt": "2026-10-02T10:00:00.000Z"
      },
      {
        "_id": "65b9e102a01f9c0012345672",
        "name": "Fashion",
        "description": "Apparel, clothing, and wear",
        "createdAt": "2026-10-02T10:05:00.000Z"
      }
    ]
  }
  ```

---

### 3.2 Create Category
Create a new category.

- **Method**: `POST`
- **Endpoint**: `/api/categories`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Request Body**:
  ```json
  {
    "name": "Shoes",
    "description": "Footwear, sneakers, and running shoes"
  }
  ```
- **Response `201 Created`**:
  ```json
  {
    "success": true,
    "message": "Category created successfully",
    "category": {
      "_id": "65b9e103a01f9c0012345673",
      "name": "Shoes",
      "description": "Footwear, sneakers, and running shoes",
      "createdAt": "2026-10-02T10:15:00.000Z"
    }
  }
  ```
- **Errors**: `400 Bad Request` (Name required), `403 Forbidden` (Not an admin).

---

### 3.3 Update Category
Edit an existing category.

- **Method**: `PUT`
- **Endpoint**: `/api/categories/:id`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Request Body**:
  ```json
  {
    "name": "Footwear & Shoes",
    "description": "Updated description for shoes"
  }
  ```
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "message": "Category updated successfully",
    "category": {
      "_id": "65b9e103a01f9c0012345673",
      "name": "Footwear & Shoes",
      "description": "Updated description for shoes"
    }
  }
  ```

---

### 3.4 Delete Category
Remove a category.

- **Method**: `DELETE`
- **Endpoint**: `/api/categories/:id`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "message": "Category deleted successfully"
  }
  ```
- **Errors**: `404 Not Found`, `403 Forbidden`.

---

## 4. Product Endpoints (`/api/products`)

### 4.1 Get All Products (Filter & Search)
Retrieve list of products with optional category and search filters.

- **Method**: `GET`
- **Endpoint**: `/api/products`
- **Access**: Public
- **Query Parameters**:
  - `category` (optional): Category Name or Category ID (e.g. `electronics`)
  - `search` (optional): Partial keyword search against product name (e.g. `phone`)
  - *Example*: `GET /api/products?category=electronics&search=phone`
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "count": 1,
    "products": [
      {
        "_id": "65b9e201a01f9c0012345680",
        "name": "Wireless Smartphone Pro",
        "description": "Flagship 5G smartphone with 120Hz OLED display",
        "price": 699.99,
        "image": "https://images.unsplash.com/photo-1511707171634-5f897ff02aa9",
        "category": {
          "_id": "65b9e101a01f9c0012345671",
          "name": "Electronics"
        },
        "stock": 25,
        "createdAt": "2026-10-02T11:00:00.000Z"
      }
    ]
  }
  ```

---

### 4.2 Get Product By ID
Retrieve complete details for a single product.

- **Method**: `GET`
- **Endpoint**: `/api/products/:id`
- **Access**: Public
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "product": {
      "_id": "65b9e201a01f9c0012345680",
      "name": "Wireless Smartphone Pro",
      "description": "Flagship 5G smartphone with 120Hz OLED display",
      "price": 699.99,
      "image": "https://images.unsplash.com/photo-1511707171634-5f897ff02aa9",
      "category": {
        "_id": "65b9e101a01f9c0012345671",
        "name": "Electronics"
      },
      "stock": 25
    }
  }
  ```
- **Errors**: `404 Not Found`.

---

### 4.3 Create Product
Add a new product to inventory.

- **Method**: `POST`
- **Endpoint**: `/api/products`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Request Body**:
  ```json
  {
    "name": "Running Sport Sneakers",
    "description": "Ergonomic breathable running shoes",
    "price": 89.99,
    "image": "https://images.unsplash.com/photo-1542291026-7eec264c27ff",
    "category": "65b9e103a01f9c0012345673",
    "stock": 40
  }
  ```
- **Validation**:
  - `name`: Required
  - `price`: Required, numeric, > 0
  - `stock`: Required, integer, >= 0
  - `category`: Required, valid MongoDB ObjectId
  - `image`: Required URL string
- **Response `201 Created`**:
  ```json
  {
    "success": true,
    "message": "Product created successfully",
    "product": {
      "_id": "65b9e202a01f9c0012345681",
      "name": "Running Sport Sneakers",
      "price": 89.99,
      "stock": 40
    }
  }
  ```

---

### 4.4 Update Product
Edit an existing product.

- **Method**: `PUT`
- **Endpoint**: `/api/products/:id`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Request Body**: Any subset of `name`, `description`, `price`, `image`, `category`, `stock`.
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "message": "Product updated successfully",
    "product": { ... }
  }
  ```

---

### 4.5 Delete Product
Delete a product from the database.

- **Method**: `DELETE`
- **Endpoint**: `/api/products/:id`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "message": "Product deleted successfully"
  }
  ```

---

## 5. Order Endpoints (`/api/orders` & `/api/admin/orders`)

### 5.1 Place Order (Checkout)
Create an order via Cash on Delivery. Prices are looked up from MongoDB; stock is validated and decremented.

- **Method**: `POST`
- **Endpoint**: `/api/orders`
- **Access**: Authenticated Customer (`Authorization: Bearer <TOKEN>`)
- **Request Body**:
  ```json
  {
    "items": [
      {
        "productId": "65b9e201a01f9c0012345680",
        "quantity": 2
      },
      {
        "productId": "65b9e202a01f9c0012345681",
        "quantity": 1
      }
    ],
    "shippingAddress": {
      "name": "Jane Doe",
      "phone": "+1987654321",
      "address": "123 Market Street, Apt 4B",
      "city": "Metropolis",
      "pincode": "10001"
    }
  }
  ```
- **Response `201 Created`**:
  ```json
  {
    "success": true,
    "message": "Order placed successfully",
    "order": {
      "_id": "65b9e301a01f9c0012345690",
      "user": "65b9e078a01f9c0012345678",
      "products": [
        {
          "product": "65b9e201a01f9c0012345680",
          "name": "Wireless Smartphone Pro",
          "price": 699.99,
          "quantity": 2
        },
        {
          "product": "65b9e202a01f9c0012345681",
          "name": "Running Sport Sneakers",
          "price": 89.99,
          "quantity": 1
        }
      ],
      "totalAmount": 1489.97,
      "shippingAddress": {
        "name": "Jane Doe",
        "phone": "+1987654321",
        "address": "123 Market Street, Apt 4B",
        "city": "Metropolis",
        "pincode": "10001"
      },
      "status": "Pending",
      "createdAt": "2026-10-02T12:00:00.000Z"
    }
  }
  ```
- **Errors**:
  - `400 Bad Request`: Empty cart items, missing address fields, or requested quantity exceeds available stock.
  - `404 Not Found`: One or more product IDs do not exist.

---

### 5.2 Get Customer Orders ("My Orders")
Retrieve all orders placed by the authenticated customer.

- **Method**: `GET`
- **Endpoint**: `/api/orders/my-orders`
- **Access**: Authenticated Customer (`Authorization: Bearer <TOKEN>`)
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "count": 1,
    "orders": [
      {
        "_id": "65b9e301a01f9c0012345690",
        "totalAmount": 1489.97,
        "status": "Pending",
        "products": [ ... ],
        "shippingAddress": { ... },
        "createdAt": "2026-10-02T12:00:00.000Z"
      }
    ]
  }
  ```

---

### 5.3 Get All Orders (Admin)
Retrieve all orders across all customers.

- **Method**: `GET`
- **Endpoint**: `/api/admin/orders`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "count": 12,
    "orders": [
      {
        "_id": "65b9e301a01f9c0012345690",
        "user": {
          "_id": "65b9e078a01f9c0012345678",
          "name": "Jane Doe",
          "email": "jane@example.com"
        },
        "totalAmount": 1489.97,
        "status": "Pending",
        "products": [ ... ],
        "shippingAddress": { ... },
        "createdAt": "2026-10-02T12:00:00.000Z"
      }
    ]
  }
  ```

---

### 5.4 Update Order Status (Admin)
Change the fulfillment status of an order.

- **Method**: `PATCH`
- **Endpoint**: `/api/admin/orders/:id/status`
- **Access**: Admin only (`Authorization: Bearer <TOKEN>`)
- **Request Body**:
  ```json
  {
    "status": "Shipped"
  }
  ```
- **Allowed Status Values**:
  - `"Pending"`
  - `"Confirmed"`
  - `"Shipped"`
  - `"Delivered"`
  - `"Cancelled"`
- **Response `200 OK`**:
  ```json
  {
    "success": true,
    "message": "Order status updated successfully",
    "order": {
      "_id": "65b9e301a01f9c0012345690",
      "status": "Shipped"
    }
  }
  ```
- **Errors**: `400 Bad Request` (Invalid status value), `404 Not Found` (Order not found).
