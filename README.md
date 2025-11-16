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

## Service Communication

Services communicate through:
1. **API Gateway**: Routes external requests to appropriate services
2. **Inter-service calls**: Services can call each other via HTTP requests
3. **Shared authentication**: JWT tokens validated across services

## Key Features

- **Independent Deployment**: Each service can be deployed separately
- **Scalability**: Scale services based on demand (search and booking handle more traffic)
- **Fault Isolation**: Failure in one service doesn't crash the entire system
- **Technology Flexibility**: Each service can use different tech if needed
- **Team Independence**: Different teams can work on different services

## Security

- JWT-based authentication
- Password hashing with `password_hash()`
- Input validation and sanitization
- SQL injection prevention with prepared statements
- CORS configuration in API Gateway

## Testing

Test each service independently:
```bash
# Test Property Service
curl http://localhost:8001/api/properties

# Test through API Gateway
curl http://localhost:8000/api/properties
```