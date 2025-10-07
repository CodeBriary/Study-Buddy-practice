# 📘 Study Buddy — Full Stack App

**Study Buddy** is a full-stack mobile app designed to help students manage their study life — assignments, sessions, and an AI-powered study tutor.  
Built with **Flutter (frontend)** and **Node.js + MySQL (backend)**.

---

## 🧠 Tech Stack

### 🖥️ Backend
- **Node.js + Express**
- **MySQL** database
- **JWT Authentication**
- **OpenAI API** for AI tutor
- **bcrypt** for password hashing

### 📱 Mobile App
- **Flutter** (Dart)
- **REST API integration**
- **SharedPreferences** for token persistence
- **Material Design UI**

---

## 🗂️ Project Structure

```
study_buddy/
├── backend/
│   ├── src/
│   │   ├── server.js
│   │   ├── config/db.js
│   │   ├── controllers/
│   │   ├── routes/
│   │   └── middleware/
│   ├── .env
│   └── package.json
│
└── mobile/
    ├── lib/
    │   ├── main.dart
    │   ├── routes.dart
    │   ├── services/api_service.dart
    │   └── screens/
    ├── android/
    ├── ios/
    └── pubspec.yaml
```

---

# 🚀 Backend Setup (Node.js + MySQL)

### 1️⃣ Prerequisites
- Node.js (v18 or newer)
- MySQL 8+
- npm (comes with Node)
- Postman (optional for testing)

### 2️⃣ Setup the database

```sql
CREATE DATABASE study_buddy;
USE study_buddy;

CREATE TABLE users (
  user_id INT AUTO_INCREMENT PRIMARY KEY,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL
);

CREATE TABLE chat_history (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT,
  question TEXT,
  answer TEXT,
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(user_id)
);

CREATE TABLE assignments (
  assignment_id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT,
  title VARCHAR(255),
  description TEXT,
  due_date DATETIME,
  status ENUM('pending','completed') DEFAULT 'pending',
  FOREIGN KEY (user_id) REFERENCES users(user_id)
);

CREATE TABLE study_sessions (
  session_id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT,
  topic VARCHAR(255),
  date DATETIME,
  duration INT,
  notes TEXT,
  FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

### 3️⃣ Configure environment variables

Create a `.env` file inside `/backend`:

```
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASS=your_mysql_password
DB_NAME=study_buddy
JWT_SECRET=your_jwt_secret
OPENAI_API_KEY=sk-your-openai-key
```

### 4️⃣ Install dependencies
```bash
cd backend
npm install
```

### 5️⃣ Run the backend
```bash
npm run dev
```

Expected output:
```
✅ MySQL Connected
✅ Server running on http://localhost:3000
```

---

# 📱 Flutter App Setup

### 1️⃣ Prerequisites
- Flutter SDK 3.22+
- Android Studio or VS Code
- Working Android emulator or real device

### 2️⃣ Connect to the backend
In `mobile/lib/services/api_service.dart`:

For Android emulator:
```dart
final String baseUrl = "http://10.0.2.2:3000/api";
```
For real device (replace IP):
```dart
final String baseUrl = "http://YOUR_COMPUTER_IP:3000/api";
```

### 3️⃣ Install dependencies
```bash
cd mobile
flutter pub get
```

### 4️⃣ Run the app
Make sure backend is running, then:
```bash
flutter run
```

Expected flow:
- Login / Register → Dashboard → AI Tutor & Assignments

---

# 🧠 Quick Test (Postman)

**Register:**
```
POST http://localhost:3000/api/auth/register
```
Body (JSON):
```json
{ "first_name": "John", "last_name": "Doe", "email": "john@example.com", "password": "test1234" }
```

**Login:**
```
POST http://localhost:3000/api/auth/login
```

**Ask AI Tutor:**
```
POST http://localhost:3000/api/ai/ask
Headers: Authorization: Bearer <token>
Body: { "question": "What is photosynthesis?" }
```

---

# 🧩 Environment Setup Summary

| Component | Version | Notes |
|------------|----------|-------|
| Node.js | ≥ 18 | Backend |
| npm | ≥ 9 | Package manager |
| MySQL | ≥ 8 | Database |
| Flutter | ≥ 3.22 | Mobile app |
| Android NDK | 27.0.12077973 | Plugin compatibility |

---

# 🧪 Common Issues

| Issue | Fix |
|-------|-----|
| ECONNREFUSED | Backend not running / wrong IP |
| Invalid credentials | Register a new user |
| compileSdkVersion missing | `flutter clean` then rebuild |
| NDK mismatch | Add `ndkVersion "27.0.12077973"` |
| Emulator offline | Cold boot from Device Manager |

---

# 👨‍💻 Team Workflow

```bash
# 1. Pull latest code
git pull origin main

# 2. Setup backend
cd backend && npm install && npm run dev

# 3. Setup Flutter app
cd mobile && flutter pub get && flutter run

# 4. Commit changes
git add .
git commit -m "Updated AI tutor feature"
git push origin main
```

---

# 🎯 Sprint 3 Goal

> A production-ready Study Buddy app where users can register, log in, manage assignments, track study sessions, and chat with an AI tutor — all synced to a cloud-hosted backend.
