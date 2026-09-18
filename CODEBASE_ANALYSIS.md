# Rydex Codebase Analysis

## Overview
Rydex is a full-stack ride-sharing platform structured into two main parts:
1. **Frontend + Serverless API**: A Next.js 16 application located in the `rydex/` directory.
2. **Real-time Server**: A separate Node.js Socket.io server located in the `socketServer/` directory.

## Architecture and Technologies

### Frontend & API (`rydex/`)
- **Framework**: Next.js 16 (App Router), React 19, TypeScript
- **Styling**: Tailwind CSS 4, Framer Motion, Lucide React
- **State Management**: Redux Toolkit
- **Maps**: React-Leaflet, Leaflet
- **Auth**: NextAuth.js (v5)
- **Database**: MongoDB via Mongoose
- **Real-time Comms**: socket.io-client, @zegocloud/zego-uikit (for video KYC)
- **Payments**: Razorpay, Stripe
- **File Storage**: Cloudinary

### Real-time Server (`socketServer/`)
- **Runtime**: Node.js
- **Dependencies**: Express, Socket.io, Mongoose, Axios
- **Purpose**: Dedicated service for managing live connections, real-time vehicle location tracking, chat messaging between driver and rider, and live booking state transitions.

## Data Models (`rydex/src/models/`)
Key MongoDB schemas defined with Mongoose:
- **User** (`user.model.ts`): Manages riders, drivers (vendors), and admins. Handles driver onboarding steps, video KYC statuses, and real-time location mapping (GeoJSON Point).
- **Booking** (`booking.model.ts`): Orchestrates the ride lifecycle. Includes pickup/dropoff GeoJSON coordinates, booking status, payment details, OTPs, and references to User, Driver, and Vehicle.
- **Other Models**: `vehicle.model.ts`, `vehicleDocument.model.ts`, `partnerBank.model.ts`, and `chatMessage.model.ts`.

## Core Application Structure

### `rydex/src/app` Routes
- `api/`: API endpoints acting as the backend (auth, booking, chat, payment, socket, user, vehicles, zego, admin, partner, me).
- `admin/`: Administrator dashboard pages.
- `partner/`: Driver-facing application (dashboard, active rides, onboarding).
- `book/`, `ride/`: User-facing ride booking flows.
- `search/`: Finding nearby vehicles.
- `video-kyc/`: Video verification screens for drivers.

## Flow Summary
1. **Booking**: A user finds nearby drivers via GeoJSON-based searches and requests a ride.
2. **Real-time Coordination**: The `socketServer` broadcasts the request. Once accepted, live locations and chat run through WebSockets.
3. **Verification & Completion**: Secure pickups and drop-offs are facilitated via OTP. Payments are handled at the end of the lifecycle.
4. **Partner Payouts**: Admin deducts a commission, and partner amounts are calculated.