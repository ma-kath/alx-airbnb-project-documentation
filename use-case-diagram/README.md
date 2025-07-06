
---

### **Actors Identified:**
* **Guest**
* **Host**
* **Admin**

Actors are external entities that interact with the system. From your requirements, the key actors are:

Guest: A user who books properties.

Host: A user who lists properties.

Admin: A user who manages the system.

---

### **1. User Management**

**Actor: Guest, Host**

* **Main Use Case: User Registration**
    * **Actors:** Guest, Host
    * **Includes:**
        * `«include»` **Perform Secure Authentication (JWT)**
    * **Extends:**
        * *(No direct `«extend»` specific to core functionality as per list, but `OAuth Login` is a potential extension of the broader 'Login' process)*

* **Main Use Case: User Login**
    * **Actors:** Guest, Host
    * **Includes:**
        * `«include»` **Perform Secure Authentication (JWT)**
    * **Extends:**
        * `«extend»` **Login via OAuth (Google, Facebook)** (condition: user chooses OAuth option)

* **Main Use Case: Manage Profile**
    * **Actors:** Guest, Host
    * **Includes:**
        * `«include»` **Update Profile Information**
        * `«include»` **Update Contact Information**
    * **Extends:**
        * `«extend»` **Upload Profile Photo** (condition: user updates photo)
        * `«extend»` **Update Preferences** (condition: user updates preferences)

---

### **2. Property Listings Management**

**Actor: Host**

* **Main Use Case: Add Property Listing**
    * **Actor:** Host
    * **Includes:**
        * `«include»` **Provide Listing Details** (title, description, location, price, amenities, availability)
        * `«include»` **Validate Listing Data**
    * **Extends:**
        * *(No direct `«extend»` listed in core functionalities)*

* **Main Use Case: Edit Property Listing**
    * **Actor:** Host
    * **Includes:**
        * `«include»` **Retrieve Listing Details**
        * `«include»` **Update Listing Details**
        * `«include»` **Validate Listing Data**
    * **Extends:**
        * *(No direct `«extend»` listed)*

* **Main Use Case: Delete Property Listing**
    * **Actor:** Host
    * **Includes:**
        * `«include»` **Confirm Deletion**
    * **Extends:**
        * *(No direct `«extend»` listed)*

---

### **3. Search and Filtering**

**Actor: Guest**

* **Main Use Case: Search Properties**
    * **Actor:** Guest
    * **Includes:**
        * `«include»` **Define Search Criteria** (location, price range, #guests, amenities)
        * `«include»` **Display Search Results**
        * `«include»` **Apply Pagination** (for large datasets)
    * **Extends:**
        * *(No direct `«extend»` listed)*

---

### **4. Booking Management**

**Actor: Guest, Host**

* **Main Use Case: Create Booking**
    * **Actor:** Guest
    * **Includes:**
        * `«include»` **Select Property Dates**
        * `«include»` **Validate Available Dates** (prevent double bookings)
        * `«include»` **Handle Payment Process** (This use case now represents the system's internal handling of payment, rather than direct actor interaction with a gateway)
        * `«include»` **Update Booking Status (to Confirmed/Pending)**
        * `«include»` **Generate Booking Confirmation Notification**
    * **Extends:**
        * *(No direct `«extend»` listed)*

* **Main Use Case: Cancel Booking**
    * **Actors:** Guest, Host
    * **Includes:**
        * `«include»` **Update Booking Status (to Canceled)**
        * `«include»` **Apply Cancellation Policy**
        * `«include»` **Handle Refund Process (if applicable)**
        * `«include»` **Generate Cancellation Notification**
    * **Extends:**
        * *(No direct `«extend»` listed)*

* **Main Use Case: Track Booking Status**
    * **Actors:** Guest, Host
    * **Includes:**
        * `«include»` **Retrieve Booking Details**
    * **Extends:**
        * *(No direct `«extend»` listed)*

---

### **5. Payment Integration**

**Actor: (No direct human actor initiating this as a standalone use case; it's `«include»`d by others)**

* **Main Use Case: Handle Payment Process** (System-level use case, `«include»`d by `Create Booking` and `Cancel Booking`)
    * **Actors:** None (represents internal system functionality triggered by human actors)
    * **Includes:**
        * `«include»` **Manage Upfront Payments**
        * `«include»` **Manage Automatic Payouts**
        * `«include»` **Handle Multiple Currencies**
    * **Extends:**
        * *(No direct `«extend»` listed)*

---

### **6. Reviews and Ratings**

**Actor: Guest, Host**

* **Main Use Case: Leave Review and Rating**
    * **Actor:** Guest
    * **Includes:**
        * `«include»` **Submit Review Text**
        * `«include»` **Submit Rating Score**
        * `«include»` **Link Review to Booking**
    * **Extends:**
        * *(No direct `«extend»` listed)*

* **Main Use Case: Respond to Review**
    * **Actor:** Host
    * **Includes:**
        * `«include»` **Submit Response Text**
    * **Extends:**
        * *(No direct `«extend»` listed)*

---

### **7. Notifications System**

**Actor: (No direct human actor initiating this as a standalone use case; it's `«include»`d by others)**

* **Main Use Case: Generate Notification** (System-level use case, `«include»`d by others)
    * **Actors:** None (represents internal system functionality triggered by human actors' actions)
    * **Includes:**
        * `«include»` **Generate Notification Content**
    * **Extends:**
        * *(No direct `«extend»` listed)*

    *This `Generate Notification` use case will be `«include»`d by specific events:*
    * `Create Booking` `«include»` `Generate Notification` (Booking Confirmations)
    * `Cancel Booking` `«include»` `Generate Notification` (Cancellations)
    * `Handle Payment Process` `«include»` `Generate Notification` (Payment Updates)

---

### **8. Admin Dashboard**

**Actor: Admin**

* **Main Use Case: Manage Users**
    * **Actor:** Admin
    * **Includes:**
        * `«include»` **View User List**
        * `«include»` **Edit User Details**
        * `«include»` **Delete User Accounts**
    * **Extends:**
        * *(No direct `«extend»` listed)*

* **Main Use Case: Manage Listings**
    * **Actor:** Admin
    * **Includes:**
        * `«include»` **View All Listings**
        * `«include»` **Edit Listing Details**
        * `«include»` **Remove Listings**
    * **Extends:**
        * *(No direct `«extend»` listed)*

* **Main Use Case: Manage Bookings**
    * **Actor:** Admin
    * **Includes:**
        * `«include»` **View All Bookings**
        * `«include»` **Update Booking Status**
        * `«include»` **Cancel Bookings** (potentially, overriding user actions)
    * **Extends:**
        * *(No direct `«extend` listed)*

* **Main Use Case: Manage Payments (Admin View)**
    * **Actor:** Admin
    * **Includes:**
        * `«include»` **View All Payment Transactions**
        * `«include»` **Monitor Payouts**
    * **Extends:**
        * *(No direct `«extend»` listed)*

---