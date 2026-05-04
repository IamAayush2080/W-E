# 🌿 Evergreen Pacific English Boarding School — Website

## What Was Fixed & Improved

### 🔴 Critical Bug Fixes

1. **Students/Teachers/Notices disappearing on reload**
   - Root cause: All admin panels were using **in-memory SEED data** — not calling the API at all
   - Fix: Every panel (`StudentsPanel`, `TeachersPanel`, `NoticesPanel`, `GalleryPanel`, `FeesPanel`) now calls the real MongoDB API using `fetch()` on load and saves data to MongoDB on every add/edit/delete

2. **Gallery upload not working**
   - Root cause: The upload button was only updating local React state (never called `/api/gallery`)
   - Fix: Now sends a real `FormData` POST to `/api/gallery`, which uploads to Cloudinary (or local disk if no Cloudinary credentials) and saves to MongoDB

3. **"Campus" → "School"**
   - Replaced every occurrence of "campus" with "school" across: Home.jsx, Gallery.jsx, AdminDashboard.jsx, server.js gallery categories

4. **Index/Home page background**
   - Added a rich green gradient with decorative overlays to give depth to the hero section
   - Notices on home now load **live from the database** instead of hardcoded static data

5. **Teacher photos (Faculty page)**
   - Teachers page now loads from `/api/teachers/public` (no auth required)
   - Each teacher card shows their photo if set
   - Admin can set photos two ways:
     - **Upload**: Click Photo button → select file
     - **Manual path**: Place image in `backend/public/uploads/teachers/` → enter path `/uploads/teachers/filename.jpg`

6. **Gallery manual path support**
   - New "Add by Path" button in Admin Gallery panel
   - Place image in `backend/public/uploads/gallery/` → enter path to link it permanently

---

## Setup Instructions

### Backend
```bash
cd backend
npm install
node server.js   # or: npx nodemon server.js
```

**First run — seed the admin account:**
```
POST http://localhost:5000/api/auth/seed-admin
```
Then login at `/login` with:
- Email: `admin@evergreen.edu.np`  
- Password: `Admin@2081`

### Frontend
```bash
cd frontend
npm install
npm run dev
```

---

## Image Paths (Manual Upload)

For teacher photos:
1. Place file in: `backend/public/uploads/teachers/`
2. Admin → Teachers → Photo → Manual Path → enter: `/uploads/teachers/yourfile.jpg`

For gallery images:
1. Place file in: `backend/public/uploads/gallery/`
2. Admin → Gallery → Add by Path → enter: `/uploads/gallery/yourfile.jpg`

---

## File Changes Summary

| File | What Changed |
|------|-------------|
| `backend/server.js` | Added local file storage fallback, teacher photo upload routes, gallery path route, public teachers route |
| `frontend/src/pages/AdminDashboard.jsx` | **Full rewrite** — all panels now use real API (no seed data). Gallery uses real upload. Teacher photo modal added. |
| `frontend/src/pages/Home.jsx` | Notices loaded from DB. Hero redesigned with depth. "Campus" → "School". |
| `frontend/src/pages/Faculty.jsx` | Loads teachers from DB. Photo display. Card grid design. |
| `frontend/src/pages/Gallery.jsx` | Loads photos from DB. Displays real images. "Campus" → "School". |
| `frontend/src/pages/Notices.jsx` | Loads notices from DB. Expandable cards. Fallback to static if DB empty. |
| `frontend/src/styles/globals.css` | Improved card hover bars, fade-in animation, glass utility |
