# Backend Requirement Specifications - Airbnb Clone Project

## Objective
This document outlines the technical and functional requirements for three core backend features of the Airbnb Clone Project:
1. User Authentication
2. Property Management
3. Booking System

Each section contains API endpoints, input/output specifications, validation rules, and performance criteria.

## 1. User Authentication
Purpose: To securely register, authenticate, and manage user accounts (guests, hosts, and admins).

API Endpoints:
- POST /users/register → Create a new user account.
- POST /users/login → Authenticate user credentials.
- GET /users/{id} → Retrieve user profile.
- PUT /users/{id} → Update user profile.
- DELETE /users/{id} → Delete a user account.

Input Specifications (Registration):
{
  "name": "string",
  "email": "string (valid email)",
  "password": "string (min. 8 chars)",
  "role": "guest | host"
}

Input Specifications (Login):
{
  "email": "string",
  "password": "string"
}

Output Specifications:
- Success: JSON object with status, message, and data (including JWT token for login).
- Failure: JSON object with status and error message.

Validation Rules:
- Email must be unique and in a valid format.
- Password must be at least 8 characters.
- Role must be either guest or host.

Performance Criteria:
- Registration and login should respond within 300ms under normal server load.
- Passwords must be hashed using bcrypt before storage.

## 2. Property Management
Purpose: To allow hosts to add, update, retrieve, and delete property listings.

API Endpoints:
- POST /properties → Add a new property listing.
- GET /properties → Retrieve all properties (with pagination and filters).
- GET /properties/{id} → Retrieve specific property details.
- PUT /properties/{id} → Update property details.
- DELETE /properties/{id} → Remove a property listing.

Input Specifications (Add Property):
{
  "title": "string",
  "description": "string",
  "location": "string",
  "price_per_night": "number",
  "amenities": ["Wi-Fi", "Pool", "Parking"],
  "availability_dates": ["YYYY-MM-DD", "YYYY-MM-DD"]
}

Output Specifications:
- Success: JSON object with status, message, and property details.
- Failure: JSON object with status and error message.

Validation Rules:
- title and description are required.
- price_per_night must be a positive number.
- availability_dates must be in a valid date format.

Performance Criteria:
- Property retrieval must support pagination to return results in under 500ms.
- Search and filtering should be optimized using database indexing.

## 3. Booking System
Purpose: To enable guests to reserve properties and manage bookings.

API Endpoints:
- POST /bookings → Create a booking.
- GET /bookings → Retrieve all bookings for the authenticated user.
- GET /bookings/{id} → Retrieve booking details.
- PUT /bookings/{id} → Update booking dates.
- DELETE /bookings/{id} → Cancel a booking.

Input Specifications (Create Booking):
{
  "property_id": "integer",
  "start_date": "YYYY-MM-DD",
  "end_date": "YYYY-MM-DD"
}

Output Specifications:
- Success: JSON object with status, message, and booking details.
- Failure: JSON object with status and error message.

Validation Rules:
- Booking dates must not overlap with existing bookings.
- start_date must be before end_date.
- Only authenticated guests can create bookings.

Performance Criteria:
- Booking creation should respond within 400ms.
- Database must enforce constraints to prevent double bookings.

## References
- Django REST Framework: https://www.django-rest-framework.org/
- PostgreSQL Documentation: https://www.postgresql.org/docs/
- JWT Authentication: https://jwt.io/
