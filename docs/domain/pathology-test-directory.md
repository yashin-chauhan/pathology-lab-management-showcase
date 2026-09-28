# 🧪 Domain Design: Pathology Test Directory & Search

The Pathology Test Directory is a core patient-facing domain module that organizes diagnostic medical tests into an intuitive, accessible catalog.

---

## 🏗️ Architectural Mechanics

### 1. A-Z Alphabetical Navigation Strip
- The UI dynamically generates 26 interactive letter buttons (A through Z) using client-side JavaScript.
- Clicking on a letter invokes a route handler `GET /test_menu_search/{c}` where `{c}` represents the chosen letter.

### 2. Query Execution & Filtering
In `HomeController.php`:
```php
public function test_menu_search($c)
{
    $data['service'] = DB::table('services')
        ->where('name', 'LIKE', $c . '%')
        ->get();
    return view('alpha', $data);
}
```

### 3. Test Metadata & Clinical Attributes
Each test record stores:
- **`name`**: Formal clinical test name (e.g. Complete Blood Count - CBC).
- **`price`**: Standard diagnostic laboratory test fee.
- **`short_desc`**: Essential sample type (e.g. Fasting Blood Sample, Urine 24hr, Serum).
- **`long_desc`**: Clinical significance, normal reference ranges, and pre-test dietary guidelines.
- **`img` / `img2`**: Sample collection tube color code and testing instrument diagrams.
