# Restaurant Food Menu Management System (MERN Stack)

A complete, production-grade Restaurant Food Menu Management System developed for a MERN Stack practical examination. This system enables restaurant managers and chefs to curate menu categories, manage culinary food items, upload high-resolution food imagery, and maintain accurate pricing in Pakistani Rupees (PKR).

---

## 1. Project Overview

The Restaurant Food Menu Management System is built with a decoupled architecture featuring:
- **Backend**: RESTful API powered by Node.js, Express, MongoDB, Mongoose, and Multer for multipart/form-data image handling.
- **Frontend**: Responsive Single Page Application (SPA) built with React, Vite, Tailwind CSS, Axios, and Lucide icons following refined typographic and visual standards.
- **Image Pipeline**: Secure upload handling with file type verification, 5 MB limits, unique timestamp naming, and safe cleanup routines on deletion or update.

---

## 2. Technology Stack

### Backend
- **Node.js** (v20+ / v22+)
- **Express.js** 4.x (Routing, middleware, static asset delivery)
- **MongoDB** & **Mongoose** (Document database, schema validation, ObjectId references)
- **Multer** (Disk storage, MIME validation, 5 MB payload limit)
- **CORS** & **dotenv** (Cross-origin security & environment isolation)

### Frontend
- **React.js 19** & **Vite**
- **Tailwind CSS v4**
- **Axios** (Centralized API client with interceptors)
- **Lucide React** (Accessible UI icons)
- **Plus Jakarta Sans** & **Fraunces Serif** typography

---

## 3. Project Folder Structure

```text
├── backend/
│   ├── config/
│   │   └── db.js                 # Mongoose connection logic with error handling
│   ├── middleware/
│   │   └── upload.js             # Reusable Multer upload and removeFile utilities
│   ├── models/
│   │   ├── Category.js           # Mongoose model for menu categories
│   │   └── Food.js               # Mongoose model for food items referencing Category
│   ├── routes/
│   │   ├── categoryRoutes.js     # Full CRUD routes for categories (/api/categories)
│   │   └── foodRoutes.js         # Full CRUD routes for food items (/api/foods)
│   ├── uploads/
│   │   └── .gitkeep              # Directory for stored uploaded media files
│   ├── .env.example              # Sample environment configuration for backend
│   ├── .gitignore                # Git ignore rules for backend
│   ├── package.json              # Standalone backend package definition
│   └── server.js                 # Express server entry point on port 5000
│
├── src/
│   ├── assets/images/            # High-fidelity culinary reference assets
│   ├── components/
│   │   ├── Navbar.tsx            # Top navigation bar (3-Zone Contract)
│   │   ├── StatusBanner.tsx      # Database connection & runtime diagnostic banner
│   │   ├── CategoryModal.tsx     # Add & edit category form with image preview
│   │   ├── FoodModal.tsx         # Add & edit food form with category dropdown
│   │   └── DeleteConfirmModal.tsx# Accessible delete modal with 409 conflict guard
│   ├── pages/
│   │   ├── DashboardPage.tsx     # Operational metrics, recent dishes & category allocations
│   │   ├── CategoriesPage.tsx    # Category catalog with search & image cards
│   │   └── FoodsPage.tsx         # Food catalog with PKR pricing, filter & sort
│   ├── services/
│   │   ├── api.ts / api.js       # Axios instance with getImageUrl utility
│   │   ├── categoryService.ts    # Category CRUD methods
│   │   └── foodService.ts        # Food CRUD methods
│   ├── App.tsx                   # Main React dashboard controller
│   ├── index.css                 # Global styling & Tailwind directives
│   └── main.tsx                  # React DOM entry point
│
├── frontend/src/services/        # Examination path compatibility aliases
├── uploads/                      # Root uploads directory for dev server
├── server.ts                     # Root full-stack dev server mounting Vite & Express
├── index.html                    # HTML shell with synced meta tags & fonts
├── metadata.json                 # AI Studio applet metadata
├── package.json                  # Root dependencies and run scripts
├── tsconfig.json                 # TypeScript compiler configuration
└── vite.config.ts                # Vite build and asset bundler configuration
```

---

## 4. Prerequisites

Before running the application locally, ensure you have installed:
1. **Node.js** (v18.0.0 or higher, v20+ recommended) - [Download Node.js](https://nodejs.org/)
2. **npm** (comes with Node.js)
3. **MongoDB Community Server** - [Download MongoDB](https://www.mongodb.com/try/download/community) or **MongoDB Compass**

---

## 5. Local MongoDB Setup

1. Start your local MongoDB server:
   ```bash
   # On macOS / Linux
   mongod --dbpath /path/to/data/db

   # Or via Homebrew (macOS)
   brew services start mongodb-community

   # On Windows (as a Windows Service)
   net start MongoDB
   ```
2. Verify MongoDB is accessible at:
   `mongodb://127.0.0.1:27017/restaurant_menu`

*(Note: MongoDB automatically creates the `restaurant_menu` database and collections upon inserting the first record.)*

---

## 6. Environment Configuration

### Backend Environment (`backend/.env`)
Create a file named `.env` inside the `backend/` directory based on `backend/.env.example`:
```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/restaurant_menu
CLIENT_URL=http://localhost:3000
```

### Root / Frontend Environment (`.env`)
Create a file named `.env` in the root project folder based on `.env.example`:
```env
PORT=3000
MONGO_URI=mongodb://127.0.0.1:27017/restaurant_menu
VITE_API_BASE_URL=http://localhost:5000/api
```

---

## 7. Installation & Startup Commands

### Option A: Unified Full-Stack Mode (Port 3000)
Runs Express API and React Vite frontend concurrently through `server.ts`:
```bash
# 1. Install dependencies
npm install

# 2. Start full-stack dev server
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

---

### Option B: Dedicated Standalone Architecture (Exam Standard)
Run the backend and frontend in two separate terminal windows:

#### Terminal 1 — Backend (Port 5000):
```bash
# Navigate to backend directory
cd backend

# Install backend dependencies
npm install

# Start backend server
npm run dev
# Or: npm start
```
*Expected Output:*
```text
[MongoDB] Successfully connected: 127.0.0.1/restaurant_menu
=================================================
 Restaurant Food Menu Backend Server Running!
 Port: 5000
 Status: http://localhost:5000/api/status
 Categories API: http://localhost:5000/api/categories
 Foods API: http://localhost:5000/api/foods
=================================================
```

#### Terminal 2 — Frontend (Port 3000):
```bash
# In the root project directory:
npm run dev
```

---

## 8. API Endpoint Documentation

Base URL: `http://localhost:5000/api`

### Health & Diagnostic
| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/status` | Returns API status, MongoDB connection state, and uploads directory path |

### Category Endpoints (`/api/categories`)
| Method | Endpoint | Description | Content-Type | Status Codes |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/api/categories` | Retrieve all categories (newest first) | `application/json` | 200, 500 |
| `GET` | `/api/categories/:id` | Retrieve single category by MongoDB ID | `application/json` | 200, 400, 404 |
| `POST` | `/api/categories` | Create new category with image upload | `multipart/form-data` | 201, 400, 500 |
| `PUT` | `/api/categories/:id` | Update category (optionally replaces image) | `multipart/form-data` | 200, 400, 404 |
| `DELETE` | `/api/categories/:id` | Delete category (checks linked foods) | `application/json` | 200, 400, 404, 409 |

### Food Endpoints (`/api/foods`)
| Method | Endpoint | Description | Content-Type | Status Codes |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/api/foods` | Retrieve all foods (populates category details) | `application/json` | 200, 500 |
| `GET` | `/api/foods?category=:id&search=:query` | Filter foods by category and search term | `application/json` | 200, 500 |
| `GET` | `/api/foods/:id` | Retrieve single food item with category | `application/json` | 200, 400, 404 |
| `POST` | `/api/foods` | Create new food item with image upload | `multipart/form-data` | 201, 400, 500 |
| `PUT` | `/api/foods/:id` | Update food item (optionally replaces image) | `multipart/form-data` | 200, 400, 404 |
| `DELETE` | `/api/foods/:id` | Delete food item and remove image | `application/json` | 200, 400, 404 |

---

## 9. Multipart Form-Data Field Specifications

### Category Creation / Update (`POST` / `PUT /api/categories`)
- `name` (String, required): 2 to 100 characters, trimmed.
- `description` (String, required): 5 to 1000 characters, trimmed.
- `image` (Binary File): Image file under 5 MB. Required for `POST`, optional for `PUT`.

### Food Creation / Update (`POST` / `PUT /api/foods`)
- `name` (String, required): 2 to 120 characters, trimmed.
- `price` (Number, required): Positive finite number greater than 0.
- `description` (String, required): 5 to 1500 characters, trimmed.
- `category` (String, required): Valid MongoDB ObjectId corresponding to an existing Category.
- `image` (Binary File): Image file under 5 MB. Required for `POST`, optional for `PUT`.

---

## 10. Image Upload & File Lifecycle

1. **Storage Location**: Uploads are placed into `backend/uploads/` (or `uploads/`).
2. **File Naming**: Formatted as `img-<timestamp>-<random-suffix>.<extension>` (e.g. `img-1742081234567-891234567.jpg`).
3. **Format Support**: JPG, JPEG, PNG, WEBP, and GIF.
4. **Size Cap**: 5 MB (`5 * 1024 * 1024` bytes). Oversized files trigger a 400 status error.
5. **Database Storage**: Only relative paths (e.g., `uploads/img-12345.jpg`) are persisted in MongoDB.
6. **Safe Cleanup**:
   - When updating with a new image, the old image is deleted **only after** the database update succeeds.
   - When deleting a category or food dish, its associated image is unlinked from disk.
   - File deletion is constrained to the `uploads/` directory to prevent directory traversal exploits.

---

## 11. Common Issues & Solutions

### 1. `MongooseServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017`
- **Cause**: Local MongoDB daemon is not running.
- **Solution**: Open your terminal and run `mongod` or start MongoDB via your OS service manager. The web dashboard will automatically indicate when MongoDB connects.

### 2. `409 Conflict: Cannot delete category. N food item(s) are currently assigned`
- **Cause**: Referential integrity protection prevents leaving orphaned food items.
- **Solution**: Delete or reassign the food items assigned to that category before deleting the category.

### 3. `File size exceeds maximum allowed limit of 5 MB`
- **Cause**: Uploaded image exceeds 5 MB.
- **Solution**: Compress the image or use an image file under 5 MB.

---

## 12. GitHub Submission Steps

To push this repository to GitHub for evaluation:

```bash
# 1. Initialize local git repository (if not already initialized)
git init

# 2. Stage all project files
git add .

# 3. Create initial commit
git commit -m "Initial Restaurant Menu Management System"

# 4. Set default branch to main
git branch -M main

# 5. Add remote origin (replace YOUR_GITHUB_REPOSITORY_URL with your actual repo)
git remote add origin YOUR_GITHUB_REPOSITORY_URL

# 6. Push to GitHub
git push -u origin main
```
#   m e r n p r o j e c t  
 