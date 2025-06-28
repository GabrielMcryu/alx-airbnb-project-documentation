# Task 5

# 📌 Backend Feature Requirement Specifications

Detailed specifications for key backend features of the Airbnb Clone system.

---

## 1. 🔐 User Authentication

### ✅ Overview
Enables user registration and login using email and password. Supports role-based access control (`guest`, `host`, `admin`).

### 📣 API Endpoints

| Method | Endpoint      | Description           |
|--------|---------------|-----------------------|
| POST   | /api/register | Register a new user   |
| POST   | /api/login    | Login and get token   |

### 📥 Input Specifications

**POST /api/register**
```json
{
  "first_name": "Alice",
  "last_name": "Smith",
  "email": "alice@example.com",
  "password": "securePassword123",
  "role": "guest"
}
```

**POST /api/login**
```json
{
  "email": "alice@example.com",
  "password": "securePassword123"
}
```

### 📤 Output (Login Success)
```json
{
  "acess_token": "jwt_token_string",
  "refresh_token": "jwt_token_string",
  "user": {
    "user_id": "uuid",
    "email": "alice@example.com",
    "role": "guest"
  }
}
```

### ✅ Validation Rules
- Unique and valid email format
- Secure password hashing
- Role must be `guest`, `host`, or `admin`

## 2. 🏠 Property Management (Host)

### ✅ Overview
Allows hosts to manage property listings with details like price, location, and description.

### 📣 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/properties | Create a new listing (Host) |
| GET | /api/properties | View all listings |
| GET | /api/properties/{id} | View specific listing |
| PUT | /api/properties/{id} | Edit a listing (Host only) |
| DELETE | /api/properties/{id} | Delete a listing (Host only) |

### 📥 Input Example
```json
{
  "name": "Sea View Cottage",
  "description": "Beautiful cottage with ocean view",
  "location": "Mombasa, Kenya",
  "price_per_night": 75.00
}
```

### 📤 Output Example
```json
{
  "property_id": "uuid",
  "name": "Sea View Cottage",
  "price_per_night": 75.00
}
```

### ✅ Validation Rules
- Only `host` can create/update/delete
- Price must be a positive number
- Required fields: name, description, location

## 3. 📅 Booking System (Guest + Host)

### ✅ Overview
Enables guests to book properties and hosts to view/manage bookings.

### 📣 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/bookings | Guest creates booking |
| GET | /api/bookings | View own bookings (guest/host) |
| DELETE | /api/bookings/{id} | Cancel booking (guest/host) |

### 📥 Input Example (POST)
```json
{
  "property_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-03"
}
```

### 📤 Output Example
```json
{
  "booking_id": "uuid",
  "status": "confirmed",
  "total_price": 150.00
}
```

### ✅ Validation Rules
- Dates must not overlap with existing bookings
- Start date must be before end date
- Only guests can create bookings

## 4. 💳 Payment Feature

### ✅ Overview
Processes payments for confirmed bookings using external gateways like Stripe or PayPal.

### 📣 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/payments | Process payment (guest) |
| GET | /api/payments | View payments (admin/host) |

### 📥 Input Example
```json
{
  "booking_id": "uuid",
  "payment_method": "credit_card"
}
```

### 📤 Output Example
```json
{
  "payment_id": "uuid",
  "status": "successful",
  "amount": 150.00
}
```

### ✅ Validation Rules
- Only confirmed bookings are payable
- Only guests can make payments
- Payment method must be one of: `credit_card`, `paypal`, `stripe`

## 5. 🛠️ Admin Management

### ✅ Overview
Admins can manage users, listings, reviews, and bookings from a centralized dashboard.

### 📣 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/admin/users | List all users |
| PUT | /api/admin/users/{id} | Update user role/status |
| DELETE | /api/admin/users/{id} | Delete user |
| GET | /api/admin/reports | View system logs and metrics |

### 📤 Output Example (GET /api/admin/users)
```json
[
  {
    "user_id": "uuid",
    "email": "admin@example.com",
    "role": "admin",
    "status": "active"
  }
]
```

### ✅ Validation Rules
- Only users with role `admin` can access these endpoints
- Admins cannot delete themselves
- All actions must be audited/logged