# API DOCUMENTATION – CAB SYSTEM

> **Project:** CAB System
> **Version:** 1.0.0
> **API Type:** RESTful API
> **Data Format:** JSON
> **Authentication:** Bearer Token (JWT)

---

# TABLE OF CONTENTS

1. [Overview](#1-overview)
2. [General Conventions](#2-general-conventions)
3. [Authentication](#3-authentication-api)
4. [Customer API](#4-customer-api)
5. [Driver API](#5-driver-api)
6. [Vehicle API](#6-vehicle-api)
7. [Ride API](#7-ride-api)
8. [Driver Assignment API](#8-driver-assignment-api)
9. [Trip Management API](#9-trip-management-api)
10. [Location API](#10-location-api)
11. [Fare API](#11-fare-api)
12. [Payment API](#12-payment-api)
13. [Transaction API](#13-transaction-api)
14. [Notification API](#14-notification-api)
15. [Rating API](#15-rating-api)
16. [Report API](#16-report-api)
17. [Admin API](#17-admin-api)
18. [Error Codes](#18-error-codes)
19. [API Endpoint Summary](#19-api-endpoint-summary)

---

# 1. OVERVIEW

## 1.1. Base URL

```text
http://localhost:8080/api/v1
```

Production:

```text
https://api.cabsystem.com/api/v1
```

---

## 1.2. Content Type

All requests and responses use:

```http
Content-Type: application/json
```

---

## 1.3. Authentication

Protected APIs require a JWT token.

```http
Authorization: Bearer <access_token>
```

Example:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## 1.4. Standard Success Response

```json
{
  "success": true,
  "message": "Request successful",
  "data": {}
}
```

---

## 1.5. Standard Error Response

```json
{
  "success": false,
  "error_code": "VALIDATION_ERROR",
  "message": "Invalid request data"
}
```

---

## 1.6. Pagination Response

```json
{
  "success": true,
  "data": {
    "items": [],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 100,
      "total_pages": 10
    }
  }
}
```

---

# 2. GENERAL CONVENTIONS

## 2.1. HTTP Methods

| Method | Description             |
| ------ | ----------------------- |
| GET    | Retrieve data           |
| POST   | Create data             |
| PUT    | Update entire resource  |
| PATCH  | Update part of resource |
| DELETE | Delete resource         |

---

## 2.2. HTTP Status Codes

| Code | Description           |
| ---- | --------------------- |
| 200  | Success               |
| 201  | Created successfully  |
| 204  | Deleted successfully  |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 409  | Conflict              |
| 422  | Validation Error      |
| 500  | Internal Server Error |

---

## 2.3. User Roles

| Role     | Description     |
| -------- | --------------- |
| CUSTOMER | Customer        |
| DRIVER   | Driver          |
| ADMIN    | Administrator   |
| OPERATOR | System Operator |

---

# 3. AUTHENTICATION API

---

## 3.1. Register Customer

### Endpoint

```http
POST /auth/register
```

### Description

Create a new customer account.

### Request Body

```json
{
  "full_name": "Nguyen Van A",
  "phone": "0901234567",
  "email": "nguyenvana@gmail.com",
  "password": "12345678"
}
```

### Parameters

| Field     | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| full_name | string | Yes      | Customer full name |
| phone     | string | Yes      | Phone number       |
| email     | string | Yes      | Email address      |
| password  | string | Yes      | Password           |

### Success Response

**HTTP 201**

```json
{
  "success": true,
  "message": "Registration successful",
  "data": {
    "user_id": 1,
    "full_name": "Nguyen Van A",
    "phone": "0901234567",
    "email": "nguyenvana@gmail.com",
    "role": "CUSTOMER"
  }
}
```

### Errors

| Code | Error Code   | Description          |
| ---- | ------------ | -------------------- |
| 400  | INVALID_DATA | Invalid request      |
| 409  | PHONE_EXISTS | Phone already exists |
| 409  | EMAIL_EXISTS | Email already exists |

---

## 3.2. Login

### Endpoint

```http
POST /auth/login
```

### Request Body

```json
{
  "phone": "0901234567",
  "password": "12345678"
}
```

### Success Response

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIs...",
    "token_type": "Bearer",
    "user": {
      "user_id": 1,
      "full_name": "Nguyen Van A",
      "role": "CUSTOMER"
    }
  }
}
```

### Errors

| Code | Error Code          |
| ---- | ------------------- |
| 401  | INVALID_CREDENTIALS |
| 404  | USER_NOT_FOUND      |

---

## 3.3. Get Current User

### Endpoint

```http
GET /auth/me
```

### Header

```http
Authorization: Bearer <access_token>
```

### Response

```json
{
  "success": true,
  "data": {
    "user_id": 1,
    "full_name": "Nguyen Van A",
    "phone": "0901234567",
    "email": "nguyenvana@gmail.com",
    "role": "CUSTOMER"
  }
}
```

---

## 3.4. Logout

### Endpoint

```http
POST /auth/logout
```

### Response

```json
{
  "success": true,
  "message": "Logout successful"
}
```

---

# 4. CUSTOMER API

---

## 4.1. Get Customer List

```http
GET /customers
```

### Authorization

```text
ADMIN
OPERATOR
```

### Query Parameters

| Parameter | Type    | Required | Description       |
| --------- | ------- | -------- | ----------------- |
| page      | integer | No       | Page number       |
| limit     | integer | No       | Number of records |
| keyword   | string  | No       | Search keyword    |

### Example

```http
GET /customers?page=1&limit=10
```

### Response

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "customer_id": 1,
        "full_name": "Nguyen Van A",
        "phone": "0901234567",
        "email": "nguyenvana@gmail.com"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 1,
      "total_pages": 1
    }
  }
}
```

---

## 4.2. Get Customer Detail

```http
GET /customers/{customer_id}
```

### Response

```json
{
  "success": true,
  "data": {
    "customer_id": 1,
    "full_name": "Nguyen Van A",
    "phone": "0901234567",
    "email": "nguyenvana@gmail.com"
  }
}
```

---

## 4.3. Update Customer

```http
PUT /customers/{customer_id}
```

### Request Body

```json
{
  "full_name": "Nguyen Van A",
  "email": "newemail@gmail.com"
}
```

### Response

```json
{
  "success": true,
  "message": "Customer updated successfully"
}
```

---

## 4.4. Delete Customer

```http
DELETE /customers/{customer_id}
```

### Response

```json
{
  "success": true,
  "message": "Customer deleted successfully"
}
```

---

# 5. DRIVER API

---

## 5.1. Create Driver

```http
POST /drivers
```

### Authorization

```text
ADMIN
```

### Request Body

```json
{
  "full_name": "Tran Van B",
  "phone": "0912345678",
  "email": "driver@gmail.com",
  "license_number": "B123456789",
  "password": "12345678"
}
```

### Response

```json
{
  "success": true,
  "message": "Driver created successfully",
  "data": {
    "driver_id": 1,
    "status": "OFFLINE"
  }
}
```

---

## 5.2. Get Driver List

```http
GET /drivers
```

### Query Parameters

| Parameter | Description       |
| --------- | ----------------- |
| page      | Page number       |
| limit     | Number of records |
| status    | Driver status     |
| keyword   | Search keyword    |

---

## 5.3. Get Driver Detail

```http
GET /drivers/{driver_id}
```

### Response

```json
{
  "success": true,
  "data": {
    "driver_id": 1,
    "full_name": "Tran Van B",
    "phone": "0912345678",
    "license_number": "B123456789",
    "status": "AVAILABLE"
  }
}
```

---

## 5.4. Update Driver

```http
PUT /drivers/{driver_id}
```

### Request Body

```json
{
  "full_name": "Tran Van B",
  "email": "newdriver@gmail.com"
}
```

---

## 5.5. Update Driver Status

```http
PATCH /drivers/{driver_id}/status
```

### Request Body

```json
{
  "status": "AVAILABLE"
}
```

### Driver Status

| Status    | Description       |
| --------- | ----------------- |
| AVAILABLE | Available         |
| BUSY      | Currently working |
| OFFLINE   | Offline           |

---

# 6. VEHICLE API

---

## 6.1. Create Vehicle

```http
POST /vehicles
```

### Request Body

```json
{
  "driver_id": 1,
  "license_plate": "59A-12345",
  "vehicle_type": "CAR",
  "brand": "Toyota",
  "model": "Vios"
}
```

### Response

```json
{
  "success": true,
  "message": "Vehicle created successfully",
  "data": {
    "vehicle_id": 1
  }
}
```

---

## 6.2. Get Vehicle List

```http
GET /vehicles
```

---

## 6.3. Get Vehicle Detail

```http
GET /vehicles/{vehicle_id}
```

---

## 6.4. Update Vehicle

```http
PUT /vehicles/{vehicle_id}
```

### Request Body

```json
{
  "license_plate": "59A-99999",
  "brand": "Toyota",
  "model": "Vios"
}
```

---

## 6.5. Delete Vehicle

```http
DELETE /vehicles/{vehicle_id}
```

---

# 7. RIDE API

---

## 7.1. Create Ride Booking

```http
POST /rides
```

### Authorization

```text
CUSTOMER
```

### Request Body

```json
{
  "pickup": {
    "address": "12 Nguyen Van Bao, Go Vap",
    "latitude": 10.8231,
    "longitude": 106.6297
  },
  "destination": {
    "address": "Ben Thanh Market",
    "latitude": 10.7720,
    "longitude": 106.6984
  },
  "vehicle_type": "CAR"
}
```

### Response

```json
{
  "success": true,
  "message": "Ride request created successfully",
  "data": {
    "ride_id": 1001,
    "status": "SEARCHING_DRIVER"
  }
}
```

---

## 7.2. Get Ride List

```http
GET /rides
```

### Query Parameters

| Parameter   | Description       |
| ----------- | ----------------- |
| page        | Page number       |
| limit       | Number of records |
| status      | Ride status       |
| customer_id | Customer ID       |
| driver_id   | Driver ID         |

---

## 7.3. Get Ride Detail

```http
GET /rides/{ride_id}
```

### Response

```json
{
  "success": true,
  "data": {
    "ride_id": 1001,
    "customer_id": 1,
    "driver_id": 2,
    "pickup_address": "12 Nguyen Van Bao",
    "destination_address": "Ben Thanh Market",
    "status": "DRIVER_ASSIGNED"
  }
}
```

---

## 7.4. Cancel Ride

```http
PATCH /rides/{ride_id}/cancel
```

### Request Body

```json
{
  "reason": "Customer changed plan"
}
```

### Response

```json
{
  "success": true,
  "message": "Ride cancelled successfully"
}
```

---

# 8. DRIVER ASSIGNMENT API

---

## 8.1. Search Available Drivers

```http
POST /rides/{ride_id}/drivers/search
```

### Response

```json
{
  "success": true,
  "data": {
    "ride_id": 1001,
    "drivers_found": 5
  }
}
```

---

## 8.2. Send Ride Request to Driver

```http
POST /rides/{ride_id}/driver-requests
```

### Request Body

```json
{
  "driver_id": 5
}
```

### Response

```json
{
  "success": true,
  "message": "Ride request sent successfully"
}
```

---

## 8.3. Driver Accept Ride

```http
PATCH /rides/{ride_id}/accept
```

### Authorization

```text
DRIVER
```

### Response

```json
{
  "success": true,
  "message": "Ride accepted successfully",
  "data": {
    "ride_id": 1001,
    "status": "DRIVER_ASSIGNED"
  }
}
```

---

## 8.4. Driver Reject Ride

```http
PATCH /rides/{ride_id}/reject
```

### Request Body

```json
{
  "reason": "Too far"
}
```

### Response

```json
{
  "success": true,
  "message": "Ride rejected successfully"
}
```

---

# 9. TRIP MANAGEMENT API

---

## 9.1. Update Ride Status

```http
PATCH /rides/{ride_id}/status
```

### Request Body

```json
{
  "status": "ARRIVING"
}
```

### Ride Status

| Status           | Description          |
| ---------------- | -------------------- |
| SEARCHING_DRIVER | Searching for driver |
| DRIVER_ASSIGNED  | Driver assigned      |
| ARRIVING         | Driver is arriving   |
| ARRIVED          | Driver arrived       |
| PICKED_UP        | Customer picked up   |
| IN_PROGRESS      | Trip in progress     |
| COMPLETED        | Trip completed       |
| CANCELLED        | Trip cancelled       |

---

## 9.2. Driver Arrived

```http
PATCH /rides/{ride_id}/arrived
```

### Response

```json
{
  "success": true,
  "message": "Driver arrived at pickup location",
  "data": {
    "ride_id": 1001,
    "status": "ARRIVED"
  }
}
```

---

## 9.3. Pickup Customer

```http
PATCH /rides/{ride_id}/pickup
```

### Response

```json
{
  "success": true,
  "message": "Customer picked up successfully"
}
```

---

## 9.4. Start Ride

```http
PATCH /rides/{ride_id}/start
```

### Response

```json
{
  "success": true,
  "message": "Ride started successfully",
  "data": {
    "status": "IN_PROGRESS"
  }
}
```

---

## 9.5. Complete Ride

```http
PATCH /rides/{ride_id}/complete
```

### Response

```json
{
  "success": true,
  "message": "Ride completed successfully",
  "data": {
    "ride_id": 1001,
    "status": "COMPLETED"
  }
}
```

---

# 10. LOCATION API

---

## 10.1. Update Driver Location

```http
POST /drivers/{driver_id}/locations
```

### Request Body

```json
{
  "latitude": 10.8231,
  "longitude": 106.6297
}
```

### Response

```json
{
  "success": true,
  "message": "Driver location updated successfully"
}
```

---

## 10.2. Get Latest Driver Location

```http
GET /drivers/{driver_id}/locations/latest
```

### Response

```json
{
  "success": true,
  "data": {
    "driver_id": 1,
    "latitude": 10.8231,
    "longitude": 106.6297,
    "updated_at": "2026-09-07T18:00:00Z"
  }
}
```

---

# 11. FARE API

---

## 11.1. Calculate Fare

```http
POST /rides/{ride_id}/fare
```

### Response

```json
{
  "success": true,
  "data": {
    "ride_id": 1001,
    "distance": 8.5,
    "duration": 25,
    "fare": 85000,
    "currency": "VND"
  }
}
```

---

## 11.2. Get Ride Fare

```http
GET /rides/{ride_id}/fare
```

### Response

```json
{
  "success": true,
  "data": {
    "ride_id": 1001,
    "fare": 85000,
    "currency": "VND"
  }
}
```

---

# 12. PAYMENT API

---

## 12.1. Create Payment

```http
POST /payments
```

### Request Body

```json
{
  "ride_id": 1001,
  "payment_method": "CASH"
}
```

### Payment Methods

| Method     | Description        |
| ---------- | ------------------ |
| CASH       | Cash payment       |
| ELECTRONIC | Electronic payment |

### Response

```json
{
  "success": true,
  "message": "Payment created successfully",
  "data": {
    "payment_id": 5001,
    "status": "PENDING"
  }
}
```

---

## 12.2. Process Payment

```http
POST /payments/{payment_id}/process
```

### Response

```json
{
  "success": true,
  "message": "Payment processed successfully",
  "data": {
    "payment_id": 5001,
    "status": "SUCCESS"
  }
}
```

---

## 12.3. Get Payment Detail

```http
GET /payments/{payment_id}
```

### Response

```json
{
  "success": true,
  "data": {
    "payment_id": 5001,
    "ride_id": 1001,
    "amount": 85000,
    "payment_method": "ELECTRONIC",
    "status": "SUCCESS"
  }
}
```

---

## 12.4. Retry Payment

```http
PATCH /payments/{payment_id}/retry
```

### Response

```json
{
  "success": true,
  "message": "Payment retry initiated successfully"
}
```

---

# 13. TRANSACTION API

---

## 13.1. Get Transaction List

```http
GET /transactions
```

### Query Parameters

| Parameter | Description        |
| --------- | ------------------ |
| page      | Page number        |
| limit     | Number of records  |
| ride_id   | Ride ID            |
| status    | Transaction status |
| from_date | Start date         |
| to_date   | End date           |

---

## 13.2. Get Transaction Detail

```http
GET /transactions/{transaction_id}
```

### Response

```json
{
  "success": true,
  "data": {
    "transaction_id": 1,
    "payment_id": 5001,
    "ride_id": 1001,
    "amount": 85000,
    "status": "SUCCESS",
    "created_at": "2026-09-07T18:00:00Z"
  }
}
```

---

# 14. NOTIFICATION API

---

## 14.1. Get Notification List

```http
GET /notifications
```

### Response

```json
{
  "success": true,
  "data": [
    {
      "notification_id": 1,
      "title": "Driver Assigned",
      "message": "Your driver has accepted the ride",
      "is_read": false,
      "created_at": "2026-09-07T18:00:00Z"
    }
  ]
}
```

---

## 14.2. Send Notification

```http
POST /notifications
```

### Request Body

```json
{
  "user_id": 1,
  "title": "Ride Completed",
  "message": "Your ride has been completed"
}
```

---

## 14.3. Mark Notification as Read

```http
PATCH /notifications/{notification_id}/read
```

### Response

```json
{
  "success": true,
  "message": "Notification marked as read"
}
```

---

# 15. RATING API

---

## 15.1. Create Driver Rating

```http
POST /rides/{ride_id}/ratings
```

### Authorization

```text
CUSTOMER
```

### Request Body

```json
{
  "rating": 5,
  "comment": "Driver was friendly and professional"
}
```

### Parameters

| Field   | Type    | Required | Description        |
| ------- | ------- | -------- | ------------------ |
| rating  | integer | Yes      | Rating from 1 to 5 |
| comment | string  | No       | Customer comment   |

### Response

```json
{
  "success": true,
  "message": "Rating submitted successfully"
}
```

---

## 15.2. Get Driver Ratings

```http
GET /drivers/{driver_id}/ratings
```

### Response

```json
{
  "success": true,
  "data": {
    "average_rating": 4.8,
    "total_ratings": 120,
    "items": [
      {
        "rating_id": 1,
        "rating": 5,
        "comment": "Very good driver"
      }
    ]
  }
}
```

---

# 16. REPORT API

---

## 16.1. Get Overview Report

```http
GET /reports/overview
```

### Authorization

```text
ADMIN
OPERATOR
```

### Response

```json
{
  "success": true,
  "data": {
    "total_rides": 1000,
    "completed_rides": 850,
    "cancelled_rides": 150,
    "revenue": 85000000
  }
}
```

---

## 16.2. Get Revenue Report

```http
GET /reports/revenue
```

### Query Parameters

| Parameter | Type | Required |
| --------- | ---- | -------- |
| from_date | date | No       |
| to_date   | date | No       |

### Example

```http
GET /reports/revenue?from_date=2026-09-01&to_date=2026-09-30
```

### Response

```json
{
  "success": true,
  "data": {
    "from_date": "2026-09-01",
    "to_date": "2026-09-30",
    "total_revenue": 85000000,
    "currency": "VND"
  }
}
```

---

## 16.3. Get Ride Report

```http
GET /reports/rides
```

### Response

```json
{
  "success": true,
  "data": {
    "total_rides": 1000,
    "completed": 850,
    "cancelled": 150
  }
}
```

---

## 16.4. Get Completion Rate Report

```http
GET /reports/completion-rate
```

### Response

```json
{
  "success": true,
  "data": {
    "completion_rate": 85
  }
}
```

---

## 16.5. Get Cancellation Rate Report

```http
GET /reports/cancellation-rate
```

### Response

```json
{
  "success": true,
  "data": {
    "cancellation_rate": 15
  }
}
```

---

## 16.6. Get Driver Performance Report

```http
GET /reports/drivers-performance
```

### Response

```json
{
  "success": true,
  "data": [
    {
      "driver_id": 1,
      "driver_name": "Tran Van B",
      "total_rides": 150,
      "completed_rides": 140,
      "cancelled_rides": 10,
      "average_rating": 4.8
    }
  ]
}
```

---

# 17. ADMIN API

---

## 17.1. Get Role List

```http
GET /roles
```

### Response

```json
{
  "success": true,
  "data": [
    {
      "role_id": 1,
      "role_name": "ADMIN"
    },
    {
      "role_id": 2,
      "role_name": "CUSTOMER"
    },
    {
      "role_id": 3,
      "role_name": "DRIVER"
    }
  ]
}
```

---

## 17.2. Get Permission List

```http
GET /permissions
```

---

## 17.3. Assign Permissions to Role

```http
POST /roles/{role_id}/permissions
```

### Request Body

```json
{
  "permission_ids": [
    1,
    2,
    3
  ]
}
```

### Response

```json
{
  "success": true,
  "message": "Permissions assigned successfully"
}
```

---

## 17.4. Monitor Active Rides

```http
GET /rides/monitoring/active
```

### Authorization

```text
ADMIN
OPERATOR
```

### Response

```json
{
  "success": true,
  "data": [
    {
      "ride_id": 1001,
      "customer_id": 1,
      "driver_id": 2,
      "status": "IN_PROGRESS"
    }
  ]
}
```

---

## 17.5. Report Ride Incident

```http
POST /rides/{ride_id}/incidents
```

### Request Body

```json
{
  "incident_type": "CONNECTION_ERROR",
  "description": "Driver lost internet connection"
}
```

### Response

```json
{
  "success": true,
  "message": "Incident reported successfully"
}
```

---

# 18. ERROR CODES

| HTTP Code | Error Code            | Description                 |
| --------- | --------------------- | --------------------------- |
| 400       | BAD_REQUEST           | Invalid request             |
| 400       | INVALID_DATA          | Invalid data                |
| 401       | UNAUTHORIZED          | Authentication required     |
| 401       | INVALID_TOKEN         | Invalid token               |
| 401       | TOKEN_EXPIRED         | Token expired               |
| 401       | INVALID_CREDENTIALS   | Incorrect login information |
| 403       | FORBIDDEN             | Permission denied           |
| 404       | NOT_FOUND             | Resource not found          |
| 404       | USER_NOT_FOUND        | User not found              |
| 404       | DRIVER_NOT_FOUND      | Driver not found            |
| 404       | RIDE_NOT_FOUND        | Ride not found              |
| 404       | PAYMENT_NOT_FOUND     | Payment not found           |
| 409       | CONFLICT              | Resource conflict           |
| 409       | PHONE_EXISTS          | Phone already exists        |
| 409       | EMAIL_EXISTS          | Email already exists        |
| 422       | VALIDATION_ERROR      | Validation failed           |
| 500       | INTERNAL_SERVER_ERROR | System error                |

---

# 19. API ENDPOINT SUMMARY

| #  | Method | Endpoint                         | Description            |
| -- | ------ | -------------------------------- | ---------------------- |
| 1  | POST   | `/auth/register`                 | Register customer      |
| 2  | POST   | `/auth/login`                    | Login                  |
| 3  | GET    | `/auth/me`                       | Get current user       |
| 4  | POST   | `/auth/logout`                   | Logout                 |
| 5  | GET    | `/customers`                     | Get customers          |
| 6  | GET    | `/customers/{id}`                | Get customer detail    |
| 7  | PUT    | `/customers/{id}`                | Update customer        |
| 8  | DELETE | `/customers/{id}`                | Delete customer        |
| 9  | POST   | `/drivers`                       | Create driver          |
| 10 | GET    | `/drivers`                       | Get drivers            |
| 11 | GET    | `/drivers/{id}`                  | Get driver detail      |
| 12 | PUT    | `/drivers/{id}`                  | Update driver          |
| 13 | PATCH  | `/drivers/{id}/status`           | Update driver status   |
| 14 | POST   | `/vehicles`                      | Create vehicle         |
| 15 | GET    | `/vehicles`                      | Get vehicles           |
| 16 | GET    | `/vehicles/{id}`                 | Get vehicle detail     |
| 17 | PUT    | `/vehicles/{id}`                 | Update vehicle         |
| 18 | DELETE | `/vehicles/{id}`                 | Delete vehicle         |
| 19 | POST   | `/rides`                         | Create ride            |
| 20 | GET    | `/rides`                         | Get rides              |
| 21 | GET    | `/rides/{id}`                    | Get ride detail        |
| 22 | PATCH  | `/rides/{id}/cancel`             | Cancel ride            |
| 23 | POST   | `/rides/{id}/drivers/search`     | Search drivers         |
| 24 | POST   | `/rides/{id}/driver-requests`    | Send ride request      |
| 25 | PATCH  | `/rides/{id}/accept`             | Accept ride            |
| 26 | PATCH  | `/rides/{id}/reject`             | Reject ride            |
| 27 | PATCH  | `/rides/{id}/status`             | Update ride status     |
| 28 | PATCH  | `/rides/{id}/arrived`            | Driver arrived         |
| 29 | PATCH  | `/rides/{id}/pickup`             | Pickup customer        |
| 30 | PATCH  | `/rides/{id}/start`              | Start ride             |
| 31 | PATCH  | `/rides/{id}/complete`           | Complete ride          |
| 32 | POST   | `/drivers/{id}/locations`        | Update location        |
| 33 | GET    | `/drivers/{id}/locations/latest` | Get location           |
| 34 | POST   | `/rides/{id}/fare`               | Calculate fare         |
| 35 | GET    | `/rides/{id}/fare`               | Get fare               |
| 36 | POST   | `/payments`                      | Create payment         |
| 37 | POST   | `/payments/{id}/process`         | Process payment        |
| 38 | GET    | `/payments/{id}`                 | Get payment            |
| 39 | PATCH  | `/payments/{id}/retry`           | Retry payment          |
| 40 | GET    | `/transactions`                  | Get transactions       |
| 41 | GET    | `/transactions/{id}`             | Get transaction detail |
| 42 | GET    | `/notifications`                 | Get notifications      |
| 43 | POST   | `/notifications`                 | Send notification      |
| 44 | PATCH  | `/notifications/{id}/read`       | Mark as read           |
| 45 | POST   | `/rides/{id}/ratings`            | Create rating          |
| 46 | GET    | `/drivers/{id}/ratings`          | Get driver ratings     |
| 47 | GET    | `/reports/overview`              | Overview report        |
| 48 | GET    | `/reports/revenue`               | Revenue report         |
| 49 | GET    | `/reports/rides`                 | Ride report            |
| 50 | GET    | `/reports/completion-rate`       | Completion rate        |
| 51 | GET    | `/reports/cancellation-rate`     | Cancellation rate      |
| 52 | GET    | `/reports/drivers-performance`   | Driver performance     |
| 53 | GET    | `/roles`                         | Get roles              |
| 54 | GET    | `/permissions`                   | Get permissions        |
| 55 | POST   | `/roles/{id}/permissions`        | Assign permissions     |
| 56 | GET    | `/rides/monitoring/active`       | Monitor active rides   |
| 57 | POST   | `/rides/{id}/incidents`          | Report incident        |

---

# END OF DOCUMENT

**CAB System API Documentation**
**Version:** 1.0.0
