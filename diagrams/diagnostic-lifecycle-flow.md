# 🔄 Diagnostic & Patient Lifecycle Flow

This sequence diagram illustrates the complete end-to-end lifecycle of a diagnostic test — from test discovery, appointment booking, and online payment to sample testing, digital report upload, and patient report download.

```mermaid
sequenceDiagram
    autonumber
    actor Patient as 👤 Patient
    participant Web as 🌐 Web Portal (Blade UI)
    participant Server as ⚙️ Laravel Backend
    participant Razorpay as 💳 Razorpay API
    participant Admin as 🔬 Lab Admin / Pathologist
    participant DB as 🗄️ MySQL Database
    participant Disk as 📁 File Storage

    Note over Patient, Web: 1. Test Discovery & Selection
    Patient->>Web: Browse A-Z Test Directory (/test_menu)
    Web->>Server: Filter tests by alphabet (/test_menu_search/{c})
    Server->>DB: Query tests matching prefix
    DB-->>Server: Return test list with pricing & sample rules
    Server-->>Web: Render test cards with pricing
    Patient->>Web: Select Test & Click "Book Appointment"

    Note over Patient, Razorpay: 2. Booking & Online Payment
    Patient->>Web: Fill Appointment Form (Date, Doctor, Dept)
    Web->>Server: POST /forms/appointment
    Server->>DB: Insert into `appointments` table
    Web->>Patient: Redirect to Payment (/razorpay-payment)
    Patient->>Razorpay: Submit Card / UPI / NetBanking Details
    Razorpay-->>Server: Callback with razorpay_payment_id
    Server->>Razorpay: Fetch & Capture Payment via API
    Razorpay-->>Server: Payment Success Confirmation
    Server->>DB: Record transaction status
    Server-->>Web: Flash "Payment Successful & Appointment Confirmed"

    Note over Admin, Disk: 3. Sample Processing & Report Upload
    Admin->>Web: Login to Admin CMS (/admin/auth)
    Admin->>Web: Navigate to Patient Directory (/admin/users)
    Admin->>Web: Select Patient & Upload Diagnostic PDF/Image
    Web->>Server: POST /admin/report_upload (file + user_id)
    Server->>Disk: Store report in storage/app/public/report/
    Server->>DB: Insert file record into `reports` table (mapped to user_id)
    Server-->>Web: Flash "Report Uploaded Successfully"

    Note over Patient, Web: 4. Report Access & Download
    Patient->>Web: Login to Patient Portal (/login)
    Patient->>Web: Open Dashboard (/user_page)
    Server->>DB: Fetch user appointments & reports where user_id = ID
    DB-->>Server: Return patient reports & schedules
    Server-->>Web: Render Dashboard with Downloadable Report Links
    Patient->>Disk: Click & Download Medical Report PDF / Image
```
