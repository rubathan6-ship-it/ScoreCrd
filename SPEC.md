# SportScore Live - Real-time Sports Scorecard Application

## 1. Project Overview

**Project Name:** SportScore Live
**Type:** Full-stack real-time sports dashboard
**Core Functionality:** Live scorecards for Cricket (International + IPL) and Football matches with auto-refresh, detailed match stats, and team favorites.
**Target Users:** Sports enthusiasts who want real-time match updates

---

## 2. Technical Architecture

### Frontend Stack
- **Framework:** React 18 with Vite
- **Styling:** Tailwind CSS
- **State Management:** React Context API
- **Real-time:** Polling (15-second intervals)
- **Icons:** Lucide React
- **Routing:** React Router v6

### Backend Stack
- **Runtime:** Node.js
- **Framework:** Express.js
- **Real-time:** Server-Sent Events (SSE) for live updates
- **Data:** Mock data generator with realistic sports data

### Project Structure
```
sports-score-app/
├── backend/
│   ├── src/
│   │   ├── index.js
│   │   ├── routes/
│   │   │   └── matches.js
│   │   ├── data/
│   │   │   └── mockData.js
│   │   └── utils/
│   │       └── matchSimulator.js
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── context/
│   │   ├── hooks/
│   │   └── utils/
│   ├── package.json
│   └── vite.config.js
└── SPEC.md
```

---

## 3. UI/UX Specification

### Color Palette

**Light Mode:**
- Primary: `#1a56db` (Royal Blue)
- Secondary: `#059669` (Emerald Green)
- Accent Live: `#dc2626` (Red - for live indicators)
- Background: `#f8fafc` (Slate 50)
- Card Background: `#ffffff`
- Text Primary: `#0f172a` (Slate 900)
- Text Secondary: `#64748b` (Slate 500)
- Border: `#e2e8f0` (Slate 200)

**Dark Mode:**
- Primary: `#3b82f6` (Blue 500)
- Secondary: `#10b981` (Emerald 500)
- Accent Live: `#ef4444` (Red 500)
- Background: `#0f172a` (Slate 900)
- Card Background: `#1e293b` (Slate 800)
- Text Primary: `#f8fafc` (Slate 50)
- Text Secondary: `#94a3b8` (Slate 400)
- Border: `#334155` (Slate 700)

**Cricket Accent Colors:**
- Team A: `#2563eb` (Blue 600)
- Team B: `#7c3aed` (Violet 600)
- Wicket: `#dc2626` (Red 600)
- Six: `#f59e0b` (Amber 500)
- Four: `#3b82f6` (Blue 500)

**Football Accent Colors:**
- Home Team: `#1d4ed8` (Blue 700)
- Away Team: `#b91c1c` (Red 700)
- Goal: `#22c55e` (Green 500)
- Card (Yellow): `#eab308` (Yellow 500)
- Card (Red): `#ef4444` (Red 500)

### Typography
- **Font Family:** Inter (Google Fonts)
- **Headings:** 600-700 weight
- **Body:** 400-500 weight
- **Scores:** 700 weight, tabular-nums
- **Scale:**
  - H1: 24px
  - H2: 20px
  - H3: 16px
  - Body: 14px
  - Caption: 12px

### Spacing System
- Base unit: 4px
- Card padding: 16px (p-4)
- Section gaps: 24px (gap-6)
- Component margins: 8px-16px

### Visual Effects
- Card shadows: `shadow-sm` (default), `shadow-md` (hover)
- Border radius: 12px (cards), 8px (buttons), 6px (badges)
- Live pulse animation: 2s infinite pulse on live indicator
- Score update animation: brief scale(1.05) on change
- Smooth transitions: 150ms ease-out

### Layout Structure

**Header:**
- Logo (left)
- Sport toggle: Cricket | Football (center)
- Theme toggle + Search icon (right)
- Height: 64px

**Tab Navigation:**
- Three tabs: Live | Upcoming | Completed
- Active tab: primary color underline
- Sticky below header

**Main Content:**
- Max width: 1280px (centered)
- Mobile: single column
- Tablet (768px+): 2 columns for match cards
- Desktop (1024px+): 3 columns for match cards

**Match Card:**
- Width: 100% (mobile), 48% (tablet), 32% (desktop)
- Min-height: 180px
- Shows: team logos, scores, status, venue

**Match Details Page:**
- Full width
- Sections: Header, Scorecard (Cricket) / Stats (Football)
- Back button in header

---

## 4. Component Specifications

### MatchCard Component
**Props:** match, sport, onClick
**States:**
- Default: white/dark card
- Live: pulsing red dot, "LIVE" badge
- Upcoming: clock icon, start time
- Completed: checkmark, final score
**Hover:** shadow-md, slight scale(1.02)

### TeamBadge Component
**Props:** team, score, extras (overs/runs)
**Layout:** Logo | Team Name | Score
**Cricket extras:** Show CRR (current run rate)
**Football extras:** Show formation

### ScoreCard Component (Cricket Detail)
**Sections:**
- Batting table: Player, dismissal, runs, balls, 4s, 6s, SR
- Bowling table: Bowler, overs, maidens, runs, wickets, ECON
- Fall of wickets
- Partnership

### MatchStats Component (Football Detail)
**Sections:**
- Score timeline
- Team stats: Possession, Shots, Shots on Target, Corners, Fouls
- Goal scorers with assists
- Cards

### SearchModal Component
- Overlay modal
- Search input with debounce (300ms)
- Results grouped by: Live, Upcoming, Completed
- Recent searches (localStorage)

### FavoriteButton Component
- Heart icon
- Toggle favorite (localStorage)
- Animate on click (scale bounce)

---

## 5. Data Models

### Match (Base)
```javascript
{
  id: string,
  sport: 'cricket' | 'football',
  status: 'live' | 'upcoming' | 'completed',
  series: string,
  venue: string,
  startTime: ISO8601,
  updatedAt: ISO8601
}
```

### CricketMatch extends Match
```javascript
{
  teams: [
    { name, shortName, logo, score, overs, crr, wickets }
  ],
  currentInnings: 1 | 2,
  battingTeam: 0 | 1,
  batsmen: [{ name, runs, balls, fours, sixes, dismissal, striker }],
  bowlers: [{ name, overs, maidens, runs, wickets, econ }],
  recentBalls: ['1', '4', '6', 'W', '0', '2'],
  fallOfWickets: [{ wicket, score, over }]
}
```

### FootballMatch extends Match
```javascript
{
  teams: [
    { name, shortName, logo, score, possession }
  ],
  minute: number,
  period: '1st' | '2nd' | 'ET' | 'PEN' | 'FT',
  events: [{ type, minute, player, assist?, team }],
  stats: {
    possession: [home%, away%],
    shots: [home, away],
    shotsOnTarget: [home, away],
    corners: [home, away],
    fouls: [home, away],
    yellowCards: [home, away],
    redCards: [home, away]
  }
}
```

---

## 6. Backend API Endpoints

### GET /api/matches
**Query params:** `sport`, `status`, `search`
**Response:** `{ matches: Match[], lastUpdated: timestamp }`

### GET /api/matches/:id
**Response:** Full Match object with all details

### GET /api/matches/stream
**Response:** SSE stream with match updates every 15 seconds

### GET /api/teams
**Response:** List of all teams with favorites count

---

## 7. Features Implementation

### Auto-Refresh
- Poll /api/matches every 15 seconds
- Compare with current state
- Animate score changes
- Show "Updated X seconds ago" indicator

### Dark/Light Mode
- Toggle in header
- Persist to localStorage
- System preference detection
- CSS variables for theme colors

### Search
- Debounced search input
- Search by team name, series, venue
- Highlight matching text
- Recent searches (last 5, localStorage)

### Favorites
- Heart button on team badges
- Persist to localStorage
- Filter option: Show only favorites
- Badge count in header

### IPL Section
- Featured IPL matches on homepage
- Special badge: "IPL 2026"
- Orange cap / Purple cap leaderboards (simplified)

---

## 8. Mock Data Strategy

### Cricket Matches (8 total)
- 3 Live: India vs Australia (T20), England vs South Africa (ODI), MI vs CSK (IPL)
- 3 Upcoming: Pakistan vs New Zealand, RCB vs KKR, India vs England (Test)
- 2 Completed: Sri Lanka vs Bangladesh, DC vs SRH

### Football Matches (6 total)
- 3 Live: Manchester United vs Liverpool, Real Madrid vs Barcelona, ISL match
- 2 Upcoming: Chelsea vs Arsenal, Bayern vs Dortmund
- 1 Completed: Juventus vs Inter

### Data Simulation
- Live scores update every 15 seconds
- Random: runs, wickets, overs progression
- Realistic: normal distribution around expected rates
- Occasional events: boundaries, wickets, goals

---

## 9. Loading & Error States

### Loading Skeleton
- Match card skeleton: gray animated blocks
- 3 cards on mobile, 6 on desktop
- Pulse animation

### Error State
- Icon + message
- Retry button
- "Unable to load matches. Check your connection."

### Empty State
- "No matches found"
- Suggestion text based on active filter

---

## 10. Responsive Breakpoints

| Breakpoint | Width | Layout |
|------------|-------|--------|
| Mobile | <640px | 1 column |
| Tablet | 640-1023px | 2 columns |
| Desktop | 1024px+ | 3 columns |

### Header (Mobile)
- Logo centered
- Sport toggle below
- Icons in footer bar or hamburger menu

---

## 11. Accessibility

- Semantic HTML
- ARIA labels on interactive elements
- Keyboard navigation
- Focus visible states
- Color contrast ratios (WCAG AA)
- Screen reader announcements for live updates

---

## 12. Performance Targets

- First Contentful Paint: <1.5s
- Time to Interactive: <3s
- Lighthouse Score: 90+
- Bundle size: <200KB (gzipped)

---

## 13. Deployment Instructions

### Backend
```bash
cd backend
npm install
npm start
# Runs on http://localhost:3001
```

### Frontend
```bash
cd frontend
npm install
npm run dev
# Runs on http://localhost:5173
```

### Environment Variables
```
# backend/.env
PORT=3001
REFRESH_INTERVAL=15000

# frontend/.env
VITE_API_URL=http://localhost:3001
```
