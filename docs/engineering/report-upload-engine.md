# 📁 Engineering: Report Upload & File Storage Engine

The Report Upload Engine handles multipart form uploads of medical files, sanitization, disk storage, and database association.

---

## 🏗️ Storage Pipeline

1. **Upload Form**: Admin selects patient and attaches file (`.pdf`, `.jpg`, `.png`).
2. **Sanitization & Renaming**:
   - File extension extracted via `$request->file('report')->extension()`.
   - Unique Unix timestamp filename generated (`time() . '.' . $ext`) to prevent filename collisions and overwrite risks.
3. **Disk Storage**:
   - File saved to `storage/app/public/report/` using Laravel's filesystem abstraction (`$report->storeAs('public/report', $report_name)`).
4. **Symlink Distribution**:
   - Through `php artisan storage:link`, files in `storage/app/public` are exposed via `public/storage/report/` for fast HTTP static streaming.
5. **Database Mapping**:
   - Insertion into `reports` table linking `name` and `user_id`.
