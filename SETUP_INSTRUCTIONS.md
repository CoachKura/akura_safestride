# SafeStride VS Code Setup Instructions
**Date**: March 5, 2026  
**Project**: SafeStride Workout Generator with Strava Integration

## 📥 Step 1: Download and Extract Files

### Option A: Download from GenSpark (Recommended)
```powershell
# Download backup
# URL: https://www.genspark.ai/api/files/s/j17FDX93
# Save to: C:\Users\2160\Downloads\safestride-backup.tar.gz

# Extract using 7-Zip or WinRAR
# Right-click → Extract to folder
# Move contents to: C:\Users\2160\webapps\webapp
```

### Option B: Manual File Creation
If download doesn't work, create these key files manually in VS Code:

1. **COPILOT_PROMPTS.md** (already exists in sandbox)
2. **QUICK_START_COPILOT.md** (already exists in sandbox)

---

## 🔧 Step 2: VS Code Setup

### Open Project
```powershell
cd C:\Users\2160\webapps\webapp
code .
```

### Install Required Extensions
1. **GitHub Copilot** (microsoft.copilot)
2. **ESLint** (for JavaScript linting)
3. **Prettier** (for code formatting)
4. **Cloudflare Workers** (optional but helpful)

---

## 🚀 Step 3: Start Building with Copilot

### Day 1: Strava OAuth Integration (2-3 hours)

#### Create File Structure
```powershell
mkdir -p src/api
mkdir -p src/services
mkdir -p src/utils
mkdir -p public
mkdir -p migrations
```

#### Use Copilot Prompt 1
1. Open: `src/api/strava-auth.js` (create new file)
2. Press: **Ctrl+I** (Copilot Chat)
3. Copy-paste from `COPILOT_PROMPTS.md` → Prompt 1
4. Let Copilot generate the code
5. Review and accept

**What you'll get:**
- Complete OAuth 2.0 flow
- Token storage in Cloudflare D1
- Automatic token refresh
- Error handling

#### Files to Create Today
```
src/api/strava-auth.js         (OAuth handler)
src/services/strava-client.js  (API client)
public/strava-callback.html    (OAuth callback page)
```

#### Test It
```bash
npm run dev:sandbox
# Visit: http://localhost:3000/api/strava/auth
```

---

## 📋 Step 4: Follow the 7-Day Plan

Open `QUICK_START_COPILOT.md` in VS Code and follow:

- **Day 1**: Strava OAuth ✅ (Start today!)
- **Day 2**: Activity Analysis
- **Day 3**: AISRI Engine  
- **Day 4**: AI Workout Generator
- **Day 5**: 12-Week Plan Builder
- **Day 6**: Database Setup
- **Day 7**: Deployment

---

## 🔑 Step 5: Environment Variables

Create `.dev.vars` file:
```bash
STRAVA_CLIENT_ID=your_client_id_here
STRAVA_CLIENT_SECRET=your_secret_here
APP_URL=http://localhost:3000
```

**Get Strava API Credentials:**
1. Go to: https://www.strava.com/settings/api
2. Create new application
3. Set callback: `http://localhost:3000/strava-callback`
4. Copy Client ID and Secret

---

## 💡 Using VS Code Copilot Effectively

### Method 1: Inline Suggestions (Automatic)
- Start typing
- Copilot suggests completions
- Press **Tab** to accept
- Press **Esc** to reject

### Method 2: Chat Interface (Ctrl+I)
1. Open file
2. Press **Ctrl+I**
3. Paste prompt from `COPILOT_PROMPTS.md`
4. Copilot generates full implementation
5. Review and insert

### Method 3: Copilot Chat Panel
1. Click Copilot icon (sidebar)
2. Ask questions: "Explain this code"
3. Request refactors: "Add error handling"
4. Generate tests: "Write unit tests for this"

---

## 🎯 Today's Goal (Day 1)

**Build Strava OAuth Integration:**

✅ Create `src/api/strava-auth.js`  
✅ Create `src/services/strava-client.js`  
✅ Create `public/strava-callback.html`  
✅ Test OAuth flow  
✅ Store tokens in D1 database

**Time Estimate**: 2-3 hours

**Success Criteria:**
- Athlete clicks "Connect Strava"
- Redirects to Strava authorization
- Returns with access token
- Token saved in database
- Can fetch athlete activities

---

## 📞 Need Help?

If you encounter issues:
1. Check `COPILOT_PROMPTS.md` for detailed specs
2. Ask me questions in this chat
3. Use Copilot Chat to debug errors
4. Review `QUICK_START_COPILOT.md` for troubleshooting

---

## 📦 Current Project Structure

```
webapp/
├── public/
│   ├── index.html              ✅ Home page
│   ├── success-stories.html    ✅ Rajesh's story
│   ├── data/
│   │   ├── mock-activities.json
│   │   └── rajesh-timeline.json
│   └── strava-callback.html    ⏳ Create today
├── src/
│   ├── api/
│   │   └── strava-auth.js      ⏳ Create today
│   └── services/
│       └── strava-client.js    ⏳ Create today
├── migrations/
├── COPILOT_PROMPTS.md          ✅ Ready
├── QUICK_START_COPILOT.md      ✅ Ready
├── package.json
├── wrangler.jsonc
└── .dev.vars                    ⏳ Create today
```

---

## 🚀 Quick Start Commands

```powershell
# 1. Navigate to project
cd C:\Users\2160\webapps\webapp

# 2. Open VS Code
code .

# 3. Open files side-by-side
# Left: COPILOT_PROMPTS.md (for reference)
# Right: src/api/strava-auth.js (to create)

# 4. Press Ctrl+I and paste Prompt 1

# 5. Review generated code

# 6. Save and test
npm run dev:sandbox
```

---

## ✨ Tips for Success

1. **Read prompts carefully** - They contain all specs
2. **Review Copilot output** - Don't blindly accept
3. **Test frequently** - After each file creation
4. **Commit often** - Use Git for version control
5. **Ask questions** - I'm here to help!

---

**Ready to start?** Open VS Code and begin with Day 1! 🎉
