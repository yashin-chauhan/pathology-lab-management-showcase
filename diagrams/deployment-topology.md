# 🌐 Deployment & Infrastructure Topology

This document details the production/staging deployment topology and server configuration for the **Medilab** Pathology Laboratory platform.

```mermaid
flowchart TD
    subgraph Clients["External Users"]
        PATIENT_DEVICE["📱 Mobile / Desktop Patients"]
        LAB_DESK["💻 Laboratory Reception & Admin Desks"]
    end

    subgraph Security["Edge & Reverse Proxy Tier"]
        SSL["🔒 SSL / TLS Termination (Let's Encrypt)"]
        NGINX["⚡ Nginx / Apache Web Server"]
    end

    subgraph AppServer["Application Tier (PHP 8.x + Laravel 8 Engine)"]
        FPM["PHP-FPM Worker Pool"]
        ARTISAN["Artisan CLI & Route Handler"]
        SESSION_MGR["File / Redis Session Manager"]
    end

    subgraph StorageTier["Persistence & File Storage Tier"]
        MYSQL[("🗄️ MySQL Server (InnoDB Engine)")]
        SYMLINK["📁 Symlinked Storage (storage/app/public)"]
    end

    subgraph ExternalGateway["Third-Party Payment Gateway"]
        RAZORPAY_GATEWAY["💳 Razorpay Payment API (HTTPS Rest)"]
    end

    PATIENT_DEVICE -->|HTTPS 443| SSL
    LAB_DESK -->|HTTPS 443| SSL
    SSL --> NGINX
    NGINX -->|FastCGI| FPM
    FPM --> ARTISAN
    ARTISAN --> SESSION_MGR
    ARTISAN --> MYSQL
    ARTISAN --> SYMLINK
    ARTISAN <-->|REST API| RAZORPAY_GATEWAY
```

---

## 🛠️ Server Environment Specifications

- **Operating System**: Linux (Ubuntu 20.04/22.04 LTS) or Windows Server
- **Web Server**: Nginx or Apache with `mod_rewrite` enabled
- **PHP Version**: PHP 7.4 or 8.0+ with required extensions (`pdo_mysql`, `mbstring`, `openssl`, `tokenizer`, `xml`, `ctype`, `json`, `fileinfo`, `curl`)
- **Database Server**: MySQL 5.7 / 8.0 or MariaDB 10.4+
- **File Symlink**: `public/storage` linked to `storage/app/public`
