# ⚡ Electricity Billing System

A desktop-based **Electricity Billing System** built with **Java Swing** and **MySQL**. It provides a complete billing solution for an electricity utility company, supporting both **Admin** and **Customer** roles through an intuitive graphical interface.

---

## 📋 Table of Contents

- [Project Description](#-project-description)
- [Features](#-features)
- [Technologies Used](#-technologies-used)
- [Database Schema](#-database-schema)
- [Prerequisites](#-prerequisites)
- [Setup Instructions](#-setup-instructions)
- [Running the Application](#-running-the-application)
- [Screenshots](#-screenshots)
- [Project Structure](#-project-structure)

---

## 📖 Project Description

The Electricity Billing System is a Java desktop application that automates electricity billing operations. It enables administrators to manage customer records, configure meter information, calculate monthly bills based on configurable tax rates, and track payment statuses. Customers can view their bill details, generate printable bills, and pay outstanding dues — all through a clean Swing-based GUI.

---

## ✨ Features

### 👨‍💼 Admin
| Feature | Description |
|---|---|
| **New Customer** | Register a new customer with personal details and auto-generated meter number |
| **Meter Information** | Configure meter location, type, phase code, and billing type for a customer |
| **Customer Details** | Browse all registered customer records |
| **Deposit Details** | View deposit/payment history across customers |
| **Calculate Bill** | Compute monthly electricity bill for a customer based on units consumed and tax table |

### 👤 Customer
| Feature | Description |
|---|---|
| **Generate Bill** | View a detailed, printable bill for any month |
| **Pay Bill** | Pay an outstanding bill and update its status to "Paid" |
| **Bill Details** | Review billing history by month |
| **View Information** | View personal profile and meter details |
| **Update Information** | Edit personal account information |

### 🛠️ Utility (All Users)
- **Notepad** — Launch Windows Notepad
- **Calculator** — Launch Windows Calculator

---

## 🛠️ Technologies Used

| Technology | Version / Details |
|---|---|
| **Java** | JDK 8 or later (Swing & AWT GUI) |
| **MySQL** | 5.7 / 8.x |
| **JDBC** | `mysql-connector-java.jar` (bundled in `lib/`) |
| **IDE** | IntelliJ IDEA (`.iml` project file included) |

---

## 🗄️ Database Schema

Database name: **`Bill_system`**

### `new_customer`
| Column | Type | Description |
|---|---|---|
| `name` | VARCHAR | Customer full name |
| `meter_no` | VARCHAR | Unique meter number (auto-generated) |
| `address` | VARCHAR | Street address |
| `city` | VARCHAR | City |
| `state` | VARCHAR | State |
| `email` | VARCHAR | Email address |
| `phone_no` | VARCHAR | Phone number |

### `Signup`
| Column | Type | Description |
|---|---|---|
| `meter_no` | VARCHAR | Meter number (links to `new_customer`) |
| `username` | VARCHAR | Login username |
| `name` | VARCHAR | User display name |
| `password` | VARCHAR | Login password |
| `usertype` | VARCHAR | `Admin` or `Customer` |

### `meter_info`
| Column | Type | Description |
|---|---|---|
| `meter_number` | VARCHAR | Meter number (links to `new_customer`) |
| `meter_location` | VARCHAR | `Outside` / `Inside` |
| `meter_type` | VARCHAR | `Electric Meter` / `Solar Meter` / `Smart Meter` |
| `phase_code` | VARCHAR | Phase code (e.g., `011`, `022`, …) |
| `bill_type` | VARCHAR | `Normal` / `Industrial` |
| `Days` | INT | Billing period days (default: 30) |

### `bill`
| Column | Type | Description |
|---|---|---|
| `meter_no` | VARCHAR | Meter number |
| `month` | VARCHAR | Billing month (e.g., `January`) |
| `unit` | INT | Units consumed |
| `total_bill` | INT | Total amount due |
| `status` | VARCHAR | `Not Paid` / `Paid` |

### `tax`
| Column | Type | Description |
|---|---|---|
| `cost_per_unit` | INT | Per-unit electricity cost |
| `meter_rent` | INT | Fixed meter rent charge |
| `service_charge` | INT | Service charge |
| `service_tax` | INT | Service tax |
| `swacch_bharat` | INT | Swachh Bharat cess |
| `fixed_tax` | INT | Fixed tax |

---

## ✅ Prerequisites

- **Java JDK 8+** installed and `JAVA_HOME` set
- **MySQL Server** (5.7 or 8.x) running on `localhost:3306`
- **MySQL Workbench** or any SQL client (optional, for DB setup)
- **IntelliJ IDEA** (recommended) or any Java IDE

---

## 🚀 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/TahaXCoder/Electricity-Billing-System.git
cd Electricity-Billing-System
```

### 2. Configure MySQL

Open your MySQL client and run the following SQL to create the database and tables:

```sql
CREATE DATABASE Bill_system;
USE Bill_system;

CREATE TABLE new_customer (
    name       VARCHAR(100),
    meter_no   VARCHAR(100) PRIMARY KEY,
    address    VARCHAR(200),
    city       VARCHAR(100),
    state      VARCHAR(100),
    email      VARCHAR(100),
    phone_no   VARCHAR(20)
);

CREATE TABLE Signup (
    meter_no   VARCHAR(100),
    username   VARCHAR(100),
    name       VARCHAR(100),
    password   VARCHAR(100),
    usertype   VARCHAR(20)
);

CREATE TABLE meter_info (
    meter_number    VARCHAR(100),
    meter_location  VARCHAR(50),
    meter_type      VARCHAR(50),
    phase_code      VARCHAR(10),
    bill_type       VARCHAR(50),
    Days            INT
);

CREATE TABLE bill (
    meter_no    VARCHAR(100),
    month       VARCHAR(20),
    unit        INT,
    total_bill  INT,
    status      VARCHAR(20)
);

CREATE TABLE tax (
    cost_per_unit   INT,
    meter_rent      INT,
    service_charge  INT,
    service_tax     INT,
    swacch_bharat   INT,
    fixed_tax       INT
);

-- Insert default tax rates (adjust values as needed)
INSERT INTO tax VALUES (5, 100, 50, 20, 10, 30);

-- Create an Admin account (seed data)
INSERT INTO Signup VALUES ('ADMIN001', 'admin', 'Administrator', 'admin123', 'Admin');
```

### 3. Update Database Credentials

Open `src/electricity/billing/system/database.java` and update the connection URL, username, and password to match your MySQL setup:

```java
connection = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/Bill_system",
    "root",       // ← your MySQL username
    "yourpassword" // ← your MySQL password
);
```

### 4. Add the JDBC Driver

The MySQL connector JAR is already bundled in the `lib/` directory (`mysql-connector-java.jar`). Ensure it is on the classpath:

- **IntelliJ IDEA**: Go to **File → Project Structure → Modules → Dependencies**, click **+**, choose **JARs or directories**, and add `lib/mysql-connector-java.jar`.

### 5. Open in IDE

Open the project root folder in **IntelliJ IDEA**. The `.iml` project file will be detected automatically.

---

## ▶️ Running the Application

Set `electricity.billing.system.Splash` (or `Login`) as the **main class** and run the project.

- The **Splash screen** displays for 3 seconds, then the **Login** window opens.
- Log in as **Admin** to manage customers and billing.
- Log in as **Customer** to view and pay bills.

> **Note:** The Utility menu's Notepad and Calculator features launch native Windows applications (`notepad.exe` / `calc.exe`) and require a Windows OS.

---

## 📸 Screenshots

> _Add your screenshots to a `screenshots/` folder and update the links below._

| Splash Screen | Login |
|:---:|:---:|
| ![Splash](screenshots/splash.png) | ![Login](screenshots/login.png) |

| Admin Dashboard | New Customer |
|:---:|:---:|
| ![Dashboard](screenshots/dashboard.png) | ![New Customer](screenshots/new_customer.png) |

| Calculate Bill | Generate Bill |
|:---:|:---:|
| ![Calculate Bill](screenshots/calculate_bill.png) | ![Generate Bill](screenshots/generate_bill.png) |

| Pay Bill | Bill Details |
|:---:|:---:|
| ![Pay Bill](screenshots/pay_bill.png) | ![Bill Details](screenshots/bill_details.png) |

---

## 📁 Project Structure

```
Electricity-Billing-System/
├── lib/
│   └── mysql-connector-java.jar       # JDBC driver
├── src/
│   ├── electricity/billing/system/
│   │   ├── Splash.java                # Splash screen (entry point)
│   │   ├── Login.java                 # Login window
│   │   ├── Signup.java                # Signup / account creation
│   │   ├── main_class.java            # Main application window & menu
│   │   ├── database.java              # MySQL connection helper
│   │   ├── newCustomer.java           # Register new customer
│   │   ├── meterInfo.java             # Meter information setup
│   │   ├── customer_details.java      # View all customers
│   │   ├── deposit_details.java       # Deposit/payment details
│   │   ├── calculate_bill.java        # Calculate monthly bill
│   │   ├── generate_bill.java         # Generate printable bill
│   │   ├── pay_bill.java              # Pay outstanding bill
│   │   ├── payment_bill.java          # Payment confirmation
│   │   ├── bill_details.java          # Bill history for customer
│   │   ├── view_information.java      # View customer profile
│   │   └── update_information.java    # Update customer profile
│   └── icon/                          # Application icons & images
└── Electiricity Billing System.iml    # IntelliJ project file
```

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
