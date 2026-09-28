# 🔐 Administrative & Lab Back-Office Documentation

The Administrative Portal (`/admin`) is a password-protected back-office suite designed for laboratory directors, technicians, and administrative staff.

---

## 🛠️ Back-Office Subsystems

### 1. Admin Authentication (`/admin`)
- Secure login mechanism with bcrypt password verification.
- Guarded by custom Laravel session middleware `admin_auth`.

### 2. Patient Directory & Medical Report Upload (`/admin/users`)
- Master list of all registered patients with their contact details, address, and registration timestamps.
- **Report Upload Interface (`POST /admin/report_upload`)**:
  - Modal/Form to select patient `user_id` and upload medical report file (`.pdf`, `.jpg`, `.png`).
  - Stores file in `storage/app/public/report/` with a unique timestamped filename.
  - Automatically inserts report reference into the `reports` database table.

### 3. Appointment Manager (`/admin/appointments`)
- Centralized queue of all patient appointments.
- Displays patient name, contact number, doctor, department, scheduled date, and patient notes.
- Quick delete action to archive or cancel appointments.

### 4. Diagnostic Test Catalog CMS (`/admin/service`)
- CRUD operations for pathology test services:
  - Add new diagnostic tests (`/admin/service/add`) with title, pricing, color theme, dual image upload, short summary, and long description.
  - Edit existing tests (`/admin/service/manage_service/{id}`).
  - Delete obsolete tests.

### 5. Medical Staff & Doctor CMS (`/admin/team`)
- Add and manage medical staff profiles with name, designation, photo, and social links.
