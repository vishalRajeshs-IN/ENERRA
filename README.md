# ENERRA ⚡ — Version 1.5

Full-stack electrical distribution and smart-meter service prototype built with React, Vite, Node.js, Express, MongoDB, Mongoose and Leaflet.

## v1.5 features

- Role-based access for consumers, technicians and operators.
- Consumer login using an 8-digit smart-meter ID and registered phone number.
- Consumer smart-meter page with latest stored reading, meter status, prepaid balance, usage chart and reading history.
- Consumer complaint reporting and complaint-status tracking.
- Technician workspace for manually creating outage notices and updating planned / active / restored status.
- Operator workspace focused on complaints, meter availability and low-balance alerts, meter payment records, and recorded load-usage graph/counts.
- Operator can mark a meter online, offline or disconnected.
- Pay Bill page, public sign-up link and demo-access shortcuts removed from the UI.
- Responsive interface with refined cards, rounded panels, chart visuals and role-specific navigation.

## Requirements

- Node.js 18 or later
- MongoDB local instance or MongoDB Atlas database

## Local setup

1. Extract the ZIP archive.
2. Copy `server/.env.example` to `server/.env`.
3. Configure `MONGO_URI`, a long random `JWT_SECRET`, and `CLIENT_ORIGINS`.
4. Start the backend:

   ```bash
   cd server
   npm install
   npm run seed
   npm run dev
   ```

5. In a second terminal, start the frontend:

   ```bash
   cd client
   npm install
   npm run dev
   ```

6. Open the Vite URL shown in the terminal (normally `http://localhost:5173`).

## Seeded local test credentials

Run `npm run seed` in `server` to create test accounts. These are for local development only.

- **Consumer:** meter ID `12345678`, registered phone `9876543210`
- **Operator:** `operator@enerra.demo` / `ENERRADemo123!`
- **Technician:** `technician@enerra.demo` / `ENERRADemo123!`

Change staff credentials and secrets before deployment. The consumer test meter and phone are stored in the development database by the seed script.

## Environment variables

`server/.env`:

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/enerra
JWT_SECRET=replace_with_a_long_random_secret
CLIENT_ORIGINS=http://localhost:5173,https://your-enerra.vercel.app
```

`client/.env` (only if your API is not at the default local URL):

```env
VITE_API_URL=http://localhost:5000/api
```

For Render, set `MONGO_URI`, `JWT_SECRET`, and `CLIENT_ORIGINS` in the service environment. `CLIENT_ORIGINS` accepts comma-separated frontend origins. The backend also permits ENERRA-named Vercel preview origins to support preview deployments.

## Main API routes

- `POST /api/auth/login` — staff login
- `POST /api/auth/consumer-access` — consumer meter ID + phone verification
- `GET /api/meter` — authenticated consumer meter details and readings
- `GET /api/outages` — outage notices
- `POST /api/outages` — technician/operator creates a manual outage notice
- `PATCH /api/outages/:id/status` — technician/operator updates outage status
- `GET /api/complaints` — consumer's own complaints or all complaints for staff
- `POST /api/complaints` — create complaint, optional photo upload
- `PATCH /api/complaints/:id` — staff complaint workflow update
- `GET /api/operator/overview` — operator complaints, meters, payment records and readings
- `PATCH /api/operator/meters/:id/status` — operator updates meter status
- `GET /api/health` — backend health check

## Important limitations

- Meter readings and charts display stored/manual/sample data unless a real smart-meter gateway is connected. API connectivity alone does not mean live telemetry is available.
- Meter online/offline status is operator-maintained in this release; automated device heartbeat detection is not yet integrated.
- Payment rows are records from the billing collection. This release does not process real payments or connect a payment gateway.
- The seeded meter and staff credentials are development-only examples. Replace them before production use.
- Manual outage notices are operator/technician-entered information, not automatic grid fault detection.
- Demo map coordinates must not be treated as real electrical infrastructure locations.


## ENERRA v1.5 branding
- Product name changed from GridWatch to ENERRA throughout the user interface.
- TNPDCL logo added to the sign-in experience, application sidebar and browser favicon.
- Existing prototype workflows and API paths are retained.
- This is a prototype representation; it does not imply official TNPDCL deployment, endorsement or connection to live utility systems.
