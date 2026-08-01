# 🏢 HRMS — Enterprise Resource & Leave Management System

A full-stack enterprise application for managing employees, departments, and leave requests.

---

## 🛠 Tech Stack

**Backend:** Spring Boot 3 · Java 17 · MySQL · JWT · Spring Security · Spring Mail · Thymeleaf  
**Frontend:** React 18 · Vite · Tailwind CSS · Axios · React Router v6

---

## 📁 Project Structure

```
hrms/
├── backend/                   ← Spring Boot Maven project
│   ├── pom.xml
│   └── src/main/java/com/hrms/
│       ├── HrmsApplication.java
│       ├── config/
│       │   ├── SecurityConfig.java
│       │   ├── JwtFilter.java
│       │   ├── CustomUserDetailsService.java
│       │   └── DataInitializer.java     ← Seeds roles + admin user
│       ├── controller/
│       │   ├── AuthController.java
│       │   ├── UserController.java
│       │   ├── LeaveController.java
│       │   ├── DepartmentController.java
│       │   └── NotificationController.java
│       ├── service/
│       │   ├── AuthService.java
│       │   ├── UserService.java
│       │   ├── LeaveService.java
│       │   ├── DepartmentService.java
│       │   └── NotificationService.java
│       ├── repository/
│       ├── entity/
│       │   ├── User.java
│       │   ├── Role.java
│       │   ├── Department.java
│       │   ├── LeaveRequest.java
│       │   ├── Notification.java
│       │   ├── EmailVerificationToken.java
│       │   └── PasswordResetToken.java
│       ├── dto/{request,response}/
│       ├── exception/
│       │   └── GlobalExceptionHandler.java
│       ├── email/
│       │   └── EmailService.java
│       └── util/
│           └── JwtUtil.java
│
└── frontend/                  ← React + Vite
    ├── src/
    │   ├── pages/
    │   │   ├── Login.jsx
    │   │   ├── Register.jsx
    │   │   ├── Dashboard.jsx
    │   │   ├── ApplyLeave.jsx
    │   │   ├── MyLeaves.jsx
    │   │   ├── ManageLeaves.jsx     ← Manager
    │   │   ├── AdminUsers.jsx       ← Admin
    │   │   ├── AdminDepartments.jsx ← Admin
    │   │   └── Notifications.jsx
    │   ├── components/
    │   │   └── Layout.jsx
    │   ├── context/
    │   │   └── AuthContext.jsx
    │   └── services/
    │       └── api.js
    └── ...
```

---

## ⚙️ Setup & Run

### 1. Database
```sql
CREATE DATABASE hrms_db;
```

### 2. Backend Configuration
Edit `backend/src/main/resources/application.yml`:
```yaml
spring:
  datasource:
    password: your_mysql_password
  mail:
    username: your_gmail@gmail.com
    password: your_gmail_app_password   # Not your Gmail password!
```

> **Gmail App Password:** Go to Google Account → Security → 2-Step Verification → App Passwords

### 3. Run Backend
```bash
cd backend
mvn spring-boot:run
```
Backend starts at **http://localhost:8080**

### 4. Run Frontend
```bash
cd frontend
npm install
npm run dev
```
Frontend starts at **http://localhost:5173**

---

## 🔐 Default Credentials

| Email | Password | Role |
|-------|----------|------|
| admin@hrms.com | Admin@123 | ADMIN |

---

## 🌐 API Endpoints

### Auth
| Method | URL | Access |
|--------|-----|--------|
| POST | /api/auth/register | Public |
| POST | /api/auth/login | Public |
| GET | /api/auth/verify-email?token= | Public |
| POST | /api/auth/forgot-password | Public |
| POST | /api/auth/reset-password | Public |

### Users
| Method | URL | Role |
|--------|-----|------|
| GET | /api/users/me | All |
| GET | /api/users?search=&page=&size= | ADMIN |
| PUT | /api/users/{id} | ADMIN |
| PATCH | /api/users/{id}/disable | ADMIN |
| PATCH | /api/users/{id}/enable | ADMIN |

### Leave Requests
| Method | URL | Role |
|--------|-----|------|
| POST | /api/leaves/apply | All |
| GET | /api/leaves/my | All |
| PATCH | /api/leaves/{id}/cancel | Owner |
| GET | /api/leaves/department | MANAGER/ADMIN |
| PATCH | /api/leaves/{id}/review | MANAGER/ADMIN |
| GET | /api/leaves | ADMIN |

### Departments
| Method | URL | Role |
|--------|-----|------|
| GET | /api/departments | All |
| POST | /api/departments | ADMIN |
| PUT | /api/departments/{id} | ADMIN |
| DELETE | /api/departments/{id} | ADMIN |

### Notifications
| Method | URL |
|--------|-----|
| GET | /api/notifications |
| GET | /api/notifications/unread-count |
| PATCH | /api/notifications/mark-all-read |

---

## 🎭 User Roles

| Role | Permissions |
|------|-------------|
| **EMPLOYEE** | Apply leave, view own requests, cancel pending |
| **MANAGER** | All employee + approve/reject department leaves |
| **ADMIN** | Full access: users, departments, all leaves |

---

## 📧 Email Features

- ✅ Email verification on registration
- 🔐 Password reset with 15-min expiry token
- 📋 Manager notified when employee applies leave
- ✅ Employee notified when leave is approved/rejected
- 👤 Admin notified when new employee registers

---

## 💡 Viva-Ready Extension Points

| Examiner Question | How to Extend |
|------------------|---------------|
| "Add leave balance" | Add `leaveBalance` column to User |
| "Add half-day leave" | Add `HALF_DAY` to LeaveType enum |
| "Add leave type filter" | Already supported via `leaveType` param |
| "Add audit log" | Create AuditLog entity + AOP |
| "Add Excel export" | Add Apache POI dependency |
| "Add charts" | Add recharts to frontend |
