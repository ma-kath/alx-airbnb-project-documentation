
---

## Airbnb Clone Backend: User Stories

These user stories capture the core interactions between the human actors (Guest, Host, Admin) and the system, derived directly from the core functionalities.

### **1. User Management**

* **As a new user, I want to be able to register an account** so that I can either list my property or book a place to stay.
* **As a registered user, I want to be able to log in with my email and password** so that I can access my account and use the platform's features.
* **As a registered user, I want to be able to log in using my Google or Facebook account** so that I can quickly and conveniently access the platform without creating a new password. (Extends "User Login")
* **As a user, I want to be able to update my profile information (e.g., contact info, preferences)** so that my account details are accurate and up-to-date.
* **As a user, I want to be able to upload a profile photo** so that I can personalize my profile and be recognized by others. (Extends "Manage Profile")

### **2. Property Listings Management**

* **As a host, I want to be able to add a new property listing with details like title, description, location, price, amenities, and availability** so that I can make my property available for guests to book.
* **As a host, I want to be able to edit my existing property listings** so that I can keep the information accurate and up-to-date.
* **As a host, I want to be able to delete my property listings** so that I can remove properties that are no longer available or relevant.

### **3. Search and Filtering**

* **As a guest, I want to be able to search for properties by location, price range, number of guests, and specific amenities** so that I can easily find a place that meets my specific needs.
* **As a guest, I want to see search results with pagination** so that I can efficiently browse through many properties without overwhelming the page.

### **4. Booking Management**

* **As a guest, I want to be able to book a property for specific dates** so that I can secure my accommodation.
* **As a guest, I want the system to prevent double bookings on selected dates** so that I only see available properties and avoid conflicts. (Implicitly "includes" date validation within "Create Booking")
* **As a guest, I want to be able to cancel my booking** so that I can adjust my travel plans if needed.
* **As a host, I want to be able to cancel a guest's booking (based on policy)** so that I can manage my property's availability in unforeseen circumstances.
* **As a guest, I want to be able to view the status of my bookings (e.g., pending, confirmed, canceled, completed)** so that I can stay informed about my reservations.
* **As a host, I want to be able to view the status of all bookings for my properties** so that I can manage my rentals effectively.

### **5. Payment Integration**

* **As a guest, I want to be able to make an upfront payment securely for my booking** so that my reservation is confirmed.
* **As a host, I want to receive automatic payouts after a booking is completed** so that I am compensated for my property rentals.
* **As a guest, I want to be able to see prices and make payments in multiple currencies** so that I can easily understand costs and pay in my preferred currency.

### **6. Reviews and Ratings**

* **As a guest, I want to be able to leave reviews and ratings for properties I have stayed in** so that I can share my experience and help future guests make informed decisions.
* **As a host, I want to be able to respond to reviews left on my properties** so that I can address feedback and engage with guests.

### **7. Notifications System**

* **As a user (guest or host), I want to receive email notifications for booking confirmations, cancellations, and payment updates** so that I am always informed about the status of my interactions with the platform.
* **As a user (guest or host), I want to receive in-app notifications for important updates** so that I can quickly see relevant information while using the application.

### **8. Admin Dashboard**

* **As an admin, I want to be able to monitor and manage all users on the platform** so that I can maintain a healthy and secure user base.
* **As an admin, I want to be able to monitor and manage all property listings** so that I can ensure content quality and compliance.
* **As an admin, I want to be able to monitor and manage all bookings** so that I can resolve issues and ensure smooth operations.
* **As an admin, I want to be able to monitor and manage all payment transactions** so that I can track financial flows and address discrepancies.