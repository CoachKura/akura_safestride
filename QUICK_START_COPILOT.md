# 🚀 QUICK START - BUILD SAFESTRIDE WORKOUT GENERATOR IN VS CODE

**Date**: March 5, 2026  
**Goal**: Build complete workout generator with Strava integration using VS Code Copilot  
**Timeline**: 7 days (3-4 hours per day)  

---

## 📥 STEP 1: DOWNLOAD & SETUP (5 minutes)

### **Download Project**:
```
URL: https://www.genspark.ai/api/files/s/koMK1g7H
Save to: C:\users\2160\Downloads\
Extract to: C:\users\2160\webapps\webapp
```

### **Open in VS Code**:
```powershell
cd C:\users\2160\webapps\webapp
code .
```

---

## 📖 STEP 2: OPEN THE PROMPTS FILE (1 minute)

```powershell
# In VS Code, open:
COPILOT_PROMPTS.md
```

**This file contains 15 detailed prompts for building the complete system.**

---

## 🏗️ STEP 3: CREATE PROJECT STRUCTURE (2 minutes)

**Run in VS Code Terminal** (`Ctrl+`):

```bash
# Create folders
mkdir -p src/api src/services src/utils public/data migrations

# Create files
touch src/api/strava-auth.ts
touch src/api/strava-activities.ts
touch src/api/workout-generator.ts
touch src/services/strava-client.ts
touch src/services/activity-analyzer.ts
touch src/services/aisri-engine.ts
touch src/services/workout-ai.ts
touch src/services/plan-builder.ts
touch public/strava-callback.html
touch migrations/006_strava_integration.sql
```

---

## 🤖 STEP 4: USE COPILOT TO BUILD (Main Work)

### **How to Use Copilot**:

1. **Open a file** (e.g., `src/api/strava-auth.ts`)
2. **Press `Ctrl+I`** (inline chat) or **`Ctrl+Shift+I`** (chat panel)
3. **Copy-paste the prompt** from `COPILOT_PROMPTS.md`
4. **Wait for Copilot** to generate code
5. **Review & Test** the generated code
6. **Move to next file**

---

### **Day 1: Strava OAuth (Prompts 1-3)**

**File 1**: `src/api/strava-auth.ts`
- Open file
- Press `Ctrl+I`
- Copy-paste **Prompt 1** from `COPILOT_PROMPTS.md`
- Copilot generates OAuth handler
- Test locally

**File 2**: `src/services/strava-client.ts`
- Open file
- Press `Ctrl+I`
- Copy-paste **Prompt 2**
- Copilot generates Strava API client
- Test API calls

**File 3**: `public/strava-callback.html`
- Open file
- Press `Ctrl+I`
- Copy-paste **Prompt 3**
- Copilot generates OAuth callback page
- Test OAuth flow

**Test Day 1**:
```bash
# Start local server
npm run dev:sandbox

# Visit: http://localhost:3000/
# Click "Connect Strava"
# Complete OAuth flow
# Verify activities sync
```

---

### **Day 2: Activity Analysis (Prompts 4-5)**

**File 4**: `src/services/activity-analyzer.ts`
- Copy-paste **Prompt 4**
- Copilot generates activity analyzer
- Test with sample data

**File 5**: `src/services/running-pillar.ts`
- Copy-paste **Prompt 5**
- Copilot generates Running Pillar calculator
- Verify score calculation

**Test Day 2**:
```bash
# Create test activity data
# Run analyzer
# Verify Running Pillar Score: ~75
```

---

### **Day 3: AISRI Engine (Prompt 6)**

**File 6**: `src/services/aisri-engine.ts`
- Copy-paste **Prompt 6**
- Copilot generates AISRI calculator
- Test with evaluation form data

**Test Day 3**:
```bash
# Submit evaluation form
# Verify AISRI score: 58
# Check allowed zones: [1, 2]
```

---

### **Day 4: AI Workout Generator (Prompts 7-8)**

**File 7**: `src/services/workout-ai.ts`
- Copy-paste **Prompt 7**
- Copilot generates AI workout generator
- Test single workout generation

**File 8**: `src/services/plan-builder.ts`
- Copy-paste **Prompt 8**
- Copilot generates 12-week plan builder
- Test plan generation

**Test Day 4**:
```bash
# Generate workout for 05/Mar/2026
# Verify: ENGINE protocol (Zone 2)
# Generate 12-week plan
# Verify: 84 workouts created
```

---

### **Day 5: API & Frontend (Prompts 11, 14)**

**File 11**: `src/api/workout-generator.ts`
- Copy-paste **Prompt 11**
- Copilot generates all API endpoints

**File 14**: `public/athlete-dashboard-v2.html`
- Copy-paste **Prompt 14**
- Copilot generates enhanced dashboard

**Test Day 5**:
```bash
# Test API endpoints with Postman
# Open dashboard
# Verify today's workout displays
# Test workout completion flow
```

---

### **Day 6: Database & Seed Data (Prompts 10, 12)**

**File 10**: `migrations/006_strava_integration.sql`
- Copy-paste **Prompt 10**
- Run migration

**File 12**: `migrations/007_seed_test_athletes.sql`
- Copy-paste **Prompt 12**
- Seed 15 test athletes

**Test Day 6**:
```bash
# Run migrations
npx wrangler d1 migrations apply safestride-production --local

# Verify 15 athletes created
# Check each has workouts for next 7 days
```

---

### **Day 7: Deployment (Prompt 13)**

**Setup Environment Variables**:
- Copy-paste **Prompt 13**
- Create `.dev.vars` file
- Add Strava client ID/secret

**Deploy**:
```bash
# Build
npm run build

# Deploy to Cloudflare Pages
npx wrangler pages deploy dist --project-name safestride

# Configure production env vars in Cloudflare Dashboard
```

---

## 🎯 WORKFLOW FOR EACH PROMPT

**Repeat this for all 15 prompts**:

```
1. Open file (e.g., src/api/strava-auth.ts)
   ↓
2. Press Ctrl+I (inline Copilot chat)
   ↓
3. Copy prompt from COPILOT_PROMPTS.md
   ↓
4. Paste into Copilot chat
   ↓
5. Wait for Copilot to generate code (10-30 seconds)
   ↓
6. Review generated code
   ↓
7. Press "Accept" to insert code
   ↓
8. Test the code locally
   ↓
9. Fix any errors (ask Copilot for help)
   ↓
10. Commit changes
   ↓
11. Move to next file/prompt
```

---

## 🧪 TESTING CHECKLIST

### **After Each Day**:

**Day 1 ✅**:
- [ ] Strava OAuth works
- [ ] Activities sync to database
- [ ] Callback page displays success

**Day 2 ✅**:
- [ ] Activity analyzer calculates metrics
- [ ] Running Pillar Score calculated
- [ ] Score: ~75 for consistent athlete

**Day 3 ✅**:
- [ ] AISRI calculator works
- [ ] 6-pillar scores calculated
- [ ] Final AISRI: 58 (example)
- [ ] Allowed zones: [1, 2]

**Day 4 ✅**:
- [ ] Single workout generated correctly
- [ ] Workout respects safety gates
- [ ] 12-week plan created (84 workouts)
- [ ] Weekly structure correct

**Day 5 ✅**:
- [ ] All API endpoints work
- [ ] Dashboard displays today's workout
- [ ] Workout completion updates status
- [ ] Strava sync works

**Day 6 ✅**:
- [ ] Database migrations run successfully
- [ ] 15 test athletes created
- [ ] Each has workouts for next 7 days
- [ ] AISRI scores vary (42-95)

**Day 7 ✅**:
- [ ] Environment variables configured
- [ ] Production deployment successful
- [ ] OAuth works in production
- [ ] End-to-end flow works

---

## 💡 COPILOT TIPS

### **Best Practices**:

1. **Be Specific**: The more detailed the prompt, the better Copilot's output
2. **Review Code**: Always review generated code before accepting
3. **Test Incrementally**: Test each function as you build it
4. **Ask Follow-ups**: If code doesn't work, ask Copilot: "Fix this error: [error message]"
5. **Use Context**: Keep relevant files open so Copilot has context

### **Common Copilot Commands**:

```
Ctrl+I           - Inline chat (generate code in current file)
Ctrl+Shift+I     - Chat panel (ask questions)
Tab              - Accept suggestion
Esc              - Dismiss suggestion
Alt+]            - Next suggestion
Alt+[            - Previous suggestion
```

### **If Copilot Generates Wrong Code**:

```
Press Ctrl+I again
Type: "Fix this code: [explain issue]"
OR
Type: "Rewrite this function to [requirements]"
OR
Type: "Explain why this code isn't working"
```

---

## 🚨 TROUBLESHOOTING

### **Issue: Copilot Not Generating Code**

**Solution**:
1. Check Copilot subscription is active
2. Verify internet connection
3. Try shorter, simpler prompt first
4. Restart VS Code

### **Issue: Generated Code Has Errors**

**Solution**:
1. Read error message
2. Ask Copilot: "Fix this error: [error]"
3. Check JavaScript types
4. Verify imports are correct

### **Issue: Code Works Locally But Fails in Production**

**Solution**:
1. Check environment variables
2. Verify Cloudflare Workers compatibility
3. Check console logs in Cloudflare Dashboard
4. Test API endpoints with curl

---

## 📦 DELIVERABLES AFTER 7 DAYS

### **What You'll Have**:

1. ✅ **Strava OAuth Integration** - Athletes can connect Strava
2. ✅ **Activity Sync** - Automatically fetch and analyze workouts
3. ✅ **Running Pillar Calculator** - Score based on training consistency
4. ✅ **AISRI Engine** - Complete 6-pillar scoring system
5. ✅ **AI Workout Generator** - Generate workouts based on AISRI + past data
6. ✅ **12-Week Plan Generator** - Auto-generate personalized training plans
7. ✅ **Safety Gates** - Enforce zone restrictions based on AISRI
8. ✅ **Enhanced Dashboard** - Display today's workout + weekly overview
9. ✅ **API Endpoints** - Complete REST API for all features
10. ✅ **Database Schema** - All tables for Strava, workouts, plans
11. ✅ **15 Test Athletes** - Realistic data for testing
12. ✅ **Production Deployment** - Live on Cloudflare Pages

---

## 📞 NEED HELP?

### **During Development**:

**If stuck, come back to this GenSpark AI chat and ask**:
- "Copilot generated this code but it's not working: [paste code]"
- "How do I test Strava OAuth locally?"
- "This API endpoint returns error 500, help me debug"
- "I completed Day 3, what's next?"

**I'll help you**:
- Debug errors
- Explain Copilot's generated code
- Guide you to next step
- Provide alternative approaches

---

## 🎯 YOUR STARTING COMMAND

**Copy-paste this into VS Code terminal**:

```bash
# Navigate to project
cd C:\users\2160\webapps\webapp

# Open in VS Code
code .

# Open terminal in VS Code (Ctrl+`)
# Create structure
mkdir -p src/api src/services src/utils public/data migrations

# Create files
touch src/api/strava-auth.ts
touch src/services/strava-client.ts
touch src/services/activity-analyzer.ts
touch src/services/aisri-engine.ts
touch src/services/workout-ai.ts
touch src/services/plan-builder.ts
touch public/strava-callback.html
touch migrations/006_strava_integration.sql

# Open first file
code src/api/strava-auth.ts

# Now open COPILOT_PROMPTS.md
code COPILOT_PROMPTS.md
```

**Then**:
1. Split VS Code screen: View → Editor Layout → Split Right
2. Left side: `COPILOT_PROMPTS.md` (reference)
3. Right side: `src/api/strava-auth.ts` (coding)
4. Press `Ctrl+I` in right panel
5. Copy **Prompt 1** from left panel
6. Paste into Copilot chat
7. Magic happens! ✨

---

## ✅ SUCCESS CRITERIA

**You'll know you're done when**:

1. ✅ Athlete visits SafeStride
2. ✅ Clicks "Connect Strava"
3. ✅ Authorizes SafeStride
4. ✅ Activities sync (e.g., 03/Mar/2026: 8km easy run)
5. ✅ Completes evaluation form
6. ✅ AISRI calculated: 58 (Moderate Risk)
7. ✅ System generates workout for 05/Mar/2026: ENGINE (Zone 2, 45 min)
8. ✅ Dashboard shows workout details
9. ✅ Athlete completes workout → Status updates
10. ✅ System generates 12-week plan (84 workouts)

---

**🚀 READY TO START?**

**Reply with**: "Starting Day 1!" and I'll guide you through Strava OAuth setup! 💪

**Or**: "I have a question about [topic]" and I'll clarify before you start!

**Timeline**: 7 days × 3-4 hours = Complete workout generator system! 🎯
