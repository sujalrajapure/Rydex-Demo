# Rydex 🚗

A full-stack ride-sharing platform built with Next.js 16 (App Router). Supports user ride booking, driver (partner) management, admin dashboard, real-time tracking/chat/video KYC, and payments via Razorpay/Stripe. Deployed on Netlify.


## ✨ Features

### User
- Search nearby vehicles & book rides
- Real-time route/live tracking (Leaflet maps)
- Secure checkout & payments (Razorpay/Stripe)
- In-ride chat with driver
- OTP verification (pickup/drop)

### Partner (Driver)
- Onboarding: Vehicle/docs/bank details, video KYC (Zego)
- Active/pending bookings dashboard
- Earnings analytics (Recharts)
- Real-time navigation & status updates (arrived/arriving/start/complete)

### Admin
- Dashboard: Earnings charts, stats
- Vendor/vehicle approval/rejection
- Manage bookings & commissions

### Core
- Real-time: Socket.io (chat/location), Zego video
- Auth: NextAuth (register/OTP)
- Database: MongoDB (Mongoose schemas for Booking/User/Vehicle/etc.)

## 🏗️ Architecture

```
rydex/ (Next.js Frontend + Serverless API)
├── app/
│   ├── api/           # Routes: auth, booking lifecycle, payments, admin/partner ops
│   ├── admin/         # Dashboard pages
│   ├── partner/       # Driver dashboard/onboarding
│   ├── book/ride/     # User flow
│   └── search/        # Vehicle search
├── components/        # UI: Maps (RouteMap/LiveTrackingMap), Hero, Charts
├── lib/              # DB (Mongo/Mongoose), Razorpay/Stripe, Socket, Cloudinary
├── models/           # Schemas: Booking (geoJSON, OTPs, status), User, Vehicle
└── redux/            # User state management

socketServer/         # Node.js Socket.io for real-time events
```

**Flow**: User books → Payment → Driver accept → GPS tracking/chat → OTP verify → Complete → Payout (admin commission).

## 🛠️ Tech Stack

| Category | Technologies |
|----------|--------------|
| Framework | Next.js 16 (App Router), React 19, TypeScript |
| Styling | TailwindCSS 4 |
| State | Redux Toolkit |
| DB/ORM | MongoDB, Mongoose |
| Auth | NextAuth v5 |
| Payments | Razorpay, Stripe |
| Real-time | Socket.io-client, @zegocloud/zego-uikit |
| Maps | React-Leaflet |
| Charts | Recharts |
| UI/Motion | Framer Motion, Lucide React |
| Utils | Cloudinary (images), Nodemailer |
| Deployment | Netlify |

See [`package.json`](package.json) for full deps.

## 🚀 Quick Start

### Prerequisites
- Node.js 20+
- MongoDB (Atlas or local)
- Accounts: Razorpay/Stripe/Cloudinary/NextAuth providers/Zego

### Environment Variables
Copy `.env.example` (create if missing) and fill:

```
MONGODB_URL=mongodb://...
NEXTAUTH_SECRET=...
NEXTAUTH_URL=http://localhost:3000

RAZORPAY_KEY_ID=...
RAZORPAY_KEY_SECRET=...

STRIPE_SECRET_KEY=...

CLOUDINARY_CLOUD_NAME=...
CLOUDINARY_API_KEY=...
CLOUDINARY_API_SECRET=...
```

### Development
1. Install deps:
   ```bash
   cd rydex
   npm install
   ```
2. Run Next.js:
   ```bash
   npm run dev
   ```
   Open [http://localhost:3000](http://localhost:3000)
3. (Optional) Socket server:
   ```bash
   cd ../socketServer
   npm install
   node index.js
   ```

### Production Build
```bash
npm run build
npm run start
```

## 🌐 Deployment (Netlify)

1. Push to GitHub.
2. Connect repo in Netlify dashboard.
3. Build settings auto-configured via `netlify.toml` (`npm run build`).
4. Add env vars in Netlify dashboard.

## 📚 API Overview

Key endpoints (`/api/...`):
- `POST /auth/register`, `/auth/verify-otp`
- `POST /booking/create`, `GET /booking/my-active`
- `POST /payment/create`, `/payment/verify`
- `GET /vehicles/nearby`
- Partner: `/partner/bookings/active`, `/partner/earnings`
- Admin: `/admin/dashboard`, `/admin/vendors/[id]/approve`

Full routes in `app/api/`.

## 🤝 Contributing

1. Fork & clone.
2. Create branch `feat/xyz`.
3. PR to `main`.

## 📄 License
MIT - see [LICENSE](LICENSE) (add if missing).

## 🙌 Support
Star the repo ⭐ & raise issues for bugs/features!

---
Built with ❤️ using Next.js.
