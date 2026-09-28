# 🗄️ Database Entity-Relationship Diagram (ERD)

The **Medilab** database is designed for normalized relational storage across patients, administrative staff, diagnostic tests, appointment bookings, digital reports, and customer inquiries.

```mermaid
erDiagram
    USERS {
        bigint id PK "Auto Increment"
        string name "Patient Full Name"
        string email UK "Unique Email Address"
        string mobile "Mobile Phone Number"
        string address "Residential Address"
        string city "City"
        string pin "Postal PIN Code"
        string state "State"
        string password "Bcrypt Hashed Password"
        string remember_token "Nullable Token"
        timestamps created_at "Created & Updated Timestamps"
    }

    ADMINS {
        bigint id PK "Auto Increment"
        string email "Admin Email"
        string password "Bcrypt Hashed Password"
        timestamps created_at "Created & Updated Timestamps"
    }

    APPOINTMENTS {
        bigint id PK "Auto Increment"
        string name "Patient Name"
        string email "Patient Contact Email"
        string phone "Patient Contact Phone"
        string date "Appointment Scheduled Date"
        string department "Clinical Department / Category"
        string doctor "Selected Doctor / Pathologist"
        text message "Clinical Symptoms / Remarks"
        bigint user_id FK "Nullable User Foreign Key"
        timestamps created_at "Booking Timestamp"
    }

    REPORTS {
        bigint id PK "Auto Increment"
        string name "Filename of stored PDF/Image"
        bigint user_id FK "Patient User ID Foreign Key"
        timestamps created_at "Upload Timestamp"
    }

    SERVICES {
        bigint id PK "Auto Increment"
        string name "Test / Service Name"
        string price "Test Fee (INR)"
        string color "UI Category Color Code"
        string img "Primary Sample / Icon Image"
        string img2 "Secondary Diagram / Equipment Image"
        text short_desc "Brief Test Summary"
        longtext long_desc "Full Clinical Specifications & Preparation"
        timestamps created_at "Creation Timestamp"
    }

    TEAMS {
        bigint id PK "Auto Increment"
        string name "Doctor / Specialist Name"
        string role "Medical Specialty / Designation"
        string img "Profile Photo"
        string twitter "Twitter Handle Link"
        string facebook "Facebook Profile Link"
        string instagram "Instagram Handle Link"
        string linkedin "LinkedIn Profile Link"
        timestamps created_at "Creation Timestamp"
    }

    CONTACTS {
        bigint id PK "Auto Increment"
        string name "Sender Name"
        string email "Sender Email"
        string subject "Inquiry Subject"
        text message "Inquiry Message Body"
        timestamps created_at "Submission Timestamp"
    }

    USERS ||--o{ APPOINTMENTS : "books"
    USERS ||--o{ REPORTS : "owns & downloads"
    SERVICES ||--o{ APPOINTMENTS : "booked_for"
    TEAMS ||--o{ APPOINTMENTS : "assigned_to"
```

---

## 📋 Data Dictionary & Relationships

1. **`users` ➔ `appointments` (1:N)**: A registered patient can book multiple lab appointments over time.
2. **`users` ➔ `reports` (1:N)**: An admin uploads one or more diagnostic report files (`reports.name`) associated with a single patient (`reports.user_id`).
3. **`services`**: Stores diagnostic pathology packages and individual blood/tissue tests with dynamic pricing and descriptions.
4. **`admins`**: Independent credential store protected by `admin_auth` session middleware.
