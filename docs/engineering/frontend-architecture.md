# 🎨 Engineering: Frontend Architecture & UI Engine

The frontend architecture of Medilab is designed for fast rendering, responsive cross-device compatibility, and accessible healthcare UX.

---

## 🏗️ Core Layers & Technologies

- **Template Inheritance**: Laravel Blade with master layouts (`layout.blade.php`, `admin/layout.blade.php`), `@section('container')`, and reusable component partials.
- **CSS Framework**: Bootstrap 5 grid and utility classes for mobile, tablet, and desktop viewports.
- **Iconography**: Font Awesome Free, Boxicons, and Remix Icons for medical symbols, test tubes, syringes, and clinical equipment.
- **Interactive UI Engines**:
  - **GLightbox**: Lightbox modal viewer for medical certificates and facility photo galleries.
  - **Swiper Slider**: Touch-friendly carousel for featured medical departments and client reviews.
  - **Animate On Scroll (AOS)**: Smooth entrance animations for healthcare metrics and service cards.
  - **JavaScript A-Z Filter**: Client-side generated alphabet array enabling dynamic search filtering.
