# SportScore Live

A modern, responsive real-time sports scorecard application for Cricket (International + IPL) and Football matches.

## Features

- **Live Match Updates** - Auto-refresh every 15 seconds
- **Cricket & Football** - Switch between sports with one click
- **Match Status Tabs** - View Live, Upcoming, and Completed matches
- **Detailed Scorecards** - Batting/bowling stats (Cricket) or possession/stats (Football)
- **Search** - Find matches by team, series, or venue
- **Favorites** - Save your favorite teams (persisted to localStorage)
- **Dark/Light Mode** - Toggle theme with system preference detection
- **IPL Section** - Special highlights page with points table
- **Mobile-First Design** - Responsive layout for all devices

## Tech Stack

- **Frontend:** React 18, Vite, Tailwind CSS, React Router v6
- **Backend:** Node.js, Express.js
- **State Management:** React Context API
- **Real-time:** Polling with 15-second intervals

## Project Structure

```
sports-score-app/
├── backend/
│   ├── src/
│   │   ├── index.js           # Express server entry
│   │   ├── routes/
│   │   │   └── matches.js     # API endpoints
│   │   ├── data/
│   │   │   └── mockData.js    # Realistic mock match data
│   │   └── utils/
│   │       └── matchSimulator.js  # Real-time score simulation
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/         # Reusable UI components
│   │   ├── pages/             # Page components
│   │   ├── context/           # React Context providers
│   │   ├── hooks/             # Custom hooks
│   │   └── utils/             # Utility functions
│   ├── package.json
│   └── vite.config.js
├── SPEC.md                    # Full specification
└── README.md
```

## Setup Instructions

### Prerequisites
- Node.js 18+ installed
- npm or yarn package manager

### Installation

1. **Clone or navigate to the project directory**
   ```bash
   cd sports-score-app
   ```

2. **Install Backend Dependencies**
   ```bash
   cd backend
   npm install
   ```

3. **Install Frontend Dependencies**
   ```bash
   cd ../frontend
   npm install
   ```

### Running the Application

1. **Start the Backend Server**
   ```bash
   cd backend
   npm start
   ```
   Backend runs on: `http://localhost:3001`

2. **Start the Frontend (in a new terminal)**
   ```bash
   cd frontend
   npm run dev
   ```
   Frontend runs on: `http://localhost:5173`

3. **Open your browser** and navigate to `http://localhost:5173`

### Building for Production

```bash
cd frontend
npm run build
```

Production files will be generated in `frontend/dist/`

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/matches` | GET | Get all matches (query: sport, status, search) |
| `/api/matches/:id` | GET | Get match details |
| `/api/matches/live` | GET | Get live matches |
| `/api/matches/upcoming` | GET | Get upcoming matches |
| `/api/matches/completed` | GET | Get completed matches |
| `/api/matches/stream` | GET | SSE stream for real-time updates |
| `/api/teams/all` | GET | Get all teams |
| `/api/health` | GET | Health check |

## Environment Variables

### Backend (.env)
```
PORT=3001
```

### Frontend (.env)
```
VITE_API_URL=http://localhost:3001
```

## Mock Data

The application includes realistic mock data:

### Cricket Matches
- 3 Live: India vs Australia (T20 WC), England vs South Africa (Champions Trophy), MI vs CSK (IPL)
- 3 Upcoming: Pakistan vs New Zealand, RCB vs KKR, England vs India (Test)
- 2 Completed: Sri Lanka vs Bangladesh, DC vs SRH

### Football Matches
- 3 Live: Man United vs Liverpool (Premier League), Real Madrid vs Barcelona (El Clasico), Mumbai City vs Bengaluru (ISL)
- 2 Upcoming: Chelsea vs Arsenal, Bayern vs Dortmund
- 1 Completed: Juventus vs Inter (Serie A)

## Features Explained

### Auto-Refresh
Matches automatically refresh every 15 seconds. The header shows "Updated X seconds ago".

### Search
Click the search icon (or press Ctrl+K) to search for:
- Team names (e.g., "India", "Barcelona")
- Series names (e.g., "IPL", "Premier League")
- Venues (e.g., "Wankhede", "Old Trafford")

Recent searches are saved to localStorage.

### Favorites
Click the heart icon on any match card to add both teams to favorites. Use the heart toggle in the header to filter to only favorite teams.

### Dark Mode
Click the sun/moon icon to toggle dark/light mode. The preference is saved to localStorage and respects system preference on first load.

### IPL Highlights
Navigate to `/ipl` or click "IPL 2026" in the header to see:
- Live stats (matches by status)
- Points table
- Match list

## Deployment

### Option 1: Static Hosting (Frontend only)
1. Build: `npm run build`
2. Deploy the `dist/` folder to any static hosting service (Vercel, Netlify, etc.)
3. Set `VITE_API_URL` to your backend URL

### Option 2: Full Stack Deployment
1. Deploy backend to a Node.js hosting service (Railway, Render, Heroku, etc.)
2. Deploy frontend as static hosting
3. Update `VITE_API_URL` in frontend to point to your backend

## License

MIT License
