# 🎬 CineMark Theatre - Movie Ticket Booking System

An interactive, responsive, web-based Movie Ticket Booking System designed using Object-Oriented Analysis and Design (OOAD) principles.

---

### 🏛️ Institution
**K. Ramakrishnan College of Technology (Autonomous), Trichy**  
**Department:** Computer Science and Engineering / Information Technology  
**Course / Subject:** Object-Oriented Analysis and Design (OOAD) Project  

### 👥 Team Members
* **Arul Neelakandan T**
* **Aswanth S**
* **Aswin Kumar MR**
* **Balaji B**

---

## 📑 Table of Contents
1. [Abstract](#-abstract)
2. [Introduction](#-introduction)
3. [Existing System vs Proposed System](#-existing-system-vs-proposed-system)
4. [System Architecture](#-system-architecture)
5. [UML Diagrams](#-uml-diagrams)
   - [Use Case Diagram](#1-use-case-diagram)
   - [Class Diagram](#2-class-diagram)
   - [Sequence Diagram](#3-sequence-diagram)
   - [State Diagram](#4-state-diagram)
   - [Activity Diagram](#5-activity-diagram)
6. [Modules Description](#-modules-description)
7. [Features & Tech Stack](#-features--tech-stack)
8. [Demo Credentials](#-demo-credentials)
9. [How to Run](#-how-to-run)
10. [Conclusion](#-conclusion)

---

## 📄 Abstract
The **Movie Ticket Booking System** is an online web application that allows users to search for movies, check show timings, select tier-based seats (Regular, Premium, VIP), order refreshments, apply coupons, and book tickets conveniently. The system automates theatre operations, ticket management, and simulated payment processing. Using object-oriented concepts and UML design, the application provides a secure, scalable, and user-friendly platform that minimizes manual counter work, eliminates double-booking errors, and provides instant E-Ticket generation with verifiable QR codes.

---

## 📌 Introduction
The system enables customers to browse currently running movies, view show timings, and reserve seats through a single unified digital platform.
* Users can register, log in, select seats in real-time, order food combos, make online payments, and receive booking confirmations instantly.
* All movie schedules, customer profiles, wallet balances, and booking records are stored reliably in centralized persistent client storage (`localStorage`).
* The system reduces waiting time at physical counters, avoids duplicate seat booking conflicts, and supports multi-theatre and show management seamlessly.

---

## ⚖️ Existing System vs Proposed System

| Feature | Existing Manual Counter System | Proposed Online System |
| :--- | :--- | :--- |
| **Booking Mechanism** | Physical queues at ticket counter | 24/7 web-based anytime booking |
| **Seat Availability** | No real-time visual seat availability | Dynamic visual seat layout with tier classification |
| **Double Booking** | High chance of human errors and conflicts | Automated locking & real-time booked status check |
| **Payments** | Cash/manual card swiping at desk | Multiple integrated payment gateways (UPI, Cards, Net Banking) |
| **E-Tickets & QR** | Paper tickets prone to loss or damage | Digital E-Ticket with scannable QR verification |
| **Administration** | Paper registers and manual logs | Centralized Admin Dashboard for movies, analytics & revenue |

---

## 🏗️ System Architecture

```mermaid
flowchart TD
    User["👤 USER\n(Customer / Admin)"] <--> UI["🖥️ USER INTERFACE\n(Login, Browse Movies, Seat Selection,\nBook Ticket, View Ticket)"]
    UI <--> Server["⚙️ BOOKING LOGIC SERVICE\n(Authentication, Movie Management,\nShow Management, Seat Booking)"]
    Server <--> Payment["💳 PAYMENT MODULE\n(UPI, Credit/Debit Card, Net Banking,\nTransaction Verification)"]
    Payment <--> Notification["🔔 NOTIFICATION MODULE\n(Booking Alert, Status Messages,\nTicket Confirmation)"]
    Server --> TicketModule["🎫 TICKET / QR MODULE\n(Generate E-Ticket + QR Code)"]
    TicketModule --> Storage[("💾 PERSISTENT DATABASE\n(Users DB, Movies DB, Shows DB, Bookings DB)")]
```

---

## 📊 UML Diagrams

### 1. Use Case Diagram
```mermaid
flowchart LR
    subgraph System ["🎬 CineMark Movie Booking System"]
        UC_Reg((Register))
        UC_Login((Login))
        UC_Logout((Logout))
        UC_Browse((Browse / Search Movies))
        UC_Book((Book Ticket & Select Seats))
        UC_Food((Order Food / Beverages))
        UC_Pay((Make Payment))
        UC_History((View Bookings / Cancel))
        UC_Review((Rate & Review Movie))
        UC_Wallet((Manage Wallet))
        UC_AddMovie((Add / Delete Movies))
        UC_Dashboard((View Analytics & Revenue))
        UC_ManageBookings((View All Customer Bookings))
    end

    Customer["👤 Customer"]
    Admin["👨‍💼 Administrator"]

    Customer --> UC_Reg
    Customer --> UC_Login
    Customer --> UC_Browse
    Customer --> UC_Book
    Customer --> UC_Food
    Customer --> UC_Pay
    Customer --> UC_History
    Customer --> UC_Review
    Customer --> UC_Wallet
    Customer --> UC_Logout

    Admin --> UC_Login
    Admin --> UC_AddMovie
    Admin --> UC_Dashboard
    Admin --> UC_ManageBookings
    Admin --> UC_Logout
```

### 2. Class Diagram
```mermaid
classDiagram
    class User {
        +int id
        +string name
        +string email
        +string password
        +string phone
        +string role
        +float wallet
        +list favorites
        +list reviews
        +register()
        +login()
        +addWallet(amount)
    }

    class Movie {
        +int id
        +string name
        +string genre
        +string rating
        +string duration
        +float price
        +string emoji
        +int year
    }

    class Booking {
        +string id
        +int userId
        +string customer
        +string phone
        +string movie
        +int movieId
        +string theatre
        +string date
        +string show
        +list seats
        +list food
        +float discount
        +float total
        +string paymentMethod
        +string transaction
        +string paymentStatus
        +string status
        +string createdAt
    }

    class Seat {
        +string number
        +string type
        +float price
        +bool isBooked
    }

    class Review {
        +string movie
        +int rating
        +string text
        +string date
    }

    User "1" --> "*" Booking : creates
    User "1" --> "*" Review : writes
    Booking "1" --> "1" Movie : references
    Booking "1" --> "*" Seat : reserves
```

### 3. Sequence Diagram
```mermaid
sequenceDiagram
    autonumber
    actor Customer
    participant App as Web Application
    participant BookingService as Booking Controller
    participant DB as LocalStorage DB
    participant PaymentGateway as Payment Gateway
    participant QRService as QR Code Generator

    Customer->>App: Login(email, password)
    App->>DB: Validate user credentials
    DB-->>App: User authenticated
    Customer->>App: Browse Movies & Select Show Details
    App->>DB: Fetch seat availability for (Movie, Theatre, Date, Show)
    DB-->>App: Return booked and available seats
    Customer->>App: Pick Seats (Regular / Premium / VIP) + Food items
    Customer->>App: Click 'Proceed to Payment'
    Customer->>PaymentGateway: Select Payment Method (UPI / Card / Net Banking) & Pay
    PaymentGateway-->>BookingService: Payment Confirmed (TXN ID)
    BookingService->>DB: Save Booking (Seats, Customer, Amount, Status)
    BookingService->>QRService: Generate QR Code (Booking ID + Details)
    QRService-->>App: Render Verified E-Ticket Modal
    App-->>Customer: Display Booking Confirmation & Printable Ticket
```

### 4. State Diagram
```mermaid
stateDiagram-v2
    [*] --> Available : Show Initialized
    Available --> Selected : User selects seat
    Selected --> Available : User deselects seat
    Selected --> InPayment : Proceed to checkout
    InPayment --> Available : Payment timed out / Cancelled
    InPayment --> Booked : Payment confirmed
    Booked --> Cancelled : User requests cancellation
    Cancelled --> Available : Seat released for booking
    Booked --> [*] : Show completed
```

### 5. Activity Diagram
```mermaid
flowchart TD
    Start([Start]) --> Login[User Login / Registration]
    Login --> Browse[Browse Movies & Select Title]
    Browse --> SelectShow[Select Theatre, Date & Show Time]
    SelectShow --> CheckSeats{Seat Available?}
    CheckSeats -- No --> PromptChange[Select Another Show / Time]
    PromptChange --> SelectShow
    CheckSeats -- Yes --> SelectSeats[Choose Preferred Seats]
    SelectSeats --> AddFood[Select Food / Add-ons & Apply Coupon]
    AddFood --> PayPrompt[Select Payment Mode]
    PayPrompt --> PayCheck{Payment Successful?}
    PayCheck -- No --> PayFailed[Show Payment Failed Alert]
    PayFailed --> PayPrompt
    PayCheck -- Yes --> GenTicket[Generate E-Ticket & Dynamic QR Code]
    GenTicket --> SendNotification[Display Confirmation Toast & Save History]
    SendNotification --> End([End])
```

---

## 🧩 Modules Description

1. **User Management Module:**
   - Registration and secure authentication for customers and administrators.
   - Profile management with editable personal details.
   - Integrated CineMark Wallet balance system with instant top-up options.

2. **Movie Management Module:**
   - Display catalog of latest releases with ratings, duration, genre, and starting price.
   - Real-time movie search filter by title or genre.
   - Admin features to dynamically add new movies or remove old titles.

3. **Theatre and Show Management Module:**
   - Supports multiple cinema locations (PVR Cinemas, INOX Phoenix, AGS Cinemas, Rohini Silver Screens, Galaxy Multiplex).
   - Date picker with past date restrictions.
   - Multiple daily showtimes (10:00 AM, 01:30 PM, 04:30 PM, 07:30 PM, 10:30 PM).

4. **Seat Booking Module:**
   - Interactive 100-seat theatre auditorium grid (Rows A-J, Columns 1-10).
   - Multi-tier seat pricing:
     - **Regular:** Rows A-C (₹150)
     - **Premium:** Rows D-G (₹200)
     - **VIP:** Rows H-J (₹300)
   - Real-time conflict avoidance (already booked seats are locked and highlighted in red).

5. **Refreshment & Food Order Module:**
   - Add popcorn, beverage combos, nachos, burgers, pizzas, and ice creams directly to the cart.
   - Discount coupons: `CINE50` (₹50 off), `CINE100` (₹100 off on orders ₹500+), `MOVIE20` (20% off).

6. **Payment Module:**
   - Simulated payment modal supporting UPI, Credit Card, Debit Card, and Net Banking.
   - Automatic unique Transaction ID generation.

7. **Ticket Generation & QR Module:**
   - Instant digital ticket generation with customer info, movie details, seat numbers, and total paid.
   - Real-time QR Code rendering powered by `qrcode.js`.
   - Browser print functionality for physical or PDF ticket export.

8. **Admin Dashboard Module:**
   - Live metrics: Total Users, Total Bookings, Confirmed Bookings, Cancelled Bookings, and Total Revenue (₹).
   - Full master table of all bookings with real-time status.

---

## 💻 Features & Tech Stack

* **Frontend:** HTML5, CSS3 (Modern dark-mode UI with responsive CSS Grid & Flexbox)
* **Logic & Scripts:** Vanilla JavaScript (ES6+ Classes, Event-driven architecture)
* **Libraries:** [QRCode.js](https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js) for high-resolution client-side QR generation
* **Storage:** Browser `localStorage` for offline-capable, zero-backend persistence

---

## 🔑 Demo Credentials

| Role | Email | Password |
| :--- | :--- | :--- |
| **Customer** | `user@gmail.com` | `1234` |
| **Administrator** | `admin@cinemark.com` | `admin123` |

---

## 🚀 How to Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/aswinkumarmr2007-web/Movie-Ticket-Booking-System.git
   cd Movie-Ticket-Booking-System
   ```

2. **Open the application:**
   - Simply double-click `index.html` or open it in any modern browser (Chrome, Firefox, Edge, Safari).
   - Alternatively, serve via VS Code Live Server or Python HTTP server:
     ```bash
     python -m http.server 8000
     ```
   - Open `http://localhost:8000` in your browser.

---

## 🏁 Conclusion
The Movie Ticket Booking System provides an efficient, reliable, and user-friendly solution for managing movie ticket reservations. It automates booking, payment, and ticket generation while reducing manual work and booking errors. The centralized system improves customer convenience, enhances theatre management, and supports future expansion with online payment gateways, multiple theatre management, and real-time seat availability.
