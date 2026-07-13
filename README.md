# QuickClinic

QuickClinic is a full-stack appointment-management application for patients and doctors. Patients can discover doctors, review available time slots, book appointments, and manage their appointment history. Doctors can manage profiles, availability, appointments, patients, notifications, and earnings.

## Features

- Role-based patient and doctor accounts with authentication
- Doctor discovery by name, location, speciality, experience, and consultation fee
- Doctor schedules and available time-slot selection
- Online/offline appointment booking and payment integration support
- Patient and doctor dashboards, appointment history, rescheduling, and cancellation
- Real-time chat with Socket.IO (local/self-hosted server)
- Email and in-app notifications
- Vercel deployment configuration for the React frontend and API

## Tech stack

- Frontend: React, TypeScript, React Router, Tailwind CSS, Material UI
- Backend: Node.js, Express, TypeScript, Socket.IO
- Database: MongoDB with Mongoose
- Services: Nodemailer, Razorpay, Firebase (optional), Vercel

## Project structure

```text
quickclinic/
|-- frontend/                 # React client
|-- backend/
|   |-- controllers/          # API business logic
|   |-- routes/               # Express route definitions
|   |-- models/               # MongoDB/Mongoose models
|   |-- config/               # Database and local environment configuration
|   `-- server.ts             # Local backend server entry point
|-- api/index.ts              # Vercel serverless API entry point
|-- vercel.json               # Vercel build and rewrite configuration
`-- DEPLOYMENT_GUIDE.md       # Additional deployment notes
```

## Prerequisites

- Node.js 18 or later
- npm
- A MongoDB database (MongoDB Atlas or local MongoDB)

## Local setup

1. Install root dependencies:

   ```bash
   npm install --legacy-peer-deps
   ```

2. Install frontend dependencies:

   ```bash
   npm install --prefix frontend --legacy-peer-deps
   ```

3. Create `backend/config/config.env` from `backend/config/example.env` and provide real values:

   ```env
   PORT=5000
   DB_URL=your_mongodb_connection_string
   JWT_SECRET=use_a_long_random_secret
   JWT_EXPIRE=7d
   COOKIE_EXPIRE=7
   SMPT_SERVICE=gmail
   SMPT_MAIL=your_email@example.com
   SMPT_PASSWORD=your_email_app_password
   SESSION_SECRET=use_another_long_random_secret
   NODE_ENV=development
   ```

   Never commit `config.env`, API keys, database URLs, or passwords. The project `.gitignore` already excludes this file.

4. Compile and start the backend from the project root:

   ```bash
   npm run build:backend
   npm start
   ```

   The backend listens at `http://localhost:5000` and API routes use the `/api/v1` prefix.

5. In a second terminal, start the frontend:

   ```bash
   npm start --prefix frontend
   ```

   Open `http://localhost:3000` in your browser.

### Frontend API URL

For local development, the default API URL is `http://localhost:5000/api/v1`. To override it, create `frontend/.env`:

```env
REACT_APP_API_URL=http://localhost:5000/api/v1
REACT_APP_SOCKET_URL=http://localhost:5000
```

Restart the React development server after changing environment variables.

## Useful commands

| Command | Description |
| --- | --- |
| `npm run build:backend` | Compile the TypeScript backend to `backend/dist` |
| `npm start` | Start the compiled backend on port 5000 |
| `npm run dev:backend` | Compile and run the backend with Nodemon |
| `npm run build --prefix frontend` | Create the React production build |
| `npm start --prefix frontend` | Start the React development server |
| `npm run vercel-build` | Build backend and frontend for Vercel |

## API overview

Route definitions live in `backend/routes/` and their implementations live in `backend/controllers/`. They are mounted under `/api/v1`.

Examples:

- `POST /api/v1/register` — register a user
- `POST /api/v1/login` — log in
- `GET /api/v1/:id/patient/specific_doctors` — search doctors
- `GET /api/v1/:id/patient/appointment_bookings?doc_id=:doctorId` — get a doctor's available slots
- `POST /api/v1/:id/patient/newappointment` — create an appointment

## Deployment

The repository includes `vercel.json` and `api/index.ts` for Vercel. Before deploying, configure these environment variables in the Vercel project settings:

- `DB_URL`
- `JWT_SECRET`
- `JWT_EXPIRE`
- `COOKIE_EXPIRE`
- `SMPT_SERVICE`
- `SMPT_MAIL`
- `SMPT_PASSWORD`
- `SESSION_SECRET`
- `NODE_ENV=production`

Set Razorpay and Firebase values too if those integrations are enabled. See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) for the detailed deployment checklist.

> Socket.IO requires a persistent server connection. Vercel's serverless functions do not support persistent Socket.IO connections, so use a dedicated backend host for production chat functionality.

## Security notes

- Keep all secrets exclusively in local or hosting-provider environment variables.
- Use a strong, unique `JWT_SECRET` and `SESSION_SECRET` in production.
- Restrict MongoDB network access appropriately and use secure database credentials.
