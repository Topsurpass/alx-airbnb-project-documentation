# Backend Feature Requirement Specifications

This document outlines the requirements for three key backend features: **User Authentication**, **Property Management**, and **Booking System**. Each feature includes API endpoints, input/output specifications, validation rules, and performance criteria.

---

## 1. User Authentication

### **API Endpoints**
1. **Register User**
   - **Endpoint**: `POST /api/auth/register`
   - **Input**:
     ```json
     {
       "name": "string",
       "email": "string (valid email format)",
       "password": "string (min 8 characters)"
     }
     ```
   - **Output**:
     - **Success (201)**:
       ```json
       {
         "message": "Registration successful",
         "userId": "string"
       }
       ```
     - **Failure (400/409)**:
       ```json
       {
         "error": "Validation error or email already exists"
       }
       ```

2. **Login User**
   - **Endpoint**: `POST /api/auth/login`
   - **Input**:
     ```json
     {
       "email": "string",
       "password": "string"
     }
     ```
   - **Output**:
     - **Success (200)**:
       ```json
       {
         "message": "Login successful",
         "token": "JWT string",
         "expiresIn": 3600
       }
       ```
     - **Failure (401)**:
       ```json
       {
         "error": "Invalid credentials"
       }
       ```

3. **Refresh Token**
   - **Endpoint**: `POST /api/auth/refresh`
   - **Input**:
     ```json
     {
       "refreshToken": "JWT string"
     }
     ```
   - **Output**:
     - **Success (200)**:
       ```json
       {
         "token": "New JWT string",
         "expiresIn": 3600
       }
       ```
     - **Failure (403)**:
       ```json
       {
         "error": "Invalid or expired refresh token"
       }
       ```

### **Validation Rules**
- Name: Required, minimum length of 2.
- Email: Must be a valid format and unique.
- Password: Minimum length of 8 characters.

### **Performance Criteria**
- Token generation must complete within **500ms**.
- Validation errors should be returned within **200ms**.

---

## 2. Property Management

### **API Endpoints**
1. **Create Property**
   - **Endpoint**: `POST /api/properties`
   - **Input**:
     ```json
     {
       "title": "string",
       "description": "string",
       "price": "number",
       "location": "string",
       "images": ["array of strings (URLs)"]
     }
     ```
   - **Output**:
     - **Success (201)**:
       ```json
       {
         "message": "Property created successfully",
         "propertyId": "string"
       }
       ```
     - **Failure (400)**:
       ```json
       {
         "error": "Validation error"
       }
       ```

2. **Get Property**
   - **Endpoint**: `GET /api/properties/:propertyId`
   - **Output**:
     - **Success (200)**:
       ```json
       {
         "propertyId": "string",
         "title": "string",
         "description": "string",
         "price": "number",
         "location": "string",
         "images": ["array of strings (URLs)"],
         "ownerId": "string"
       }
       ```
     - **Failure (404)**:
       ```json
       {
         "error": "Property not found"
       }
       ```

3. **Update Property**
   - **Endpoint**: `PUT /api/properties/:propertyId`
   - **Input**:
     ```json
     {
       "title": "string (optional)",
       "description": "string (optional)",
       "price": "number (optional)",
       "location": "string (optional)",
       "images": ["array of strings (URLs, optional)"]
     }
     ```
   - **Output**:
     - **Success (200)**:
       ```json
       {
         "message": "Property updated successfully"
       }
       ```
     - **Failure (400/404)**:
       ```json
       {
         "error": "Validation error or property not found"
       }
       ```

### **Validation Rules**
- Title: Required, minimum length of 5.
- Description: Required, minimum length of 20.
- Price: Must be a positive number.
- Location: Required, valid string.

### **Performance Criteria**
- CRUD operations should complete within **1 second**.

---

## 3. Booking System

### **API Endpoints**
1. **Create Booking**
   - **Endpoint**: `POST /api/bookings`
   - **Input**:
     ```json
     {
       "propertyId": "string",
       "userId": "string",
       "startDate": "ISO8601 date string",
       "endDate": "ISO8601 date string",
       "totalPrice": "number"
     }
     ```
   - **Output**:
     - **Success (201)**:
       ```json
       {
         "message": "Booking created successfully",
         "bookingId": "string"
       }
       ```
     - **Failure (400)**:
       ```json
       {
         "error": "Validation error"
       }
       ```

2. **Get Booking**
   - **Endpoint**: `GET /api/bookings/:bookingId`
   - **Output**:
     - **Success (200)**:
       ```json
       {
         "bookingId": "string",
         "propertyId": "string",
         "userId": "string",
         "startDate": "ISO8601 date string",
         "endDate": "ISO8601 date string",
         "totalPrice": "number",
         "status": "confirmed/pending/cancelled"
       }
       ```
     - **Failure (404)**:
       ```json
       {
         "error": "Booking not found"
       }
       ```

3. **Cancel Booking**
   - **Endpoint**: `DELETE /api/bookings/:bookingId`
   - **Output**:
     - **Success (200)**:
       ```json
       {
         "message": "Booking cancelled successfully"
       }
       ```
     - **Failure (404)**:
       ```json
       {
         "error": "Booking not found"
       }
       ```

### **Validation Rules**
- Property ID: Must be a valid, existing property.
- User ID: Must be a valid, existing user.
- Dates: Start date must be before the end date.

### **Performance Criteria**
- Booking creation must complete within **2 seconds**, including database transactions.

---
