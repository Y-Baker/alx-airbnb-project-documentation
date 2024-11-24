# **Requirements and API Documentation**

## **Table of Contents**
1. [User Management](#1-user-management)  
    1.1. [Register User](#11-register-user)  
    1.2. [Login User](#12-login-user)  
    1.3. [Refresh Token](#13-refresh-token)  
    1.4. [Logout](#14-logout)  
    1.5. [Change Password](#15-change-password)  
    1.6. [Edit Profile](#16-edit-profile)  

2. [Property Management](#2-property-management)  
    2.1. [Add Property](#21-add-property)  
    2.2. [Edit Property](#22-edit-property)  
    2.3. [Delete Property](#23-delete-property)  
    2.4. [Advanced Property Search](#24-advanced-property-search)  

3. [Booking System](#3-booking-system)  
    3.1. [Create Booking](#31-create-booking)  
    3.2. [Cancel Booking](#32-cancel-booking)  
    3.3. [View Bookings](#33-view-bookings)  

4. [System Notifications](#4-system-notifications)  
    4.1. [Send Notification](#41-send-notification)  
    4.2. [Get Notifications](#42-get-notifications)  

5. [Admin Monitoring Enhancements](#5-admin-monitoring-enhancements)  
    5.1. [Get System Analytics](#51-get-system-analytics)  
    5.2. [Trigger System Alert](#52-trigger-system-alert)  
    5.3. [View Activity Logs](#53-view-activity-logs)  

6. [Performance Criteria](#6-performance-criteria)

---

## **1. User Management**

### **1.1. Register User**
- **Endpoint**: `POST /api/auth/register`
- **Description**: Registers a new user in the system.

#### **Request Body**:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "SecureP@ssw0rd",
  "role": "guest"
}
```

#### **Validation Rules**
- **email**: Must be a valid email format, unique in the database
- **password**: Minimum 8 characters, at least one uppercase letter, one number, and one special character.
- **role**: Must be one of roles.


#### **Response**
- **Success (201)**:
```json
{
  "message": "User registered successfully",
  "userId": "123e4567-e89b-12d3-a456-426614174000"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid email format"
}
```




### **1.2. Login User**
- **Endpoint**: `POST /api/auth/login`
- **Description**: Login user in the system.

#### **Request Body**:
```json
{
  "email": "user@example.com",
  "password": "P@ssw0rd!"
}
```

#### **Validation Rules**
- **email**: Must match a registered user
- **password**: Must match the password for the provided email.


#### **Response**
- **Success (200)**:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 3600
}
```

- **Error (400)**:
```json
{
  "error": "Invalid email or password"
}
```

### **1.3. Refresh Token**
- **Endpoint**: `POST /api/auth/refresh-token`
- **Description**: Refresh Token.

#### **Request Body**:
```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### **Response**
- **Success (200)**:
```json
{
  "token": "new-access-token",
  "expiresIn": 3600
}
```

- **Error (403)**:
```json
{
  "error": "Unoauthorized"
}
```

### **1.4. Logout**
- **Endpoint**: `POST /api/auth/logout`
- **Description**: Logs out the user by invalidating their refresh token.

#### **Request Body**:
```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### **Validation Rules**
- **refreshToken**: Must be a valid, non-expired token

#### **Response**
- **Success (200)**:
```json
{
  "message": "User logged out successfully"
}
```

- **Error (403)**:
```json
{
  "error": "Unoauthorized"
}
```

### **1.5. Change Password**
- **Endpoint**: `PUT /api/users/change-password`
- **Description**: Updates the user’s password after verifying the current password.

#### **Request Body**:
```json
{
  "currentPassword": "OldP@ssw0rd",
  "newPassword": "NewP@ssw0rd!",
  "confirmPassword": "NewP@ssw0rd!"
}
```

#### **Validation Rules**
- **currentPassword**: Must match the user’s current password
- **newPassword**: Minimum 8 characters, at least one uppercase letter, one number, and one special character.
- **confirmPassword**: Must match the new password.

#### **Response**
- **Success (200)**:
```json
{
  "message": "Password updated successfully"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid password format"
}
```

### **1.6. Edit Profile**
- **Endpoint**: `PUT /api/users/edit-profile`
- **Description**: Updates the user’s profile information.

#### **Request Body**:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "newemail@example.com",
  "phoneNumber": "+1234567890"
}
```

#### **Validation Rules**
- **email**: Must be a valid email format, unique in the database
- **phoneNumber**: Optional but must follow a valid phone number format

#### **Response**

- **Success (200)**:
```json
{
  "message": "Profile updated successfully"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid email format"
}
```


---


## **2. Property Management**

### **2.1. Add Property**
- **Endpoint**: `POST /api/properties`
- **Description**: Adds a new property listing to the system.

#### **Request Body**:
```json
{
  "title": "Cozy Apartment",
  "description": "A modern, fully-furnished apartment.",
  "location": "New York, NY",
  "price": 150,
  "amenities": ["Wi-Fi", "Pool", "Pet-friendly"],
  "availability": {
    "startDate": "2024-12-01",
    "endDate": "2024-12-31"
  }
}
```

#### **Validation Rules**
- **title**: Non-empty, maximum 100 characters
- **price**: Must be a positive number
- **availability**: Must have a valid start and end date

#### **Response**

- **Success (201)**:
```json
{
  "message": "Property added successfully",
  "propertyId": "123e4567-e89b-12d3-a456-426614174000"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid property details"
}
```

#### **Performance Criteria**
- **Response Time**: 700ms
- **Throughput**: 100 requests/second


### **2.2. Edit Property**
- **Endpoint**: `PUT /api/properties/:propertyId`
- **Description**: Updates an existing property listing.

#### **Request Body**:
```json
{
  "title": "Updated Apartment",
  "description": "A modern, fully-furnished apartment.",
  "price": 200,
  "amenities": ["Wi-Fi", "Pool", "Pet-friendly"],
  "availability": {
    "startDate": "2024-12-01",
    "endDate": "2024-12-31"
  }
}
```

#### **Validation Rules**
- **title**: Non-empty, maximum 100 characters
- **price**: Must be a positive number
- **availability**: Must have a valid start and end date

#### **Response**

- **Success (201)**:
```json
{
  "message": "Property updated successfully"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid property details"
}
```

### **2.3. Delete Property**
- **Endpoint**: `DELETE /api/properties/:propertyId`
- **Description**: Deletes an existing property listing.

#### **Response**

- **Success (200)**:
```json
{
  "message": "Property deleted successfully"
}
```

- **Error (404)**:
```json
{
  "error": "Property not found"
}
```

### **2.4. Advanced Property Search**
- **Endpoint**: `GET /api/properties/search`
- **Description**: Searches properties using advanced filters and pagination.

#### **Query Parameters**:
- **location**: `string` (optional, partial match supported)
- **priceMin**: `number` (optional, minimum price)
- **priceMax**: `number` (optional, maximum price)
- **amenities**: `string[]` (optional, array of amenities, matches at least one amenity)
- **rating**: `number` (optional, minimum rating to filter)
- **propertyType**: `string` (optional, e.g., `apartment`, `house`)
- **page**: `number` (optional, default 1)
- **pageSize**: `number` (optional, default 10)
- **sortBy**: `string` (optional, e.g., `price`, `rating`)
- **sortOrder**: `string` (optional, `asc` or `desc`)

#### **Response**

- **Success (200)**:
```json
{
{
  "currentPage": 1,
  "totalPages": 5,
  "properties": [
    {
      "propertyId": "987e4567-e89b-12d3-a456-426614174001",
      "title": "Luxury Apartment",
      "location": "New York, NY",
      "price": 300,
      "amenities": ["Wi-Fi", "Pool"],
      "rating": 4.5,
      "propertyType": "apartment"
    }
  ]
}
}
```

- **Error (400)**:
```json
{
  "error": "Invalid search parameters"
}
```

---

## **3. Booking System**

### **3.1. Create Booking**
- **Endpoint**: `POST /api/bookings`
- **Description**: Creates a new booking for a property.

#### **Request Body**:
```json
{
  "propertyId": "987e4567-e89b-12d3-a456-426614174001",
  "guestId": "123e4567-e89b-12d3-a456-426614174000",
  "startDate": "2024-12-10",
  "endDate": "2024-12-15"
}
```

#### **Validation Rules**
- **propertyId**: Must be a valid property
- **guestId**: Must be a valid user
- **startDate**: Must be a valid date, before the end date
- **endDate**: Must be a valid date, after the start date
- **availability**: Prevent overlapping bookings

#### **Response**

- **Success (201)**:
```json
{
  "message": "Booking created successfully",
  "bookingId": "123e4567-e89b-12d3-a456-426614174001"
}
```

- **Error (409)**:
```json
{
  "error": "Property not available for selected dates"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid booking details"
}
```

### **3.2. Cancel Booking**
- **Endpoint**: `DELETE /api/bookings/:bookingId`
- **Description**: Cancels an existing booking.

#### **Response**

- **Success (200)**:
```json
{
  "message": "Booking canceled successfully"
}
```

- **Error (404)**:
```json
{
  "error": "Booking not found"
}
```

### **3.3. View Bookings**
- **Endpoint**: `GET /api/bookings`
- **Description**: Retrieves all bookings for a user.

#### **Query Parameters**:
- **userId**: `string` (Optional, default to current user)
- **status**: `string` (Optional, e.g., `pending`, `confirmed`, `canceled`, `completed`)
- **page**: `number` (optional, default 1)
- **pageSize**: `number` (optional, default 10)

#### **Response**

- **Success (200)**:
```json
{
  "currentPage": 1,
  "totalPages": 2,
  "bookings": [
    {
      "bookingId": "123e4567-e89b-12d3-a456-426614174001",
      "propertyId": "987e4567-e89b-12d3-a456-426614174001",
      "startDate": "2024-12-10",
      "endDate": "2024-12-15",
      "status": "confirmed"
    }
  ]
}
```

- **Error (400)**:
```json
{
  "error": "Invalid search parameters"
}
```

---

## **4. System Notifications**

### **4.1. Send Notification**
- **Endpoint**: `POST /api/notifications`
- **Description**: Sends a notification to a user.

#### **Request Body**:
```json
{
  "userId": "123e4567-e89b-12d3-a456-426614174000",
  "message": "Your booking has been confirmed",
  "type": "email" // Options: 'email', 'sms', 'in-app'
}
```

#### **Validation Rules**
- **userId**: Must be a valid user
- **type**: Must be one of the specified options

#### **Response**

- **Success (201)**:
```json
{
  "message": "Notification sent successfully",
  "notificationId": "123e4567-e89b-12d3-a456-426614174001"
}
```

- **Error (404)**:
```json
{
  "error": "User not found"
}
```

### **4.2. Get Notifications**
- **Endpoint**: `GET /api/notifications`
- **Description**: Retrieves all notifications for a user.

#### **Query Parameters**:
- **userId**: `string` (Optional, default to current user)
- **status**: `string` (Optional, e.g., `unread`, `read`)
- **type**: `string` (Optional, e.g., `email`, `sms`, `in-app`)
- **page**: `number` (optional, default 1)
- **pageSize**: `number` (optional, default 10)

#### **Response**

- **Success (200)**:
```json
{
  "currentPage": 1,
  "totalPages": 2,
  "notifications": [
    {
      "notificationId": "123e4567-e89b-12d3-a456-426614174001",
      "message": "Your booking has been confirmed",
      "timestamp": "2024-12-10T12:00:00Z",
      "type": "email",
      "status": "unread"
    }
  ]
}
```

- **Error (400)**:
```json
{
  "error": "Invalid search parameters"
}
```

---

## **5. Admin Monitoring Enhancements**

### **5.1. Get System Analytics**
- **Endpoint**: `GET /api/admin/analytics`
- **Description**: Retrieves system analytics data.

#### **Query Parameters**:
- **startDate**: `string` (Optional, default to 30 days ago)
- **endDate**: `string` (Optional, default to current date)

#### **Response**

- **Success (200)**:
```json
{
  "totalUsers": 1000,
  "totalProperties": 500,
  "totalBookings": 200,
  "activeBookings": 50,
  "totalRevenue": 5000
}
```

- **Error (400)**:
```json
{
  "error": "Invalid date range"
}
```

### **5.2. Trigger System Alert**
- **Endpoint**: `POST /api/admin/alerts`
- **Description**: Triggers a system alert for all users.

#### **Request Body**:
```json
{
  "alertType": "performance", // Options: 'performance', 'user-activity', 'booking-issues'
  "message": "High API response time detected"
}
```

#### **Validation Rules**
- **alertType**: Must be one of the specified options

#### **Response**

- **Success (201)**:
```json
{
  "message": "Alert triggered successfully",
  "alertId": "123e4567-e89b-12d3-a456-426614174001"
}
```

- **Error (400)**:
```json
{
  "error": "Invalid alert type"
}
```

### **5.3. View Activity Logs**

- **Endpoint**: `GET /api/admin/logs`
- **Description**: Retrieves system activity logs.

#### **Query Parameters**:
- **startDate**: `string` (Optional, default to 30 days ago)
- **endDate**: `string` (Optional, default to current date)
- **userId**: `string` (Optional, filter by user)

#### **Response**

- **Success (200)**:
```json
{
  "currentPage": 1,
  "totalPages": 5,
  "logs": [
    {
      "logId": "123e4567-e89b-12d3-a456-426614174001",
      "userId": "123e4567-e89b-12d3-a456-426614174000",
      "action": "Property added",
      "timestamp": "2024-12-10T12:00:00Z",
      "details": "Property ID: 987e4567-e89b-12d3-a456-426614174001"
    }
  ]
}
```

- **Error (400)**:
```json
{
  "error": "Invalid date range"
}
```

---

## **6. Performance Criteria**

### **6.1. Response Time**

- **All endpoints must handle at least `1,000` requests per second**.
- **All endpoints must respond within `700ms`**.
- **Notifications and alerts must be processed and delivered within `5 seconds`**.
- **Admin analytics must process and return up to `1 year` of data within `2 seconds`**.

### **6.2. Scalability**

- **The system must be able to handle up to `10,000` concurrent users**.
- **The system must be able to store and retrieve up to `1 million` properties**.


---

## **Authors :black_nib:**

* [__Repo__](https://github.com/Y-Baker/alx-airbnb-project-documentation)
* __Yousef Bakier__ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <br />
 &nbsp;&nbsp;[<img height="" src="https://img.shields.io/static/v1?label=&message=GitHub&color=181717&logo=GitHub&logoColor=f2f2f2&labelColor=2F333A" alt="Github">](https://github.com/Y-Baker)
