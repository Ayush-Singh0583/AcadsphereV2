running :
client :npm run dev
server :npm run dev
server:node seed.js





# AcadSphere — Academic Event Management Platform

A full-stack MERN application for managing formal academic and institutional events including hackathons, college fests, workshops, seminars, conferences, and technical competitions.

---





## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 + Vite |
| Backend | Node.js + Express.js |
| Database | MongoDB Atlas (Mongoose ODM) |
| Auth | JWT (jsonwebtoken + bcryptjs) |
| Charts | Recharts |
| Date handling | date-fns |
| HTTP Client | Axios |

---

## Project Structure

```
acadsphere/
├── server/                     # Express backend
│   ├── config/
│   │   └── db.js               # MongoDB connection
│   ├── controllers/
│   │   ├── authController.js   # Auth + user management
│   │   ├── eventController.js  # Events + sessions + dashboard
│   │   ├── participantController.js
│   │   ├── teamController.js
│   │   ├── submissionController.js
│   │   ├── judgingController.js
│   │   └── miscController.js   # Attendance, feedback, announcements, tasks
│   ├── middleware/
│   │   └── auth.js             # JWT protect + role authorize
│   ├── models/
│   │   ├── User.js
│   │   ├── Event.js            # Includes sessions sub-schema
│   │   ├── Team.js
│   │   ├── Participant.js
│   │   ├── Submission.js
│   │   ├── JudgingScore.js
│   │   ├── Attendance.js
│   │   ├── Feedback.js
│   │   ├── Announcement.js
│   │   └── VolunteerTask.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── events.js
│   │   └── index.js            # All other routes
│   ├── seed.js                 # Sample data seeder
│   ├── index.js                # Server entry point
│   ├── .env.example
│   └── package.json
│
└── client/                     # React + Vite frontend
    ├── src/
    │   ├── context/
    │   │   └── AuthContext.jsx  # Global auth state
    │   ├── pages/
    │   │   ├── Login.jsx
    │   │   ├── Register.jsx
    │   │   ├── Dashboard.jsx
    │   │   ├── Events.jsx
    │   │   ├── EventDetail.jsx
    │   │   ├── EventForm.jsx
    │   │   ├── SubEvents.jsx
    │   │   ├── Participants.jsx
    │   │   ├── Teams.jsx
    │   │   ├── Submissions.jsx
    │   │   ├── JudgePanel.jsx
    │   │   ├── Leaderboard.jsx
    │   │   ├── Announcements.jsx
    │   │   ├── Attendance.jsx
    │   │   ├── Feedback.jsx
    │   │   ├── VolunteerTasks.jsx
    │   │   ├── Calendar.jsx
    │   │   ├── Profile.jsx
    │   │   └── Users.jsx
    │   ├── components/
    │   │   ├── Layout.jsx
    │   │   ├── Sidebar.jsx
    │   │   └── Sidebar.module.css
    │   ├── services/
    │   │   └── api.js          # All Axios service calls
    │   ├── utils/
    │   │   └── helpers.js      # Date formatters, helpers
    │   ├── App.jsx             # Router + route guards
    │   ├── main.jsx
    │   └── index.css           # Full design system
    ├── index.html
    ├── vite.config.js
    └── package.json
```

---

## Quick Start

### Prerequisites
- Node.js v18+
- MongoDB Atlas account (free tier works)
- npm or yarn

---

### 1. Clone & Setup

```bash
git clone <your-repo-url>
cd acadsphere
```

---

### 2. Backend Setup

```bash
cd server
npm install
```

Create your `.env` file:
```bash
cp .env.example .env
```

Edit `.env`:
```env
PORT=5000
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/acadsphere?retryWrites=true&w=majority
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRE=7d
NODE_ENV=development
```

> **Get your MongoDB URI:**  
> 1. Go to [MongoDB Atlas](https://cloud.mongodb.com)  
> 2. Create a free cluster  
> 3. Click "Connect" → "Connect your application"  
> 4. Copy the connection string and replace `<username>` and `<password>`

---

### 3. Seed the Database (Optional but Recommended)

```bash
cd server
node seed.js
```

This creates 7 sample users and 5 events so you can log in immediately.

---

### 4. Start the Backend

```bash
cd server
npm run dev        # Development (with nodemon auto-reload)
# or
npm start          # Production
```

Server runs at: **http://localhost:5000**

---

### 5. Frontend Setup

```bash
cd client
npm install
npm run dev
```

Frontend runs at: **http://localhost:5173**

The Vite dev server proxies `/api` requests to `http://localhost:5000` automatically.

---

## Demo Credentials

| Role | Email | Password |
|---|---|---|
| Admin | admin@acad.com | admin123 |
| Organizer | org@acad.com | org123 |
| Participant | part@acad.com | part123 |
| Judge | judge@acad.com | judge123 |
| Volunteer | vol@acad.com | vol123 |

---

## API Reference

### Authentication
```
POST   /api/auth/register        Register new user
POST   /api/auth/login           Login → returns JWT
GET    /api/auth/me              Get current user (auth required)
PUT    /api/auth/profile         Update profile (auth required)
GET    /api/auth/users           List users (admin/organizer)
PUT    /api/auth/users/:id/role  Change user role (admin)
```

### Events
```
GET    /api/events               List events (with filters)
POST   /api/events               Create event (organizer/admin)
GET    /api/events/:id           Get single event
PUT    /api/events/:id           Update event
DELETE /api/events/:id           Delete event
GET    /api/events/:id/subevents List sub-events
POST   /api/events/:id/subevents Create sub-event
POST   /api/events/:id/sessions  Add session to agenda
GET    /api/events/:id/stats     Event statistics
GET    /api/events/dashboard     Dashboard stats
```

### Participants
```
POST   /api/participants/:eventId/register  Register for event
GET    /api/participants/:eventId           List participants
GET    /api/participants/:eventId/check     Check my registration
PUT    /api/participants/:id/status         Approve/reject participant
GET    /api/participants/my                 My registrations
```

### Teams
```
POST   /api/teams                Create team
POST   /api/teams/join           Join team (by invite code)
GET    /api/teams/event/:eventId List teams for event
GET    /api/teams/:id            Get team details
GET    /api/teams/my             My teams
```

### Submissions
```
POST   /api/submissions                       Submit project
GET    /api/submissions/event/:eventId        List submissions
GET    /api/submissions/event/:eventId/rankings  Recalculate rankings
GET    /api/submissions/:id                   Get submission
PUT    /api/submissions/:id                   Update submission
GET    /api/submissions/my                    My submissions
```

### Judging
```
POST   /api/judging/score                Score submission (judge)
PUT    /api/judging/finalize/:subId      Finalize score
GET    /api/judging/scores/:subId        Get scores for submission
GET    /api/judging/my/:eventId          Judge's own scores
GET    /api/judging/leaderboard/:eventId Event leaderboard
```

### Other Modules
```
POST   /api/attendance                   Mark attendance session
GET    /api/attendance/:eventId          Get attendance records
GET    /api/attendance/:eventId/report   Attendance report

POST   /api/feedback/:eventId            Submit feedback
GET    /api/feedback/:eventId            Get feedback (organizer)

POST   /api/announcements/:eventId       Create announcement
GET    /api/announcements/:eventId       List announcements
DELETE /api/announcements/:id            Delete announcement

POST   /api/tasks                        Create volunteer task
GET    /api/tasks/:eventId               List tasks
PUT    /api/tasks/:id/status             Update task status
```

---

## Sample API Calls

### Register a User
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Alice Smith",
    "email": "alice@mit.edu",
    "password": "secure123",
    "role": "participant",
    "institution": "MIT"
  }'
```

### Login
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "org@acad.com", "password": "org123"}'
```

### Create an Event (with token)
```bash
curl -X POST http://localhost:5000/api/events \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -d '{
    "title": "DataHack 2025",
    "description": "48-hour data science hackathon",
    "category": "hackathon",
    "status": "published",
    "startDate": "2025-06-01T09:00:00Z",
    "endDate": "2025-06-03T09:00:00Z",
    "venue": "IIT Delhi",
    "maxParticipants": 500,
    "rules": ["Teams of 2-4", "Datasets provided on day 1"],
    "tags": ["Data Science", "Python", "Prize Money"]
  }'
```

### Register for Event
```bash
curl -X POST http://localhost:5000/api/participants/<eventId>/register \
  -H "Authorization: Bearer <participant_token>" \
  -H "Content-Type: application/json" \
  -d '{}'
```

### Score a Submission (as Judge)
```bash
curl -X POST http://localhost:5000/api/judging/score \
  -H "Authorization: Bearer <judge_token>" \
  -H "Content-Type: application/json" \
  -d '{
    "submissionId": "<submission_id>",
    "eventId": "<event_id>",
    "criteria": {
      "innovation": {"score": 9, "comment": "Highly original approach"},
      "technicalComplexity": {"score": 8, "comment": "Well-engineered solution"},
      "uiUx": {"score": 7, "comment": "Clean and intuitive UI"},
      "impact": {"score": 9, "comment": "Addresses real climate problem"},
      "presentation": {"score": 8, "comment": "Clear demo and documentation"}
    },
    "overallComment": "Impressive project with strong real-world potential."
  }'
```

---

## User Role Capabilities

| Feature | Admin | Organizer | Participant | Judge | Volunteer |
|---|:---:|:---:|:---:|:---:|:---:|
| Create Events | ✓ | ✓ | | | |
| Edit Events | ✓ | ✓(own) | | | |
| View Events | ✓ | ✓ | ✓ | ✓ | ✓ |
| Register for Events | | | ✓ | | |
| Create Teams | | | ✓ | | |
| Submit Projects | | | ✓ | | |
| Score Submissions | ✓ | | | ✓ | |
| View Participants | ✓ | ✓ | | | ✓ |
| Mark Attendance | ✓ | ✓ | | | ✓ |
| Post Announcements | ✓ | ✓ | | | |
| Assign Tasks | ✓ | ✓ | | | |
| Manage Users | ✓ | | | | |
| View Leaderboard | ✓ | ✓ | ✓ | ✓ | ✓ |
| View Analytics | ✓ | ✓ | | | |

---

## Database Schema Overview

### Users
```json
{ "name": "string", "email": "string (unique)", "password": "hashed", "role": "enum[admin|organizer|participant|judge|volunteer]", "institution": "string", "bio": "string", "skills": ["string"], "socialLinks": { "github": "", "linkedin": "", "portfolio": "" } }
```

### Events
```json
{ "title": "string", "description": "string", "category": "enum[hackathon|fest|workshop|seminar|conference|competition|other]", "status": "enum[draft|published|ongoing|completed|cancelled|archived]", "startDate": "Date", "endDate": "Date", "venue": "string", "isOnline": "boolean", "rules": ["string"], "tags": ["string"], "organizer": "ref:User", "sessions": [{ "title": "string", "speaker": "string", "startTime": "Date", "endTime": "Date", "venue": "string", "type": "enum" }], "parentEvent": "ref:Event", "isSubEvent": "boolean" }
```

### JudgingScore
```json
{ "submission": "ref:Submission", "judge": "ref:User", "event": "ref:Event", "criteria": { "innovation": { "score": 0-10, "comment": "" }, "technicalComplexity": { "score": 0-10 }, "uiUx": { "score": 0-10 }, "impact": { "score": 0-10 }, "presentation": { "score": 0-10 } }, "totalScore": "auto-calculated avg", "isFinalized": "boolean" }
```

---

## Production Deployment

### Backend (Railway / Render / Fly.io)
1. Set environment variables on your hosting platform
2. Set `NODE_ENV=production`
3. Deploy from your `server/` directory

### Frontend (Vercel / Netlify)
1. Build: `npm run build` in `client/`
2. Set `VITE_API_URL=https://your-backend-url.com`
3. Update `vite.config.js` proxy → use env variable for production

### Update CORS in production
In `server/index.js`, update:
```js
app.use(cors({ origin: ['https://your-frontend.vercel.app'], credentials: true }));
```

---

## Environment Variables Summary

### Server (.env)
```
PORT=5000
MONGODB_URI=mongodb+srv://...
JWT_SECRET=change_this_in_production
JWT_EXPIRE=7d
NODE_ENV=development
```

---

## License
MIT — Free for academic and personal use.

Built with ❤️ for the academic community.
# AcadsphereV2
