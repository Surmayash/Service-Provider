# 🛠️ Service Provider Booking System

This is a Java Servlet + MySQL-based web application built during my internship at Softvyom Consulting Services Pvt. Ltd. The project allows users to book various service providers like maids, drivers, and tutors, with a backend system for login, booking management, admin view and worker panel.

---

## 📌 Features

- User Registration and Login (via Servlet & MySQL)
- Service Booking Form
- Admin View for All Bookings and User or Worker details
- Simple front-end using HTML, CSS & Js
- Fully Responsive

---

## 🧪 Screenshorts
<img width="1919" height="826" alt="Screenshot 2025-07-26 170725" src="https://github.com/user-attachments/assets/36464cde-a6bd-4e07-b851-babc71608d9b" />
<img width="1919" height="828" alt="Screenshot 2025-07-26 170749" src="https://github.com/user-attachments/assets/af0c9bc1-a4b4-4361-9034-d57db8fe15e0" />
<img width="1919" height="826" alt="Screenshot 2025-07-26 170806" src="https://github.com/user-attachments/assets/ad49e2ab-34dd-477b-9eb9-87522827d74e" />
<img width="1919" height="819" alt="Screenshot 2025-07-26 170829" src="https://github.com/user-attachments/assets/6421e9a6-a597-4af9-9d11-3b7db60952af" />
<img width="1919" height="817" alt="Screenshot 2025-07-26 170847" src="https://github.com/user-attachments/assets/1656b1cb-d0e6-4cf1-8c0a-9d6e10bfa1ff" />
<img width="1918" height="830" alt="Screenshot 2025-07-26 170908" src="https://github.com/user-attachments/assets/318b0135-54b4-4afb-965c-8b54548e329c" />
<img width="1919" height="821" alt="Screenshot 2025-07-26 170926" src="https://github.com/user-attachments/assets/5f77eb6a-c28d-480d-b123-4195abb21334" />
<img width="1919" height="825" alt="Screenshot 2025-07-26 170944" src="https://github.com/user-attachments/assets/9e2ca02e-d3a9-429c-88e8-0d0c1a33225e" />
<img width="1919" height="821" alt="Screenshot 2025-07-26 171006" src="https://github.com/user-attachments/assets/7f31df81-8b5b-406f-a123-5bae6f5c3f06" />
<img width="1919" height="820" alt="Screenshot 2025-07-26 171034" src="https://github.com/user-attachments/assets/9d5a97f9-9fef-481c-9a73-24727e28224c" />
<img width="1919" height="819" alt="Screenshot 2025-07-26 171049" src="https://github.com/user-attachments/assets/9690d93f-c03a-40a6-9afd-950351e85189" />
<img width="1919" height="821" alt="Screenshot 2025-07-26 171105" src="https://github.com/user-attachments/assets/e1d3d234-fd2e-4022-a0a7-094b643ab114" />
<img width="1918" height="823" alt="Screenshot 2025-07-26 171139" src="https://github.com/user-attachments/assets/f7b3b6a5-a6c5-415a-bf7e-0355b7a3c70a" />

---

## 🧠 Technologies Used

- Java (Core + Servlet)
- HTML, CSS, JavaScript
- MySQL
- Apache Tomcat (Server)
- Eclipse IDE
- Git & GitHub

---

## 🧪 How to Run

1. Clone this repository or download ZIP
2. Import into Eclipse IDE
3. Configure Apache Tomcat in Eclipse
4. Create MySQL DB: `service_system`
5. Create tables
```sql
-- Create Database
CREATE DATABASE serviceProvider;
USE serviceProvider;

SELECT * FROM users;
SELECT * FROM workers;
SELECT * FROM services;
SELECT * FROM worker_service_info;
SELECT * FROM admins;
SELECT * FROM questions;
SELECT * FROM deleted_accounts;
SELECT * FROM login_logs;

-- USER Table
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(100),
    gender ENUM('Male', 'Female', 'Other'),
    address TEXT,
    contact VARCHAR(20),
    photo LONGBLOB,
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE TABLE workers (
    worker_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(100),
    gender ENUM('Male', 'Female', 'Other'),
    address TEXT,
    contact VARCHAR(20),
    photo LONGBLOB,
    service_id INT,
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (service_id) REFERENCES services(service_id)
);

CREATE TABLE services (
    service_id INT AUTO_INCREMENT PRIMARY KEY,
    service_name VARCHAR(50) UNIQUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO services (service_name) VALUES ('Maid'), ('Cook'), ('Driver'), ('Home Tutor');

-- Start -----
CREATE TABLE worker_service_info (
    info_id INT AUTO_INCREMENT PRIMARY KEY,
    worker_id INT,
    veg_pref ENUM('Veg', 'Non-Veg') DEFAULT NULL,      -- For Cook
    tutor_subject VARCHAR(100) DEFAULT NULL,           -- For Tutor
    driver_type ENUM('Car', 'Bike', 'Other') DEFAULT NULL, -- For Driver
    maid_type VARCHAR(100) DEFAULT NULL,               -- For Maid
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (worker_id) REFERENCES workers(worker_id)
);

CREATE TABLE admins (
    admin_id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    password VARCHAR(100),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO admins (email, password) VALUES ('service@gmail.com', 'service123');

CREATE TABLE questions (
    question_id INT AUTO_INCREMENT PRIMARY KEY,
    asked_by ENUM('User', 'Worker'),
    user_id INT DEFAULT NULL,
    worker_id INT DEFAULT NULL,
    question_text TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (worker_id) REFERENCES workers(worker_id)
);

CREATE TABLE deleted_accounts (
    delete_id INT AUTO_INCREMENT PRIMARY KEY,
    deleted_by ENUM('User', 'Worker'),
    ref_id INT,
    reason TEXT,
    deleted_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE login_logs (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    login_by ENUM('User', 'Worker'),
    ref_id INT,
    login_time DATETIME DEFAULT CURRENT_TIMESTAMP
);


-- INSERT DUMMY DATA --
INSERT INTO services (service_name)
VALUES 
    ('Cook'), 
    ('Maid'), 
    ('Driver'), 
    ('Home Tutor');

INSERT INTO users (name, email, password, gender, address, contact)
VALUES 
    ('Riya Sharma', 'riya@gmail.com', 'riya123', 'Female', 'Delhi', '9999988888'),
    ('Aman Verma', 'aman@gmail.com', 'aman123', 'Male', 'Mumbai', '8888877777');

INSERT INTO workers (name, email, password, gender, address, contact, service_id)
VALUES 
    ('Pooja Kumari', 'pooja@cook.com', 'cook123', 'Female', 'Lucknow', '9999911111', 1),  -- Cook
    ('Sunita Devi', 'sunita@maid.com', 'maid123', 'Female', 'Patna', '9999922222', 2),   -- Maid
    ('Rakesh Singh', 'rakesh@driver.com', 'driver123', 'Male', 'Ranchi', '9999933333', 3), -- Driver
    ('Rajeev Joshi', 'rajeev@tutor.com', 'tutor123', 'Male', 'Jaipur', '9999944444', 4); -- Tutor

INSERT INTO worker_service_info (worker_id, veg_pref) VALUES (1, 'Veg');  -- Cook

INSERT INTO worker_service_info (worker_id, maid_type) VALUES (2, 'Kitchen + Bathroom Cleaning');  -- Maid

INSERT INTO worker_service_info (worker_id, driver_type) VALUES (3, 'Car');  -- Driver

INSERT INTO worker_service_info (worker_id, tutor_subject) VALUES (4, 'Maths, Science');  -- Tutor

INSERT INTO questions (asked_by, user_id, question_text)
VALUES ('User', 1, 'How to change address after signup?');

INSERT INTO questions (asked_by, worker_id, question_text)
VALUES ('Worker', 1, 'Can I work in two cities?');

INSERT INTO deleted_accounts (deleted_by, ref_id, reason)
VALUES ('User', 2, 'User requested to delete');

INSERT INTO deleted_accounts (deleted_by, ref_id, reason)
VALUES ('Worker', 3, 'Wrong registration');

INSERT INTO login_logs (login_by, ref_id)
VALUES 
    ('User', 1),
    ('Worker', 1),
    ('Worker', 2),
    ('Admin', 1);
    
-- INSERT END DUMMY DATA --
