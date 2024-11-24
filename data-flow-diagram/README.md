## Context Diagram
* [Users] ---> [Airbnb Clone System] <--- [Payment Gateway]

## Core Processes
* [Users] ---> (1. Register/Login) ---> [User Database]
* [Users] ---> (2. Manage Properties) ---> [Property Database]
* [Users] ---> (3. Search Properties) ---> [Property Database]
* [Users] ---> (4. Book Property) ---> [Booking Database]
* [Booking_Database] ---> (5. Process Payment) ---> [Payment Gateway]
* [Users] <--- (6. Notifications) <--- [Airbnb Clone System]
* [Admin] ---> (7. Monitor System) ---> [All Databases]


## Detailed Booking Process
* [Users] ---> (Search Properties) ---> [Property Database]
* [Users] ---> (Select Property) ---> [Booking Database]
* [Booking_Database] ---> (Validate Availability) ---> [Property Database]
* [Users] ---> (Make Payment) ---> [Payment Gateway]
* [Payment_Gateway] ---> (Confirm Payment) ---> [Booking Database]
