# 🚌 CodeAlpha Cloud Bus Pass System

A complete, production-ready **Cloud-Based Bus Pass Management System** built with HTML5, CSS3, and Vanilla JavaScript on top of Firebase (Authentication + Firestore + Hosting). Built for the **CodeAlpha Cloud Computing Internship**.

---

## 📖 Project Overview

The Cloud Bus Pass System replaces traditional paper bus passes with a secure, cloud-hosted digital alternative. Riders register, apply for a pass online, get fares calculated automatically by the system, and receive a digital pass complete with a QR code that can be downloaded as a PDF and re-downloaded at any time — solving the classic "lost pass" problem entirely. Transit administrators get a real-time dashboard to review applications, manage users, and track revenue.

This project satisfies every CodeAlpha task requirement:

| Requirement | How it's satisfied |
|---|---|
| Online ticket/pass booking system | Full pass application form with live fare calculation |
| Cloud-hosted architecture | Firebase Authentication + Firestore + Hosting |
| Ticket loss prevention | Digital pass stored in the cloud, re-downloadable anytime as PDF |
| Correct pricing calculation | Fixed, non-editable fare table enforced in code |
| Scalability | Indexed Firestore queries, pagination, serverless infrastructure |
| Reliability | Real-time listeners, form validation, network error handling |
| Seamless user experience | Glassmorphism UI, toast notifications, smooth animations |

---

## ✨ Features

### Authentication
- Email/password registration and login
- Forgot password (Firebase email reset flow)
- Persistent sessions across browser restarts
- Role-based routing (user vs admin dashboard)

### Bus Pass Application
- Full applicant form: name, age, gender, mobile, email, address, source, destination, pass type
- Automatic, tamper-proof fare calculation (Daily ₹30 / Weekly ₹150 / Monthly ₹500 / Yearly ₹5000)
- Live fare breakdown shown before submission
- Form validation with inline error messages

### Digital Bus Pass
- Professional pass certificate with Pass ID, route, fare, issue/expiry dates, status, and QR code
- View pass in a modal at any time
- Download as PDF using html2canvas + jsPDF
- Re-download unlimited times — solves ticket loss permanently

### Pass Management
- Apply for new pass
- View all active passes
- Renew expired passes in one click
- Cancel a pass
- Full pass history table

### Admin Dashboard
- View all users and all passes
- Approve / reject pending applications in real time
- Search passes by name, Pass ID, mobile, source, or destination
- Search users by name, email, or mobile
- Revenue statistics, active pass count, expired pass count
- Paginated tables for scalability

### Reliability & Scalability
- Real-time Firestore `onSnapshot` listeners keep every screen in sync instantly
- Friendly error handling for network/auth failures
- Dedicated in-app sections explaining why cloud systems beat paper passes and how Firebase scales

### UI / UX
- Glassmorphism SaaS-style interface with floating cards and dashboard widgets
- Animated counters, button ripple effects, fade-in/slide-up transitions
- Toast notifications for every key action (login, registration, pass created, renewed, cancelled, approved, validation errors)
- Fully responsive: mobile, tablet, and desktop with a collapsible sidebar

---

## 🛠️ Technologies Used

**Frontend:** HTML5, CSS3 (custom properties, Glassmorphism), Vanilla JavaScript (ES Modules)
**Cloud Services:** Firebase Authentication, Firebase Firestore, Firebase Hosting
**PDF/QR Generation:** html2canvas, jsPDF, qrcode.js (loaded via CDN, no Firebase Storage used — all data lives in Firestore)

No frameworks (no React/Next.js), no backend server (no Node/Express/Python) — this is a pure client-side app talking directly to Firebase.

---

## 🔥 Firebase Setup

1. Go to the [Firebase Console](https://console.firebase.google.com/) and open (or create) the project `codealpha-buspasssystem`.
2. **Authentication** → Sign-in method → enable **Email/Password**.
3. **Firestore Database** → create a database (production mode) in your preferred region.
4. The web app config is already wired up in `js/firebase-config.js`:

   ```js
   const firebaseConfig = {
     apiKey: "AIzaSyCrIiCxKLm3T85nUndD_i1y2DafGjo6aMI",
     authDomain: "codealpha-buspasssystem.firebaseapp.com",
     projectId: "codealpha-buspasssystem",
     storageBucket: "codealpha-buspasssystem.firebasestorage.app",
     messagingSenderId: "228147055183",
     appId: "1:228147055183:web:f2cd666f4b657f2f5b383a",
     measurementId: "G-XKX6YZFP4N"
   };
   ```

5. Deploy the included `firestore.rules` so users can only access their own records while admins get full visibility:

   ```
   firebase deploy --only firestore:rules
   ```

6. **Creating an admin account:** open `js/firebase-config.js` and check/edit the `ADMIN_EMAILS` array (also mirrored in `firestore.rules`). Any account that registers with one of those email addresses is automatically treated as an admin and routed to `admin.html`.

### Firestore Collections

| Collection | Purpose |
|---|---|
| `users` | One document per registered user (`uid`, name, email, mobile, role, createdAt) |
| `passes` | Every pass application/record (status, fare, route, issue/expiry dates, owner `userId`) |
| `payments` | Payment log tied to each pass purchase/renewal |

---

## 🚀 Deployment Guide

### Prerequisites
- [Node.js](https://nodejs.org/) installed
- A Google account with access to the Firebase project

### Steps

```bash
# 1. Install the Firebase CLI (one-time)
npm install -g firebase-tools

# 2. Log in to Firebase
firebase login

# 3. From the project root, verify the project alias
firebase use codealpha-buspasssystem

# 4. Deploy hosting + Firestore rules/indexes
firebase deploy
```

After deployment, Firebase will print a Hosting URL like:

```
✔  Deploy complete!
Hosting URL: https://codealpha-buspasssystem.web.app
```

### Local testing before deploying

```bash
firebase emulators:start
# or simply open index.html with a local static server, e.g.:
npx serve .
```

> Note: Because this app uses ES Modules (`<script type="module">`), opening `index.html` directly via `file://` will not work in most browsers — always serve it over `http://` (Firebase Hosting, `firebase serve`, `npx serve`, or VS Code's "Live Server" extension all work fine).

---

## 📁 Project Structure

```
CodeAlpha_CloudBusPassSystem/
│
├── index.html              # Landing page
├── login.html               # Login page
├── register.html            # Registration page
├── dashboard.html           # User dashboard (apply, manage, view passes)
├── admin.html                # Admin dashboard (approvals, users, revenue)
│
├── css/
│   └── style.css            # Full glassmorphism design system
│
├── js/
│   ├── firebase-config.js   # Firebase init + exported SDK functions
│   ├── auth.js               # Register / Login / Logout / Forgot password / Route guard
│   ├── app.js                 # Shared utilities: toasts, validation, fare logic, formatting
│   └── dashboard.js          # User + Admin dashboard logic, PDF/QR generation
│
├── assets/
│   ├── images/
│   └── icons/
│
├── firebase.json             # Hosting + Firestore deployment config
├── .firebaserc                # Firebase project alias
├── firestore.rules            # Security rules
├── firestore.indexes.json     # Composite indexes
└── README.md
```

---

## 📸 Screenshots Section

      <img width="1920" height="1080" alt="Screenshot 2026-06-17 113206" src="https://github.com/user-attachments/assets/9ea86db4-8907-4ad8-af57-629924650692" />
      <img width="1836" height="882" alt="Screenshot 2026-06-17 112722" src="https://github.com/user-attachments/assets/ddcf3afc-5714-4683-8ce4-810f7b75b246" />
      <img width="1890" height="901" alt="Screenshot 2026-06-19 105946" src="https://github.com/user-attachments/assets/1453c441-ab92-420f-ba4c-c43fa28e56c0" />
      <img width="1865" height="711" alt="Screenshot 2026-06-19 110045" src="https://github.com/user-attachments/assets/717143e6-97c4-44d3-ab38-d2fe5759403a" />
      <img width="980" height="451" alt="Screenshot 2026-06-19 110201" src="https://github.com/user-attachments/assets/5f4dd06a-d45a-45fa-93f2-2f32e49c148c" />
      <img width="555" height="482" alt="Screenshot 2026-06-19 110217" src="https://github.com/user-attachments/assets/29f04f47-a845-4943-ad94-0ee1c443729b" />

---

## 🔮 Future Improvements

- Move admin role-checking from a hardcoded email allow-list to Firebase custom claims for stronger security
- Add SMS/email notifications on pass approval via Cloud Functions
- Integrate a real payment gateway (Razorpay/Stripe) instead of simulated payment records
- Add multi-language support for wider accessibility
- Add route-based fare variation (distance-based pricing) instead of flat pass-type pricing
- Progressive Web App (PWA) support for offline pass viewing

---

## 📄 License

This project was built for educational purposes as part of the CodeAlpha Cloud Computing Internship.
