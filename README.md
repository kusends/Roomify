# Roomify

**Project Status:** In Development. Currently, only the Frontend of the Admin Panel is implemented.

Roomify is a cross-platform short-term rental system connecting private hosts and guests. The platform focuses on the local market and provides a full interaction cycle: from search and booking to payment and reviews.

## Functional Overview

The project is divided into three logical modules according to user roles.

### For Guests

* Search by geolocation (OpenStreetMap), price, accommodation type, and amenities.
* Real-time availability check, cost calculation including service fees.
* Stripe integration for online payments or cash-on-arrival option.
* Built-in WebSocket chat for communication with the host.
* Trip history view, review system.

### For Hosts

* CRUD operations for listings (photos, descriptions, rules).
* Manage available dates, block periods, synchronize statuses.
* Set prices, view booking history.
* Tools to increase listing visibility.

### Admin Panel (Admin/Moderator)

* Validation of new listings, complaint handling.
* Dashboard with metrics (user count, transactions, conversion).
* Edit housing categories and search attributes.

## Technology Stack

The architecture follows the separation of the client side and RESTful API.

### Backend

* **Language:** Go (Golang)
* **Framework:** Goyave (REST API)
* **Databases:**
  * **PostgreSQL:** Primary storage (users, bookings, transactions).
  * **MongoDB:** Unstructured data (logs, listing details).
  * **Redis:** Caching sessions and hot data.
* **Message Broker:** RabbitMQ (asynchronous task processing, notifications).
* **Real-time:** Gorilla/websocket.

### Frontend

* **Web (Admin Panel):** React.
* **Mobile App:** React Native (iOS & Android).

### External Services & APIs

* **Maps:** OpenStreetMap + Leaflet + Nominatim (geocoding).
* **Auth:** OAuth 2.0 (Google), JWT (JSON Web Tokens).
* **Notifications:** Twilio (SMS, OTP).
* **Payments:** Stripe Connect.

## Business Logic & Rules

The system implements the following key algorithms:

1. **Cost Calculation:**
   `Total Cost = (Daily_Price * Days_Count) + 15% Service Fee`.

2. **Booking Timeout:**
   When online payment is selected, dates are blocked for 24 hours. If payment is not received, the status changes to "Cancelled" and dates are released.

3. **Listing Conversion:**
   `(Bookings / Unique Views) * 100%`.

4. **Rating:**
   Arithmetic mean of all verified reviews.

## Environment Requirements

To deploy the server side, the following are required:

* Go 1.x
* PostgreSQL 13+
* MongoDB 5+
* Redis 6+
* RabbitMQ 3+

## Installation & Setup

1. Clone the repository:
   ```bash
   git clone [https://github.com/username/roomify.git](https://github.com/username/roomify.git)

2. Configure environment variables (.env):
* DB connection config (Postgres, Mongo, Redis).
* API keys for Stripe, Twilio, Google Auth.

3. Start services (using Docker Compose):
   ```bash
   docker-compose up -d
4. Run database migrations:
   ```bash
   go run main.go migrate
