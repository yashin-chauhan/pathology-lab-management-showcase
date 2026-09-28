# 🗺️ System Architecture Diagram

This diagram visualizes the multi-tier architecture of the **Medilab Pathology Laboratory & Healthcare Platform**, illustrating user personas, entry routing, dual-guard authentication, core domain services, third-party payment gateways, and data persistence.

```mermaid
flowchart TD
    subgraph Users["User Personas & Actors"]
        PATIENT["👤 Registered Patient / Visitor"]
        PATHOLOGIST["🔬 Pathologist / Lab Technician"]
        ADMIN["👑 Lab Administrator"]
    end

    subgraph EntryPoint["Entry & Gateway Layer"]
        HTTP["HTTP / HTTPS Request"]
        REWRITE["Web Server URL Rewrite (.htaccess / Nginx)"]
        LARAVEL_PUBLIC["Laravel Public Gateway (index.php)"]
    end

    subgraph RouteLayer["Routing & Middleware Tier"]
        ROUTER["RouteServiceProvider (routes/web.php)"]
        PUB_GROUP["🌐 Public Web Routes (/, /test_menu, /login)"]
        USR_GROUP["👤 Patient Auth Group (middleware: user_auth)"]
        ADM_GROUP["🔐 Admin Auth Group (middleware: admin_auth)"]
    end

    subgraph ServiceLayer["Core Application & Domain Services"]
        AUTH_SVC["Dual Session Auth Guard (Admin & Patient)"]
        TEST_SVC["A-Z Diagnostic Test Directory Engine"]
        APPT_SVC["Appointment Scheduling Engine"]
        PAY_SVC["Razorpay Payment Integration Service"]
        REPORT_SVC["Diagnostic Report Upload & Delivery Engine"]
        TEAM_SVC["Medical Specialists & Staff CMS"]
        MSG_SVC["Inquiries & Feedback Service"]
    end

    subgraph ExternalServices["External APIs & Integrations"]
        RAZORPAY["💳 Razorpay Payment Gateway API"]
    end

    subgraph PersistenceLayer["Data & File Persistence Tier"]
        DB[("MySQL Database (Users, Admins, Appts, Reports, Services)")]
        STORAGE["Local Disk Storage (public/storage/report, services)"]
    end

    PATIENT --> HTTP
    PATHOLOGIST --> HTTP
    ADMIN --> HTTP
    HTTP --> REWRITE --> LARAVEL_PUBLIC --> ROUTER

    ROUTER --> PUB_GROUP --> TEST_SVC
    ROUTER --> PUB_GROUP --> APPT_SVC
    ROUTER --> PUB_GROUP --> PAY_SVC
    ROUTER --> USR_GROUP --> AUTH_SVC
    ROUTER --> ADM_GROUP --> AUTH_SVC

    PAY_SVC <-->|Signature & Capture| RAZORPAY
    AUTH_SVC --> REPORT_SVC
    AUTH_SVC --> TEAM_SVC
    AUTH_SVC --> MSG_SVC

    TEST_SVC --> DB
    APPT_SVC --> DB
    PAY_SVC --> DB
    REPORT_SVC --> DB
    REPORT_SVC --> STORAGE
    TEAM_SVC --> DB
    TEAM_SVC --> STORAGE
    MSG_SVC --> DB
```
