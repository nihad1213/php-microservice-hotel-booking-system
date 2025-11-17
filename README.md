# Hotel/Property Booking System - Microservices Architecture

## Project Structure

```
hotel-booking-system/
│
├── api-gateway/
│   ├── index.php
│   ├── config.php
│   └── router.php
│
├── services/
│   ├── property-service/
│   │   ├── index.php
│   │   ├── config/
│   │   │   └── database.php
│   │   ├── controllers/
│   │   │   └── PropertyController.php
│   │   ├── models/
│   │   │   └── Property.php
│   │   └── database.sql
│   │
│   ├── booking-service/
│   │   ├── index.php
│   │   ├── config/
│   │   │   └── database.php
│   │   ├── controllers/
│   │   │   └── BookingController.php
│   │   ├── models/
│   │   │   └── Booking.php
│   │   └── database.sql
│   │
│   ├── user-service/
│   │   ├── index.php
│   │   ├── config/
│   │   │   └── database.php
│   │   ├── controllers/
│   │   │   └── UserController.php
│   │   ├── models/
│   │   │   └── User.php
│   │   └── database.sql
│   │
│   ├── payment-service/
│   │   ├── index.php
│   │   ├── config/
│   │   │   └── database.php
│   │   ├── controllers/
│   │   │   └── PaymentController.php
│   │   ├── models/
│   │   │   └── Payment.php
│   │   └── database.sql
│   │
│   ├── review-service/
│   │   ├── index.php
│   │   ├── config/
│   │   │   └── database.php
│   │   ├── controllers/
│   │   │   └── ReviewController.php
│   │   ├── models/
│   │   │   └── Review.php
│   │   └── database.sql
│   │
│   └── search-service/
│       ├── index.php
│       ├── config/
│       │   └── database.php
│       ├── controllers/
│       │   └── SearchController.php
│       └── models/
│           └── Search.php
│
├── shared/
│   ├── utils/
│   │   ├── JWT.php
│   │   └── Response.php
│   └── middleware/
│       └── AuthMiddleware.php
│
└── README.md
```

## Database Setup

Each microservice has its own database:
- `hotel_property_db`
- `hotel_booking_db`
- `hotel_user_db`
- `hotel_payment_db`
- `hotel_review_db`
- `hotel_search_db`

## API Gateway Endpoints

### User Service
- `POST /api/users/register` - Register user
- `POST /api/users/login` - Login user
- `GET /api/users/{id}` - Get user profile
- `PUT /api/users/{id}` - Update user profile

### Property Service
- `GET /api/properties` - List all properties
- `GET /api/properties/{id}` - Get property details
- `POST /api/properties` - Create property (host only)
- `PUT /api/properties/{id}` - Update property
- `GET /api/properties/{id}/availability` - Check availability

### Search Service
- `GET /api/search?location=&checkin=&checkout=&guests=` - Search properties
- `GET /api/search/filters` - Get available filters

### Booking Service
- `POST /api/bookings` - Create booking
- `GET /api/bookings/{id}` - Get booking details
- `GET /api/bookings/user/{userId}` - Get user bookings
- `PUT /api/bookings/{id}/cancel` - Cancel booking

### Payment Service
- `POST /api/payments` - Process payment
- `GET /api/payments/{id}` - Get payment details
- `POST /api/payments/{id}/refund` - Process refund

### Review Service
- `POST /api/reviews` - Create review
- `GET /api/reviews/property/{propertyId}` - Get property reviews
- `PUT /api/reviews/{id}` - Update review
- `DELETE /api/reviews/{id}` - Delete review

## Technology Stack

- **Language**: Core PHP 7.4+
- **Database**: MySQL
- **Authentication**: JWT (JSON Web Tokens)
- **Communication**: REST APIs
- **Server**: Apache/Nginx

## Installation Steps

1. Create databases for each service
2. Import SQL files from each service's `database.sql`
3. Configure database credentials in each service's `config/database.php`
4. Set up virtual hosts or use PHP built-in server for each service
5. Configure API Gateway to route requests to appropriate services
6. Test endpoints using Postman or cURL

## Running the Services

### Using PHP Built-in Server (Development)

```bash
# API Gateway (Port 8000)
cd api-gateway
php -S localhost:8000

# Property Service (Port 8001)
cd services/property-service
php -S localhost:8001

# Booking Service (Port 8002)
cd services/booking-service
php -S localhost:8002

# User Service (Port 8003)
cd services/user-service
php -S localhost:8003

# Payment Service (Port 8004)
cd services/payment-service
php -S localhost:8004

# Review Service (Port 8005)
cd services/review-service
php -S localhost:8005

# Search Service (Port 8006)
cd services/search-service
php -S localhost:8006
```

## Folder Structure Explained

### `/api-gateway/`
Acts as the single entry point for all client requests. Routes incoming API calls to the appropriate microservice based on URL patterns. Handles request forwarding, response aggregation, and CORS configuration.

### `/services/`
Contains all independent microservices, each with its own database and business logic.

#### `/services/property-service/`
Manages property listings, amenities, and availability. Hosts can create/update properties, guests can view property details. Handles all property-related CRUD operations.

#### `/services/booking-service/`
Handles reservation creation, retrieval, and cancellations. Manages check-in/check-out dates, guest counts, and booking status (pending, confirmed, cancelled).

#### `/services/user-service/`
Manages user authentication, registration, and profiles. Handles both guest and host accounts, generates JWT tokens for secure authentication across services.

#### `/services/payment-service/`
Processes payments for bookings, handles refunds, and stores transaction records. Integrates with payment gateways and tracks payment status.

#### `/services/review-service/`
Allows guests to rate and review properties after their stay. Manages review CRUD operations and calculates average ratings for properties.

#### `/services/search-service/`
Provides advanced property search with filters (location, dates, price, guests, amenities). Optimized for fast search queries with indexed data.

### `/shared/`
Contains reusable code shared across all microservices.

#### `/shared/utils/`
- **JWT.php**: Handles JSON Web Token encoding/decoding for authentication
- **Response.php**: Standardizes API responses (success, error, JSON formatting)

#### `/shared/middleware/`
- **AuthMiddleware.php**: Validates JWT tokens and protects routes requiring authentication

### Folder Structure Inside Each Service:
- **`config/`**: Database connection configuration
- **`controllers/`**: Business logic and request handling
- **`models/`**: Database models and data operations
- **`database.sql`**: SQL schema for the service's database
- **`index.php`**: Entry point that routes requests to controllers