
-----

## Detailed Backend Feature Requirement Specifications

These specifications cover API endpoints, input/output, validation, and performance for key backend features of the Airbnb Clone.

### **1. Feature: User Authentication & Profile Management**

**Description:** This feature enables users to register, log in, and manage their profiles securely.

**Actors:** Guest, Host, Admin

**1.1. User Registration**

  * **API Endpoint:** `POST /api/v1/auth/register`
  * **Description:** Allows new users to create an account.
  * **Input (Request Body - JSON):**
    ```json
    {
      "email": "user@example.com",
      "password": "StrongPassword123!",
      "confirm_password": "StrongPassword123!",
      "first_name": "John",
      "last_name": "Doe",
      "role": "guest" // or "host"
    }
    ```
  * **Validation Rules:**
      * `email`: Required, valid email format, unique (must not exist in DB).
      * `password`: Required, minimum 8 characters, at least one uppercase letter, one lowercase letter, one digit, and one special character.
      * `confirm_password`: Required, must exactly match `password`.
      * `first_name`: Required, string, max 50 characters.
      * `last_name`: Required, string, max 50 characters.
      * `role`: Required, enum: ["guest", "host"].
  * **Output (Success - HTTP 201 Created):**
    ```json
    {
      "message": "User registered successfully",
      "user": {
        "id": "uuid-of-user",
        "email": "user@example.com",
        "first_name": "John",
        "last_name": "Doe",
        "role": "guest",
        "created_at": "2025-07-06T10:00:00Z"
      }
    }
    ```
  * **Output (Error - HTTP 400 Bad Request / 409 Conflict):**
    ```json
    {
      "error": "Invalid input data",
      "details": {
        "email": "Email already exists."
      }
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 200ms for 95% of requests.
      * Throughput: Support 50 registrations/second.

**1.2. User Login**

  * **API Endpoint:** `POST /api/v1/auth/login`
  * **Description:** Authenticates a user and returns a JWT.
  * **Input (Request Body - JSON):**
    ```json
    {
      "email": "user@example.com",
      "password": "StrongPassword123!"
    }
    ```
  * **Validation Rules:**
      * `email`: Required, valid email format.
      * `password`: Required, string.
  * **Output (Success - HTTP 200 OK):**
    ```json
    {
      "message": "Login successful",
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...", // JWT Token
      "user": {
        "id": "uuid-of-user",
        "email": "user@example.com",
        "role": "guest"
      }
    }
    ```
  * **Output (Error - HTTP 401 Unauthorized):**
    ```json
    {
      "error": "Invalid credentials."
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 150ms for 95% of requests.
      * Throughput: Support 100 logins/second.
      * Rate limiting: Implement for brute-force prevention (e.g., 5 attempts per minute per IP).

**1.3. User Profile Management (Update)**

  * **API Endpoint:** `PUT /api/v1/users/{user_id}`
  * **Description:** Updates a user's profile information.
  * **Authentication:** Requires valid JWT (Bearer Token in `Authorization` header).
  * **Authorization:** User can only update their own profile (or Admin can update any).
  * **Input (Request Body - JSON):**
    ```json
    {
      "first_name": "Jonathan",
      "last_name": "Smith",
      "phone_number": "+33612345678",
      "preferences": {
        "notifications": true,
        "language": "en"
      }
      // "profile_photo_url": "https://cloudinary.com/..." // If direct URL update is supported
    }
    ```
  * **Validation Rules:**
      * `user_id`: Required, UUID format, must correspond to authenticated user or admin.
      * `first_name`: Optional, string, max 50 characters.
      * `last_name`: Optional, string, max 50 characters.
      * `phone_number`: Optional, valid phone number format.
      * `preferences`: Optional, JSON object.
      * `profile_photo_url`: Optional, valid URL (for direct URL update).
  * **Output (Success - HTTP 200 OK):**
    ```json
    {
      "message": "Profile updated successfully",
      "user": {
        "id": "uuid-of-user",
        "email": "user@example.com",
        "first_name": "Jonathan",
        "last_name": "Smith",
        "phone_number": "+33612345678",
        "role": "guest",
        "preferences": { /* updated preferences */ },
        "updated_at": "2025-07-06T10:05:00Z"
      }
    }
    ```
  * **Output (Error - HTTP 400 Bad Request / 403 Forbidden / 404 Not Found):**
    ```json
    {
      "error": "Unauthorized access."
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 200ms for 95% of requests.

-----

### **2. Feature: Property Listings Management**

**Description:** This feature allows hosts to create, read, update, and delete their property listings.

**Actor:** Host

**2.1. Add Property Listing**

  * **API Endpoint:** `POST /api/v1/properties`
  * **Description:** Allows a host to create a new property listing.
  * **Authentication:** Requires valid Host JWT.
  * **Input (Request Body - JSON):**
    ```json
    {
      "title": "Cozy Apartment in City Center",
      "description": "A charming apartment close to all major attractions...",
      "location": {
        "address": "123 Main St",
        "city": "Paris",
        "country": "France",
        "latitude": 48.8566,
        "longitude": 2.3522
      },
      "price_per_night": 150.00,
      "currency": "EUR",
      "max_guests": 4,
      "number_of_beds": 2,
      "number_of_bathrooms": 1,
      "amenities": ["Wi-Fi", "Kitchen", "Air Conditioning"],
      "availability": [
        {"start_date": "2025-08-01", "end_date": "2025-08-10"},
        {"start_date": "2025-09-01", "end_date": "2025-09-05"}
      ]
      // "image_urls": ["url1", "url2"] // Assuming images are uploaded via separate process and URLs provided here
    }
    ```
  * **Validation Rules:**
      * `title`: Required, string, min 10, max 100 characters.
      * `description`: Required, string, min 50 characters.
      * `location.address/city/country`: Required, strings.
      * `location.latitude/longitude`: Required, valid decimal numbers (for geographic coordinates).
      * `price_per_night`: Required, positive number, max 2 decimal places.
      * `currency`: Required, 3-letter ISO currency code (e.g., "EUR", "USD").
      * `max_guests`: Required, positive integer, min 1.
      * `number_of_beds/bathrooms`: Required, non-negative integer.
      * `amenities`: Optional, array of strings.
      * `availability`: Required, array of objects, each with valid `start_date` and `end_date` (ISO format YYYY-MM-DD), `start_date` must be before `end_date`, dates must be in the future.
  * **Output (Success - HTTP 201 Created):**
    ```json
    {
      "message": "Property listing created successfully",
      "property": {
        "id": "uuid-of-property",
        "host_id": "uuid-of-host",
        "title": "Cozy Apartment in City Center",
        "price_per_night": 150.00,
        "location": { "city": "Paris" },
        "status": "active",
        "created_at": "2025-07-06T10:10:00Z"
      }
    }
    ```
  * **Output (Error - HTTP 400 Bad Request / 401 Unauthorized):**
    ```json
    {
      "error": "Invalid listing data.",
      "details": {
        "title": "Title is too short."
      }
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 300ms for 95% of requests (includes DB write).

**2.2. Get Property Listing (Details)**

  * **API Endpoint:** `GET /api/v1/properties/{property_id}`
  * **Description:** Retrieves detailed information about a specific property.
  * **Authentication:** Optional (guests can view public listings; hosts can view their own).
  * **Input (Path Parameter):** `property_id` (UUID)
  * **Validation Rules:**
      * `property_id`: Required, valid UUID format, must exist.
  * **Output (Success - HTTP 200 OK):**
    ```json
    {
      "id": "uuid-of-property",
      "host_id": "uuid-of-host",
      "title": "Cozy Apartment in City Center",
      "description": "A charming apartment close to all major attractions...",
      "location": {
        "address": "123 Main St",
        "city": "Paris",
        "country": "France",
        "latitude": 48.8566,
        "longitude": 2.3522
      },
      "price_per_night": 150.00,
      "currency": "EUR",
      "max_guests": 4,
      "number_of_beds": 2,
      "number_of_bathrooms": 1,
      "amenities": ["Wi-Fi", "Kitchen", "Air Conditioning"],
      "availability": [
        {"start_date": "2025-08-01", "end_date": "2025-08-10"},
        {"start_date": "2025-09-01", "end_date": "2025-09-05"}
      ],
      "image_urls": ["url1", "url2"],
      "reviews_count": 5,
      "average_rating": 4.5,
      "status": "active",
      "created_at": "2025-07-06T10:10:00Z",
      "updated_at": "2025-07-06T10:15:00Z"
    }
    ```
  * **Output (Error - HTTP 404 Not Found):**
    ```json
    {
      "error": "Property not found."
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 100ms for 99% of requests (should benefit from caching).

**2.3. Update Property Listing**

  * **API Endpoint:** `PUT /api/v1/properties/{property_id}`
  * **Description:** Allows a host to update an existing property listing.
  * **Authentication:** Requires valid Host JWT.
  * **Authorization:** Host can only update their own listings.
  * **Input (Request Body - JSON):** (Partial updates are common, but full PUT shown)
    ```json
    {
      "title": "Spacious Flat near Louvre",
      "price_per_night": 180.00,
      "amenities": ["Wi-Fi", "Kitchen", "Washer", "Dryer"]
      // All other fields are optional for update, if omitted, retain current value
    }
    ```
  * **Validation Rules:**
      * `property_id`: Required, UUID format, must exist, and belong to the authenticated host.
      * All input fields are validated as per "Add Listing" (e.g., `price_per_night` positive number).
  * **Output (Success - HTTP 200 OK):**
    ```json
    {
      "message": "Property listing updated successfully",
      "property": {
        "id": "uuid-of-property",
        "host_id": "uuid-of-host",
        "title": "Spacious Flat near Louvre",
        "price_per_night": 180.00,
        // ... (other updated or retained fields)
        "updated_at": "2025-07-06T10:20:00Z"
      }
    }
    ```
  * **Output (Error - HTTP 400 Bad Request / 403 Forbidden / 404 Not Found):**
    ```json
    {
      "error": "Unauthorized access or invalid data."
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 250ms for 95% of requests.

-----

### **3. Feature: Booking System**

**Description:** This feature enables guests to book properties and manage their reservations.

**Actor:** Guest, Host

**3.1. Create Booking**

  * **API Endpoint:** `POST /api/v1/bookings`
  * **Description:** Allows a guest to book a property for specified dates.
  * **Authentication:** Requires valid Guest JWT.
  * **Input (Request Body - JSON):**
    ```json
    {
      "property_id": "uuid-of-property",
      "check_in_date": "2025-08-01",
      "check_out_date": "2025-08-10",
      "number_of_guests": 2,
      "payment_info": { // Sensitive payment info handled by payment gateway, this is for backend processing
        "token": "stripe_payment_token_from_frontend" // Token from a frontend payment library
      }
    }
    ```
  * **Validation Rules:**
      * `property_id`: Required, UUID format, must exist and be active.
      * `check_in_date`: Required, valid ISO date (YYYY-MM-DD), must be in the future, and available for the property.
      * `check_out_date`: Required, valid ISO date (YYYY-MM-DD), must be after `check_in_date`, and available for the property.
      * `number_of_guests`: Required, positive integer, must not exceed `max_guests` for the property.
      * `payment_info.token`: Required, string (e.g., Stripe token).
      * **Availability Check:** Crucial server-side validation to prevent double bookings. Ensure the requested dates are entirely free for the given property.
  * **Output (Success - HTTP 201 Created):**
    ```json
    {
      "message": "Booking created successfully",
      "booking": {
        "id": "uuid-of-booking",
        "guest_id": "uuid-of-guest",
        "property_id": "uuid-of-property",
        "check_in_date": "2025-08-01",
        "check_out_date": "2025-08-10",
        "total_price": 1350.00, // Calculated: (price_per_night * nights) + fees
        "currency": "EUR",
        "status": "confirmed", // or "pending_payment" if asynchronous
        "created_at": "2025-07-06T10:30:00Z"
      }
    }
    ```
  * **Output (Error - HTTP 400 Bad Request / 404 Not Found / 409 Conflict):**
    ```json
    {
      "error": "Booking failed.",
      "details": "Selected dates are not available for this property."
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 500ms for 95% of requests (due to payment gateway interaction).
      * Concurrency: The booking process must handle concurrent requests for the same property dates without issues (e.g., using database transactions and locking).

**3.2. Cancel Booking**

  * **API Endpoint:** `PUT /api/v1/bookings/{booking_id}/cancel`
  * **Description:** Allows a guest or host to cancel a booking.
  * **Authentication:** Requires valid Guest or Host JWT.
  * **Authorization:** Guest can cancel their own bookings. Host can cancel bookings on their properties. Admin can cancel any.
  * **Input (Path Parameter):** `booking_id` (UUID)
  * **Input (Optional Request Body - JSON):**
    ```json
    {
      "reason": "Host needs to close listing for emergency repairs." // Optional for logging
    }
    ```
  * **Validation Rules:**
      * `booking_id`: Required, UUID format, must exist.
      * Booking must be in a cancellable status (e.g., not "completed").
      * Cancellation policy adherence (e.g., time limits for full refund).
  * **Output (Success - HTTP 200 OK):**
    ```json
    {
      "message": "Booking cancelled successfully",
      "booking": {
        "id": "uuid-of-booking",
        "status": "cancelled",
        "refund_amount": 1000.00, // If applicable
        "cancelled_at": "2025-07-06T10:45:00Z"
      }
    }
    ```
  * **Output (Error - HTTP 400 Bad Request / 403 Forbidden / 404 Not Found):**
    ```json
    {
      "error": "Booking cannot be cancelled at this time."
    }
    ```
  * **Performance Criteria:**
      * Response time: Max 300ms for 95% of requests.

-----