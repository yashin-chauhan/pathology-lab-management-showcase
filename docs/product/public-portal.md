# 🌐 Public Web Portal Documentation

The public-facing portal is optimized for user accessibility, mobile responsiveness, and patient self-service.

---

## 📄 Key Pages & User Journeys

### 1. Landing Homepage (`/`)
- **Hero Slider**: Highlighting top diagnostic facilities, laboratory equipment, and certified pathologists.
- **Why Us Section**: Highlighting accuracy, turnaround time, digital delivery, and accredited lab standards.
- **Department & Doctor Showcase**: Browse available specialists (Cardiology, Pathology, Neurology, Pediatrics, etc.).
- **Interactive Appointment Booking Form**: Directly embedded on the homepage for fast booking without page reloads.

### 2. A-Z Test Directory (`/test_menu`)
- Interactive 26-button alphabetical strip (A–Z).
- Real-time querying of the database (`/test_menu_search/{c}`) based on test names starting with the selected letter.
- Displays test pricing, sample criteria, and "Read More" link to dedicated test specifications.

### 3. Patient Authentication (`/login` & `/signup`)
- **Sign Up**: Collects full patient profile including physical address, postal PIN code, mobile number, and encrypted password.
- **Sign In**: Validates credentials and redirects authenticated patients to their private dashboard (`/user_page`).

### 4. Patient Dashboard (`/user_page`)
- Protected by `user_auth` middleware.
- View and manage medical profile information.
- View booked appointment schedules and real-time status.
- Download PDF/image medical diagnostic reports uploaded by the lab.
