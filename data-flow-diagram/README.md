
### Level 0 DFD: Context Diagram

1. **Central Process:**
   - Draw a large circle labeled **"Airbnb Backend System"**.

2. **External Entities:**
   - Draw five rectangles around the central process:
     - **[Guest]**
     - **[Host]**
     - **[Admin]**
     - **[Payment Gateway]**
     - **[Email Service]**

3. **Data Flows:**
   - Draw arrows from the external entities to the central process and label them with the corresponding data flows:
     - From **[Guest]** to **(Airbnb Backend System)**:
       - Registration Data
       - Login Credentials
       - Profile Updates
       - Search Criteria
       - Booking Request (with Dates & Property ID)
       - Booking Cancellation Request
       - Review Submission (Text, Rating)
     - From **(Airbnb Backend System)** to **[Guest]**:
       - Login Confirmation/Error
       - Profile Update Confirmation
       - Property Search Results
       - Booking Confirmation Details
       - Cancellation Confirmation
       - Payment Status Updates
       - Notifications (In-App)
       - Review Submission Confirmation
     - Repeat similar steps for **[Host]**, **[Admin]**, **[Payment Gateway]**, and **[Email Service]**.

### Level 1 DFD: Decomposition of (Airbnb Backend System)

1. **Internal Processes:**
   - Draw eight circles/rectangles for the internal processes:
     - **(1.0 User Management)**
     - **(2.0 Property Listings Management)**
     - **(3.0 Search & Filtering)**
     - **(4.0 Booking Management)**
     - **(5.0 Payment Integration)**
     - **(6.0 Reviews & Ratings)**
     - **(7.0 Notifications System)**
     - **(8.0 Admin Dashboard)**

2. **Data Stores:**
   - Draw five open-ended rectangles for the data stores:
     - **[D1 User Data Store]**
     - **[D2 Property Data Store]**
     - **[D3 Booking Data Store]**
     - **[D4 Review Data Store]**
     - **[D5 Payment Transaction Data Store]**

3. **Data Flows:**
   - Connect the internal processes with arrows to represent data flows, labeling them as specified in the description. For example:
     - For **(1.0 User Management)**:
       - From **[Guest]/[Host]** to **(1.0 User Management)**: Registration Data, Login Credentials, Profile Updates
       - From **(1.0 User Management)** to **[Guest]/[Host]**: Login Confirmation/Error, Profile Update Confirmation
       - Between **(1.0 User Management)** and **[D1 User Data Store]**: User Account Details (Read/Write)
     - Repeat similar steps for each internal process, ensuring to include interactions with data stores and external entities where applicable.

### Example Visual Representation

Here’s a simplified textual representation of how the DFD might look:

```
Level 0 DFD:
[Guest] --> (Airbnb Backend System) --> [Guest]
[Host] --> (Airbnb Backend System) --> [Host]
[Admin] --> (Airbnb Backend System) --> [Admin]
[Payment Gateway] --> (Airbnb Backend System) --> [Payment Gateway]
(Airbnb Backend System) --> [Email Service]

Level 1 DFD:
[Guest]/[Host] --> (1.0 User Management) --> [D1 User Data Store]
(1.0 User Management) --> [Guest]/[Host]
(2.0 Property Listings Management) --> [D2 Property Data Store]
(3.0 Search & Filtering) --> [D2 Property Data Store]
(4.0 Booking Management) --> [D3 Booking Data Store]
(5.0 Payment Integration) --> [D5 Payment Transaction Data Store]
(6.0 Reviews & Ratings) --> [D4 Review Data Store]
(7.0 Notifications System) --> [D1 User Data Store]
(8.0 Admin Dashboard) <--> [D1 User Data Store]
```

### How to Use This Blueprint in Draw.io

1. **Start with Level 0:** 
   - Create a large circle for "Airbnb Backend System" and place the five external entities around it. Draw arrows with labels for data flows.

2. **Move to Level 1:** 
   - Create a new page or clear the canvas. Draw the eight internal processes and five data stores. Connect them with arrows, labeling each data flow as specified.
