# 🤖 VS CODE COPILOT PROMPTS - SAFESTRIDE WORKOUT GENERATOR

**Date**: March 5, 2026  
**Goal**: Build Strava OAuth + AI Workout Generator  
**Project**: SafeStride by AKURA  

---

## 🎯 SYSTEM OVERVIEW

### **What We're Building**:

1. **Strava OAuth Integration** - Connect athlete's Strava account
2. **Activity Analysis** - Analyze past workouts (e.g., 03/Mar/2026 data)
3. **AISRI Calculation** - Calculate athlete's injury risk score
4. **AI Workout Generator** - Generate next workout (e.g., 05/Mar/2026 tempo run)
5. **Evaluation Form** - Athlete completes assessment
6. **Personalized Training Plan** - 12-week plan based on AISRI + past data

### **Example Flow**:

```
Athlete Signs Up (05/Mar/2026)
    ↓
Connects Strava (OAuth)
    ↓
System Analyzes Past Data (03/Mar/2026 workout)
    ↓
Athlete Completes Evaluation Form (6 pillars)
    ↓
System Calculates AISRI Score (e.g., 58)
    ↓
AI Generates Workout for 05/Mar/2026 (Tempo Run, Zone 2)
    ↓
System Creates 12-Week Training Plan
```

---

## 📁 PROJECT STRUCTURE TO CREATE

```
webapp/
├── src/
│   ├── api/
│   │   ├── strava-auth.js          # Strava OAuth handler
│   │   ├── strava-webhook.js       # Receive activity updates
│   │   ├── strava-activities.js    # Fetch athlete activities
│   │   ├── aisri-calculator.js     # Calculate AISRI score
│   │   ├── workout-generator.js    # AI workout generator
│   │   └── training-plan.js        # Generate 12-week plan
│   ├── services/
│   │   ├── strava-client.js        # Strava API client
│   │   ├── aisri-engine.js         # AISRI calculation logic
│   │   ├── workout-ai.js           # AI workout generation
│   │   └── plan-builder.js         # Training plan builder
│   └── utils/
│       ├── token-manager.js        # Manage OAuth tokens
│       └── date-utils.js           # Date/time helpers
├── public/
│   └── strava-callback.html        # OAuth redirect page
└── migrations/
    └── 006_strava_integration.sql  # New database tables
```

---

## 🔐 PHASE 1: STRAVA OAUTH INTEGRATION

### **Prompt 1: Create Strava OAuth Handler**

**File**: `src/api/strava-auth.js`

**Copilot Prompt**:
```
Create a Strava OAuth authentication handler for a Hono.js app running on Cloudflare Workers using JavaScript (not JavaScript).

Requirements:
- Use Strava OAuth 2.0 flow (authorization code grant)
- Client ID and Client Secret from environment variables
- Redirect URI: https://safestride.pages.dev/strava-callback
- Scopes: activity:read_all, activity:write
- Store access_token, refresh_token, expires_at in D1 database
- Handle token refresh automatically when expired
- Return athlete profile data after successful auth

Database table schema:
```sql
CREATE TABLE strava_connections (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  athlete_id TEXT NOT NULL,
  strava_athlete_id INTEGER NOT NULL,
  access_token TEXT NOT NULL,
  refresh_token TEXT NOT NULL,
  expires_at INTEGER NOT NULL,
  scope TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

API Endpoints:
- GET /api/strava/auth - Start OAuth flow
- GET /api/strava/callback - Handle OAuth callback
- POST /api/strava/refresh - Refresh expired token
- GET /api/strava/disconnect - Revoke access
```

---

### **Prompt 2: Create Strava API Client**

**File**: `src/services/strava-client.js`

**Copilot Prompt**:
```
Create a Strava API client service for Cloudflare Workers with JavaScript.

Requirements:
- Fetch athlete activities from Strava API v3
- Handle pagination (max 200 activities per page)
- Support date range filtering (after, before timestamps)
- Auto-refresh tokens if expired
- Return activities with: id, name, type, distance, moving_time, average_speed, average_heartrate, max_heartrate, elev_high, start_date

Functions needed:
- getAthleteActivities(athleteId, options?: { after?, before?, page?, per_page? })
- getActivityById(athleteId, activityId)
- getAthleteStats(athleteId)
- refreshAccessToken(athleteId)

Strava API endpoints:
- GET https://www.strava.com/api/v3/athlete/activities
- GET https://www.strava.com/api/v3/activities/{id}
- GET https://www.strava.com/api/v3/athletes/{id}/stats
- POST https://www.strava.com/oauth/token (refresh)
```

---

### **Prompt 3: Create OAuth Callback Page**

**File**: `public/strava-callback.html`

**Copilot Prompt**:
```
Create an HTML page to handle Strava OAuth callback.

Requirements:
- Extract 'code' and 'state' from URL query parameters
- Show loading spinner while processing
- Send code to backend API /api/strava/callback
- On success: Redirect to athlete dashboard with success message
- On error: Show error message with retry button
- Use Tailwind CSS for styling
- Add SafeStride branding (purple theme #9333EA)
```

---

## 📊 PHASE 2: ACTIVITY ANALYSIS

### **Prompt 4: Analyze Past Activities**

**File**: `src/services/activity-analyzer.js`

**Copilot Prompt**:
```
Create an activity analyzer service that processes Strava activities and calculates training metrics.

Given: List of athlete activities from Strava API

Calculate:
1. Training Consistency Score (0-100)
   - % of weeks with 3+ runs (last 12 weeks)
   - Longest streak of consecutive training weeks

2. Average Weekly Volume
   - Total distance per week (last 4, 8, 12 weeks)
   - Trend: increasing, stable, decreasing

3. Pace Analysis
   - Average easy run pace (Zone 1-2)
   - Average tempo pace (Zone 3)
   - Average interval pace (Zone 4-5)
   - Pace improvement trend

4. Heart Rate Analysis (if available)
   - Average HR by zone
   - Max HR reached
   - HR variability

5. Training Load
   - Calculate TSS (Training Stress Score) per activity
   - Acute Training Load (ATL) - 7-day average
   - Chronic Training Load (CTL) - 42-day average
   - Training Stress Balance (TSB) = CTL - ATL

6. Injury Risk Indicators
   - Rapid volume increase (>10% per week)
   - Insufficient recovery (< 1 rest day per week)
   - High-intensity overload (>20% Zone 4-5)

Return: ActivityAnalysis object with all metrics
```

---

### **Prompt 5: Calculate Running Pillar Score**

**File**: `src/services/running-pillar.js`

**Copilot Prompt**:
```
Create a Running Pillar calculator for AISRI system.

Input: ActivityAnalysis data from activity-analyzer.js

Calculate Running Pillar Score (0-100) based on:
- Training Consistency (30%): Higher score for consistent training
- Volume Appropriateness (20%): Optimal weekly distance (not too high/low)
- Pace Progression (20%): Improvement over time
- Recovery Pattern (15%): Adequate rest days
- Heart Rate Management (10%): Proper zone distribution
- Injury History (5%): Fewer gaps indicate fewer injuries

Formula:
Running Score = (
  Consistency * 0.30 +
  Volume * 0.20 +
  Pace * 0.20 +
  Recovery * 0.15 +
  HR * 0.10 +
  History * 0.05
)

Return: { score: number, breakdown: object, recommendations: string[] }
```

---

## 🧮 PHASE 3: AISRI CALCULATION ENGINE

### **Prompt 6: Build AISRI Calculator**

**File**: `src/services/aisri-engine.js`

**Copilot Prompt**:
```
Create a comprehensive AISRI (Athletic Injury & Strength Risk Index) calculator.

Inputs:
1. Physical Assessment Data (from evaluation form):
   - Mobility: ROM tests, flexibility scores
   - Strength: Single-leg squat, plank time, balance
2. Running Pillar Score (from Strava analysis)
3. Questionnaire Data:
   - Mental resilience, recovery quality, injury history

6 Pillars:
1. Mobility & Flexibility (15% weight)
2. Core Strength & Stability (15% weight)
3. Mental Resilience (10% weight)
4. Recovery & Regeneration (15% weight)
5. Injury Prevention (20% weight)
6. Performance Optimization (25% weight) = Running Pillar Score

Calculate:
AISRI = (
  Mobility * 0.15 +
  Strength * 0.15 +
  Mental * 0.10 +
  Recovery * 0.15 +
  Prevention * 0.20 +
  Performance * 0.25
)

Risk Categories:
- 0-39: HIGH RISK (Red) → Zone 1 only
- 40-59: MODERATE RISK (Orange) → Zone 1-2
- 60-79: LOW RISK (Yellow) → Zone 1-3
- 80-89: MINIMAL RISK (Light Green) → Zone 1-4
- 90-100: OPTIMAL (Green) → Zone 1-5

Return: { 
  score: number, 
  category: string, 
  allowedZones: number[], 
  pillarBreakdown: object,
  recommendations: string[]
}
```

---

## 🤖 PHASE 4: AI WORKOUT GENERATOR

### **Prompt 7: Create AI Workout Generator**

**File**: `src/services/workout-ai.js`

**Copilot Prompt**:
```
Create an AI-powered workout generator for SafeStride.

Context:
- Athlete signed up on 05/Mar/2026
- We have their past Strava data (e.g., 03/Mar/2026: 8km easy run)
- We have their AISRI score (e.g., 58 = Moderate Risk)
- We need to generate a workout for 05/Mar/2026 (e.g., Tempo Run)

Inputs:
1. Current Date (targetDate): 05/Mar/2026
2. AISRI Score: 58
3. Allowed Zones: [1, 2]
4. Recent Activities: Last 7 days of training
5. Training Phase: Foundation (0-500km total)
6. Race Goal: 5K in 3 months
7. Current 5K Pace: 6:00/km

AI Workout Generation Logic:

Step 1: Analyze Recent Training
- Check last workout date (03/Mar/2026)
- Check workout type (Easy Run)
- Check volume trend (increasing/stable/decreasing)

Step 2: Determine Workout Type (7 Protocols)
- START: Recovery (Zone 1, 30-40 min)
- ENGINE: Aerobic base (Zone 2, 40-60 min)
- STRENGTH: Gym/bodyweight
- OXYGEN: VO2 max intervals (Zone 4-5, blocked by safety gate)
- POWER: Hill sprints (Zone 5, blocked by safety gate)
- ZONES: Tempo (Zone 3, blocked by safety gate)
- LONG RUN: Endurance (Zone 2, 60-120 min)

Step 3: Apply Safety Gates
- AISRI 58 → Only Zone 1-2 allowed
- Cannot assign ZONES, OXYGEN, or POWER workouts
- Can only assign: START, ENGINE, STRENGTH, LONG RUN

Step 4: Weekly Structure Logic
- Monday: STRENGTH
- Tuesday: START (Recovery)
- Wednesday: ENGINE (Aerobic)
- Thursday: STRENGTH or Rest
- Friday: ENGINE or START
- Saturday: LONG RUN
- Sunday: Rest

Step 5: Calculate Workout Details
For "ENGINE" workout (Zone 2):
- Duration: 45 minutes
- Distance: ~7.5 km (based on current pace + 10% buffer)
- Target Pace: 6:20-6:40/km (Zone 2 range)
- Target HR: 135-150 bpm (70-80% max HR)

Step 6: Generate Workout Object
{
  date: "2026-03-05",
  protocol: "ENGINE",
  zone: 2,
  type: "Aerobic Base Run",
  duration: 45,
  distance: 7.5,
  targetPace: "6:20-6:40/km",
  targetHR: "135-150 bpm",
  instructions: [
    "Warm up 5 min easy pace",
    "Run 35 min at steady Zone 2 pace (conversational)",
    "Cool down 5 min easy pace",
    "Focus on breathing and form"
  ],
  safetyNote: "Stay in Zone 1-2. Do not exceed 150 bpm heart rate."
}

Return: Generated workout object
```

---

### **Prompt 8: Create 12-Week Training Plan Generator**

**File**: `src/services/plan-builder.js`

**Copilot Prompt**:
```
Create a 12-week training plan generator using the AI workout generator.

Inputs:
1. Start Date: 05/Mar/2026
2. AISRI Score: 58
3. Training Phase: Foundation (0-500km)
4. Race Goal: 5K race in 12 weeks (target: sub-30 min)
5. Current Weekly Volume: 20 km/week
6. Recent Activity History: Last 30 days from Strava

Plan Structure:

Week 1-4 (Foundation):
- Build aerobic base
- 3-4 runs per week
- Volume: 20-25 km/week
- Protocols: START, ENGINE, STRENGTH (2x)
- Example:
  Mon: STRENGTH
  Tue: START (5km easy)
  Wed: REST
  Thu: ENGINE (8km Zone 2)
  Fri: STRENGTH
  Sat: LONG RUN (10km Zone 2)
  Sun: REST

Week 5-8 (Build):
- Increase volume gradually (10% per week)
- Introduce Zone 3 if AISRI improves to 60+
- 4 runs per week
- Volume: 28-35 km/week
- Protocols: START, ENGINE, ZONES (if unlocked), STRENGTH (2x), LONG RUN

Week 9-12 (Peak & Taper):
- Peak volume: 40 km/week (Week 10)
- Add Zone 4 intervals if AISRI 80+
- Taper last 2 weeks before race
- Protocols: All 7 protocols (based on AISRI)

Safety Gates:
- Check AISRI score weekly
- Block Zone 3+ workouts if AISRI < 60
- Add recovery week every 4th week (reduce volume 20%)
- Flag rapid volume increases

Generate:
- 84 daily workouts (12 weeks × 7 days)
- Each workout has: date, protocol, zone, duration, distance, pace, HR, instructions
- Store in daily_workouts table

Return: { 
  planId: string,
  startDate: string,
  endDate: string,
  totalWeeks: 12,
  workouts: array of 84 workout objects
}
```

---

## 📝 PHASE 5: EVALUATION FORM ENHANCEMENT

### **Prompt 9: Create Enhanced Evaluation Form**

**File**: `public/athlete-evaluation-v2.html`

**Copilot Prompt**:
```
Enhance the athlete evaluation form to include Strava data integration.

Add new section BEFORE physical tests:

"Connect Your Strava Account"
- Button: "Connect Strava" (purple, prominent)
- Show loading state while connecting
- After connection:
  ✓ Connected to Strava
  Analyzing your last 30 days of training...
  Found 12 activities, 85 km total
  
- Display summary:
  - Total runs: 12
  - Total distance: 85 km
  - Average weekly volume: 21 km
  - Longest run: 15 km
  - Most recent activity: 2 days ago

Then proceed with 6-pillar physical assessment:
1. Mobility Tests (upload videos/photos)
2. Strength Tests (single-leg squat, plank, etc.)
3. Mental Resilience Questionnaire
4. Recovery Quality Questionnaire
5. Injury History Form
6. Performance Goals

After submission:
- Calculate Running Pillar Score from Strava data
- Calculate other 5 pillar scores from form data
- Calculate final AISRI score
- Display AISRI result with animated radar chart
- Show allowed training zones
- Generate first workout recommendation

Design: Use Tailwind CSS, purple theme, responsive, step-by-step wizard
```

---

## 🗄️ PHASE 6: DATABASE SCHEMA

### **Prompt 10: Create Database Migration**

**File**: `migrations/006_strava_integration.sql`

**Copilot Prompt**:
```sql
-- Create database schema for Strava integration and workout generation

-- 1. Strava Connections
CREATE TABLE IF NOT EXISTS strava_connections (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  athlete_id TEXT NOT NULL,
  strava_athlete_id INTEGER NOT NULL UNIQUE,
  access_token TEXT NOT NULL,
  refresh_token TEXT NOT NULL,
  expires_at INTEGER NOT NULL,
  scope TEXT,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (athlete_id) REFERENCES profiles(id)
);

-- 2. Synced Activities
CREATE TABLE IF NOT EXISTS synced_activities (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  athlete_id TEXT NOT NULL,
  strava_activity_id INTEGER NOT NULL UNIQUE,
  activity_date DATE NOT NULL,
  activity_type TEXT NOT NULL,
  distance REAL,
  duration INTEGER,
  avg_pace REAL,
  avg_hr INTEGER,
  max_hr INTEGER,
  elevation_gain REAL,
  detected_zone INTEGER,
  allowed_zone INTEGER,
  is_safe BOOLEAN DEFAULT 1,
  synced_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (athlete_id) REFERENCES profiles(id)
);

-- 3. Activity Analysis
CREATE TABLE IF NOT EXISTS activity_analysis (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  athlete_id TEXT NOT NULL,
  analysis_date DATE NOT NULL,
  consistency_score REAL,
  weekly_volume REAL,
  pace_trend TEXT,
  hr_avg_zone1 INTEGER,
  hr_avg_zone2 INTEGER,
  hr_avg_zone3 INTEGER,
  hr_avg_zone4 INTEGER,
  hr_avg_zone5 INTEGER,
  training_load_atl REAL,
  training_load_ctl REAL,
  training_load_tsb REAL,
  injury_risk_flags TEXT,
  running_pillar_score REAL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (athlete_id) REFERENCES profiles(id)
);

-- 4. Generated Workouts
CREATE TABLE IF NOT EXISTS generated_workouts (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  athlete_id TEXT NOT NULL,
  plan_id TEXT,
  workout_date DATE NOT NULL,
  protocol TEXT NOT NULL,
  zone INTEGER NOT NULL,
  workout_type TEXT,
  duration INTEGER,
  distance REAL,
  target_pace TEXT,
  target_hr TEXT,
  instructions TEXT,
  safety_note TEXT,
  status TEXT DEFAULT 'pending',
  completed_at DATETIME,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (athlete_id) REFERENCES profiles(id)
);

-- 5. Training Plans (enhanced)
ALTER TABLE training_plans ADD COLUMN aisri_at_creation REAL;
ALTER TABLE training_plans ADD COLUMN strava_connected BOOLEAN DEFAULT 0;
ALTER TABLE training_plans ADD COLUMN auto_generated BOOLEAN DEFAULT 1;

-- 6. AISRI Score History (enhanced)
ALTER TABLE aisri_score_history ADD COLUMN running_pillar_score REAL;

-- 7. Indexes for performance
CREATE INDEX idx_strava_athlete ON strava_connections(athlete_id);
CREATE INDEX idx_synced_activities_athlete ON synced_activities(athlete_id);
CREATE INDEX idx_synced_activities_date ON synced_activities(activity_date);
CREATE INDEX idx_generated_workouts_athlete ON generated_workouts(athlete_id);
CREATE INDEX idx_generated_workouts_date ON generated_workouts(workout_date);

-- 8. View: Current Week Workouts
CREATE VIEW current_week_workouts AS
SELECT 
  gw.*,
  p.full_name as athlete_name,
  sa.strava_activity_id,
  sa.is_safe
FROM generated_workouts gw
JOIN profiles p ON gw.athlete_id = p.id
LEFT JOIN synced_activities sa ON gw.workout_date = sa.activity_date AND gw.athlete_id = sa.athlete_id
WHERE gw.workout_date >= date('now', 'weekday 1', '-7 days')
  AND gw.workout_date < date('now', 'weekday 1');
```

---

## 🔌 PHASE 7: API ENDPOINTS

### **Prompt 11: Create Complete API Routes**

**File**: `src/api/workout-generator.js`

**Copilot Prompt**:
```typescript
Create API endpoints for workout generation system in Hono.js.

Endpoints:

1. POST /api/athlete/connect-strava
   Body: { athleteId }
   Response: { authUrl } - Redirect athlete to Strava OAuth

2. GET /api/athlete/strava/callback?code=xxx&state=yyy
   Process OAuth callback
   Save tokens to database
   Fetch initial activities
   Redirect to dashboard

3. POST /api/athlete/sync-activities
   Body: { athleteId }
   Fetch all activities from Strava
   Save to synced_activities table
   Analyze activities
   Calculate Running Pillar Score
   Return: { activities: [], runningScore: 75 }

4. POST /api/athlete/calculate-aisri
   Body: { athleteId, evaluationData }
   Calculate all 6 pillar scores
   Calculate final AISRI score
   Save to aisri_score_history
   Return: { aisri: 58, pillars: {}, allowedZones: [1,2] }

5. POST /api/athlete/generate-workout
   Body: { athleteId, targetDate }
   Get AISRI score
   Get recent activities
   Use AI to generate appropriate workout
   Save to generated_workouts table
   Return: { workout: {...} }

6. POST /api/athlete/generate-plan
   Body: { athleteId, startDate, weeks }
   Get AISRI score
   Get training history
   Generate 12-week plan (84 workouts)
   Save all workouts
   Return: { planId, workouts: [] }

7. GET /api/athlete/today-workout
   Query: ?athleteId=xxx
   Get today's generated workout
   Check if completed (sync with Strava)
   Return: { workout: {...}, completed: boolean }

8. POST /api/athlete/complete-workout
   Body: { athleteId, workoutId, stravaActivityId }
   Mark workout as completed
   Link to Strava activity
   Check if zone was followed (safety gate)
   Send alert to coach if violated
   Return: { success: true, safetyCheck: {...} }

All endpoints use:
- Authentication middleware
- Error handling
- D1 database (Cloudflare)
- JavaScript types
```

---

## 🧪 PHASE 8: TESTING DATA

### **Prompt 12: Create Mock Data for 15 Athletes**

**File**: `migrations/007_seed_test_athletes.sql`

**Copilot Prompt**:
```sql
Create test data for 15 athletes with realistic Strava data.

For each athlete:
1. Profile (id, name, email, age, current_5k_pace)
2. Strava connection (fake tokens)
3. 30 days of activities (mix of easy runs, tempo, long runs)
4. Activity analysis (calculated metrics)
5. AISRI evaluation data
6. AISRI score (varies: 42, 55, 68, 75, 82, etc.)
7. Generated workouts for next 7 days

Example Athlete 1:
- Name: Rajesh Kumar
- AISRI: 58 (Moderate Risk)
- Last activity: 03/Mar/2026 - 8km easy run
- Next workout: 05/Mar/2026 - Tempo run (but blocked, so ENGINE instead)
- Allowed zones: 1-2
- Weekly volume: 25 km

Example Athlete 2:
- Name: Priya Singh
- AISRI: 72 (Low Risk)
- Last activity: 03/Mar/2026 - 12km long run
- Next workout: 05/Mar/2026 - Tempo run (allowed)
- Allowed zones: 1-3
- Weekly volume: 40 km

Create varied profiles:
- AISRI scores: 42, 48, 52, 58, 62, 65, 68, 72, 75, 78, 82, 85, 88, 92, 95
- Training phases: Foundation, Build, Peak
- Race goals: 5K, 10K, Half Marathon
- Activity frequency: 2-6 runs per week
- Realistic paces: 5:00/km to 7:30/km
```

---

## 🚀 DEPLOYMENT PROMPTS

### **Prompt 13: Environment Variables**

**File**: `.dev.vars` (local) and Cloudflare Dashboard (production)

**Copilot Prompt**:
```bash
# Create .dev.vars file for local development

# Strava OAuth
STRAVA_CLIENT_ID=your_strava_client_id
STRAVA_CLIENT_SECRET=your_strava_client_secret
STRAVA_REDIRECT_URI=http://localhost:3000/strava-callback

# Database
DATABASE_ID=your_d1_database_id

# OpenAI (for AI workout generation)
OPENAI_API_KEY=your_openai_api_key

# Application
APP_URL=http://localhost:3000
ENVIRONMENT=development

For production (Cloudflare):
1. Go to Cloudflare Dashboard
2. Workers & Pages → safestride → Settings → Environment Variables
3. Add all above variables (use production values)
4. Update STRAVA_REDIRECT_URI to: https://safestride.pages.dev/strava-callback
```

---

## 📱 PHASE 9: FRONTEND DASHBOARD

### **Prompt 14: Enhanced Athlete Dashboard**

**File**: `public/athlete-dashboard-v2.html`

**Copilot Prompt**:
```
Create an enhanced athlete dashboard that shows:

Header:
- Athlete name and photo
- Current AISRI score (large, colored badge)
- Strava connection status (✓ Connected or "Connect Strava" button)

Today's Workout Card (Hero):
- Date: 05/Mar/2026
- Protocol: ENGINE (Aerobic Base)
- Zone: 2
- Duration: 45 minutes
- Distance: 7.5 km
- Target Pace: 6:20-6:40/km
- Target HR: 135-150 bpm
- Instructions: [list]
- Safety Note: "Stay in Zone 1-2"
- Status: Pending | Completed
- Actions:
  - [Start Workout] button (links to Strava)
  - [View Details] button

Weekly Overview:
- Mon-Sun grid showing all workouts
- Each day: protocol icon, distance, completion status
- Color code: Green (completed), Yellow (today), Gray (upcoming)

Recent Activities (from Strava):
- Last 7 activities
- Each card: date, type, distance, time, pace, HR, zone
- Safety indicator: ✓ Safe | ⚠ Zone exceeded

AISRI Progress:
- Line chart showing AISRI score over last 6 months
- Current: 58
- Goal: 70 (unlock Zone 3)
- Next evaluation: April 1, 2026

Zone Status:
- Visual representation of 5 zones
- Green: Unlocked (Z1, Z2)
- Red: Locked (Z3, Z4, Z5)
- Show requirements: "Need AISRI 60 to unlock Zone 3"

12-Week Plan Overview:
- Weeks 1-12 calendar view
- Each week: total distance, number of workouts, key sessions
- Current week highlighted

Design: Tailwind CSS, purple theme, responsive, data loaded via API
```

---

## 📧 PHASE 10: NOTIFICATIONS

### **Prompt 15: Email Notification System**

**File**: `src/services/email-notifications.js`

**Copilot Prompt**:
```typescript
Create email notification system using Cloudflare Email Workers.

Email Types:

1. Welcome Email (after signup):
   Subject: Welcome to SafeStride!
   Body:
   - Welcome message
   - Next steps: Connect Strava, Complete evaluation
   - Link to dashboard

2. Strava Connected (after OAuth):
   Subject: Strava Connected Successfully
   Body:
   - Confirmation message
   - Activities synced: 15 activities, 100 km
   - Your Running Pillar Score: 75
   - Next: Complete evaluation form

3. AISRI Score Calculated:
   Subject: Your AISRI Score: 58 (Moderate Risk)
   Body:
   - AISRI score with interpretation
   - 6-pillar breakdown
   - Allowed training zones: 1-2
   - Next workout: Tomorrow (Tempo Run → ENGINE)

4. Daily Workout Reminder (6 AM):
   Subject: Today's Workout: ENGINE (Zone 2)
   Body:
   - Workout details
   - Instructions
   - "Start Workout" button (deep link to Strava)

5. Workout Completed:
   Subject: Great Job! Workout Completed
   Body:
   - Congratulations message
   - Workout summary from Strava
   - Safety check: ✓ Stayed in Zone 2
   - Streak: 5 days
   - Tomorrow's workout preview

6. Safety Gate Violation:
   Subject: ⚠ Training Alert: Zone Exceeded
   Body:
   - Warning message
   - Activity: You ran in Zone 4 (allowed: Zone 1-2)
   - Health risk explanation
   - Coach has been notified
   - Recovery recommendation

7. Zone Unlock Notification:
   Subject: 🎉 Congratulations! Zone 3 Unlocked
   Body:
   - AISRI improved: 58 → 68
   - New zone unlocked: Zone 3 (Tempo)
   - Updated training plan
   - New workouts available

Use:
- Cloudflare Email Workers
- HTML email templates
- Responsive design
- Track email opens/clicks
```

---

## 🎯 COMPLETE IMPLEMENTATION WORKFLOW

### **Step-by-Step Guide for VS Code Copilot**

**Day 1: Setup (2-3 hours)**
```
1. Create new branch: git checkout -b feature/workout-generator
2. Create folder structure (see "Project Structure" above)
3. Run migration: 006_strava_integration.sql
4. Use Prompts 1-3 to build Strava OAuth
5. Test OAuth flow locally
```

**Day 2: Activity Analysis (3-4 hours)**
```
6. Use Prompts 4-5 to build activity analyzer
7. Test with sample Strava data
8. Verify Running Pillar calculation
```

**Day 3: AISRI Engine (2-3 hours)**
```
9. Use Prompt 6 to build AISRI calculator
10. Integrate with existing evaluation form
11. Test with various inputs
```

**Day 4: AI Workout Generator (4-5 hours)**
```
12. Use Prompts 7-8 to build workout generator
13. Test single workout generation
14. Test 12-week plan generation
15. Verify safety gate logic
```

**Day 5: API & Frontend (4-5 hours)**
```
16. Use Prompt 11 to create API endpoints
17. Use Prompt 14 to enhance dashboard
18. Connect frontend to backend
19. Test complete flow
```

**Day 6: Testing & Seed Data (2-3 hours)**
```
20. Use Prompt 12 to create test athletes
21. Generate workouts for all 15 athletes
22. Test various AISRI levels
23. Verify zone restrictions
```

**Day 7: Deployment (2-3 hours)**
```
24. Set up Strava OAuth app (get client ID/secret)
25. Configure environment variables
26. Deploy to Cloudflare Pages
27. Test production OAuth flow
28. Monitor logs
```

---

## 🔥 QUICK START: Copy-Paste These Commands

**In VS Code Terminal**:

```bash
# 1. Create new branch
git checkout -b feature/workout-generator

# 2. Create folder structure
mkdir -p src/api src/services src/utils public/data migrations

# 3. Create placeholder files
touch src/api/strava-auth.js
touch src/api/strava-activities.js
touch src/api/workout-generator.js
touch src/services/strava-client.js
touch src/services/activity-analyzer.js
touch src/services/aisri-engine.js
touch src/services/workout-ai.js
touch src/services/plan-builder.js
touch public/strava-callback.html
touch migrations/006_strava_integration.sql

# 4. Start with first prompt
code src/api/strava-auth.js
```

**Then in the file, use Copilot**:
1. Press `Ctrl+I` (inline chat) or `Ctrl+Shift+I` (chat panel)
2. Paste Prompt 1 from above
3. Let Copilot generate the code
4. Review, test, iterate

---

## ✅ SUCCESS CRITERIA

### **You'll know it's working when**:

1. ✅ Athlete clicks "Connect Strava" → OAuth flow works
2. ✅ Athlete authorizes → Activities sync to database
3. ✅ System calculates Running Pillar Score from Strava data
4. ✅ Athlete completes evaluation → AISRI score calculated
5. ✅ System generates workout for 05/Mar/2026 based on 03/Mar/2026 data
6. ✅ Workout respects safety gates (AISRI 58 → Zone 1-2 only)
7. ✅ 12-week plan generated with 84 workouts
8. ✅ Dashboard shows today's workout
9. ✅ Athlete completes workout → Strava syncs → Status updates
10. ✅ Safety gate violation detected → Coach alert sent

---

**🚀 READY TO START?**

**Your workflow**:
1. Open VS Code
2. Open first file: `src/api/strava-auth.js`
3. Press `Ctrl+I`
4. Copy-paste **Prompt 1** above
5. Let Copilot generate the code
6. Test it
7. Move to next prompt

**Treat this like a war foot** - Build the complete system systematically, one prompt at a time! 💪

Let me know when you're ready to start, and I'll guide you through each step! 🎯
