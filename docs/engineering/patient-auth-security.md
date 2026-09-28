# 🔐 Engineering: Dual Authentication & Security Architecture

Medilab employs a dual-guard session authentication architecture separating administrative operations from patient self-service activities.

---

## 🛡️ Authentication Guards & Middleware

### 1. Admin Guard (`admin_auth` Middleware)
- Protects all `/admin/*` routes.
- Checks `session()->has('ADMIN_LOGIN')` and validates `ADMIN_ID`.
- Redirects unauthenticated attempts directly to `/admin` login page with flash alerts.

### 2. Patient Guard (`user_auth` Middleware)
- Protects patient dashboard `/user_page` and private download endpoints.
- Checks `session()->has('USER_LOGIN')` and validates `USER_ID`.
- Prevents cross-patient report leakage by restricting SQL queries strictly to `session('USER_ID')`.

---

## 🔑 Password Hashing & Encryption

- All user and admin passwords are encrypted using **Bcrypt** via Laravel's `Hash::make($password)`:
  ```php
  $user->password = Hash::make($request->post('password'));
  ```
- Verification uses `Hash::check($inputPassword, $hashedPassword)` to guard against timing attacks and rainbow table lookups.
