# 🔥 Firebase Setup Guide - Heart Sync

## Quick Setup (5 Minutes)

### Step 1: Create Firebase Project

1. Go to: https://console.firebase.google.com/
2. Click: **"Add project"** or **"Create a project"**
3. Enter project name: `heart-sync-valentine` (or any name you like)
4. Click: **Continue**
5. **Google Analytics**: You can disable it (not needed for this project)
6. Click: **Create project**
7. Wait 30 seconds for setup to complete
8. Click: **Continue**

### Step 2: Enable Realtime Database

1. In the left sidebar, click: **Build** → **Realtime Database**
2. Click: **"Create Database"** button
3. **Database location**: Choose closest to your location
   - `us-central1` (United States)
   - `europe-west1` (Belgium)
   - `asia-southeast1` (Singapore)
4. **Security rules**: Select **"Start in test mode"**
   - This allows read/write for 30 days (perfect for testing)
5. Click: **Enable**
6. Database is ready! You'll see the URL like:
   ```
   https://heart-sync-valentine-default-rtdb.firebaseio.com/
   ```

### Step 3: Configure Security Rules (Important!)

1. Click on the **"Rules"** tab (top of Realtime Database page)
2. **Delete everything** in the editor
3. **Paste this code**:

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true,
        ".indexOn": ["timestamp"],
        "users": {
          ".indexOn": ["connected", "timestamp"]
        },
        "sync": {
          "$userId": {
            ".indexOn": ["holding", "timestamp"]
          }
        }
      }
    }
  }
}
```

4. Click: **Publish** button
5. Confirm: **"Publish anyway"** if warning appears

**What these rules do:**
- ✅ Allow anyone to read/write to rooms (for testing)
- ✅ Add indexes for better performance
- ⚠️ **For production**: You should add authentication

### Step 4: Get Your Firebase Config

1. Click the **⚙️ Gear icon** (top left) → **Project settings**
2. Scroll down to **"Your apps"** section
3. Click the **</>** icon (Web platform)
4. **Register app**:
   - App nickname: `Heart Sync Web`
   - ❌ Don't check "Firebase Hosting" (we'll do that later if needed)
5. Click: **Register app**
6. You'll see a code snippet like this:

```javascript
// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "heart-sync-valentine.firebaseapp.com",
  databaseURL: "https://heart-sync-valentine-default-rtdb.firebaseio.com",
  projectId: "heart-sync-valentine",
  storageBucket: "heart-sync-valentine.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

7. **Copy this entire object** (we'll use it next)
8. Click: **Continue to console**

### Step 5: Update Your Code

You need to update **3 files** with your Firebase config.

#### Update `index.html`

1. Open `index.html` in your code editor
2. Find this section (around line 240):

```javascript
// REPLACE THIS SECTION
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDatabaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

3. **Replace with YOUR config** from Step 4
4. Save the file

#### Update `sync.html`

1. Open `sync.html`
2. Find the same Firebase config section (around line 320)
3. Replace with YOUR config
4. Save the file

#### Update `message.html`

1. Open `message.html`
2. Find the same Firebase config section (around line 340)
3. Replace with YOUR config
4. Save the file

**✅ Triple-check**: Make sure all 3 files have the EXACT same config!

### Step 6: Test Locally

You **cannot** open HTML files directly in browser (Firebase won't work).

**Option A: Python (Easiest)**
```bash
# Open terminal in your project folder
python -m http.server 8000

# Then open browser to:
# http://localhost:8000
```

**Option B: VS Code Live Server**
1. Install "Live Server" extension in VS Code
2. Right-click `index.html`
3. Select "Open with Live Server"

**Option C: Node.js**
```bash
npx serve
```

### Step 7: Test the Experience

**Testing alone (2 browsers):**

1. Open browser: `http://localhost:8000`
2. Click "Create Connection"
3. **Copy the link** (it will look like: `http://localhost:8000/?room=ABC123`)
4. Open **Incognito/Private window** (Ctrl+Shift+N or Cmd+Shift+N)
5. **Paste the link** in incognito window
6. You should see "2/2 hearts connected" in BOTH windows
7. Click "Continue to Experience" in both
8. **Important**: Open DevTools (F12) to see any errors
9. Touch and hold in BOTH windows at the same time
10. Watch the magic happen! ✨

**Testing with partner (2 devices):**

1. Make sure both devices are on the same WiFi
2. Find your computer's IP address:
   - Windows: `ipconfig` → look for IPv4
   - Mac: System Preferences → Network
   - Example: `192.168.1.100`
3. On Device 1: `http://192.168.1.100:8000` (use YOUR IP)
4. On Device 2: Use the room link from Device 1
5. Touch and hold together!

---

## 🐛 Common Issues & Fixes

### Issue 1: "Connection failed" or nothing syncing

**Cause**: Firebase config incorrect or database not enabled

**Fix**:
1. Check all 3 files have the same config
2. Verify `databaseURL` is correct
3. Make sure Realtime Database is enabled (not Firestore)
4. Check browser console (F12) for errors

### Issue 2: "Permission denied" errors

**Cause**: Security rules not published

**Fix**:
1. Go back to Firebase Console → Realtime Database → Rules
2. Make sure rules are published
3. Try these simpler rules temporarily:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

### Issue 3: Data appears in one browser but not the other

**Cause**: Browsers not syncing

**Fix**:
1. Refresh both browsers
2. Clear browser cache
3. Make sure both are using the EXACT same room code
4. Check Firebase Console → Data tab to see if data is actually saving

### Issue 4: CORS or Firebase module errors

**Cause**: Opening HTML file directly instead of using a server

**Fix**:
- You MUST use a local server (see Step 6)
- Do NOT open `file:///path/to/index.html`

### Issue 5: Touch not working on mobile

**Cause**: Browser compatibility or JavaScript errors

**Fix**:
1. Try Chrome on Android or Safari on iOS
2. Check console for errors
3. Make sure both users are fully connected (2/2 hearts)
4. Hold for the FULL 3 seconds

---

## 📊 Verify Database is Working

1. Go to Firebase Console
2. Click: **Realtime Database**
3. Click: **Data** tab
4. When you create a room, you should see:

```
heart-sync-valentine-default-rtdb
└── rooms
    └── ABC123
        ├── created: 1234567890
        ├── state: "waiting"
        └── users
            └── user_xyz
                ├── connected: true
                └── timestamp: 1234567890
```

If you see this structure, Firebase is working! 🎉

---

## 🚀 Deploy to Production

Once testing works, deploy to Firebase Hosting:

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Initialize (in your project folder)
firebase init hosting

# Configure:
# - Select your project
# - Public directory: ./ (current directory)
# - Single-page app: No
# - Don't overwrite files

# Deploy
firebase deploy --only hosting
```

Your site will be live at:
```
https://heart-sync-valentine.web.app
```

---

## 🔒 Production Security Rules

For production, use these stricter rules:

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": "!data.exists() || data.child('created').val() > (now - 86400000)",
        ".indexOn": ["created"],
        "users": {
          "$userId": {
            ".validate": "newData.hasChildren(['connected', 'timestamp'])"
          }
        },
        "sync": {
          "$userId": {
            ".validate": "newData.hasChildren(['holding', 'timestamp'])"
          }
        }
      }
    }
  }
}
```

**What this does:**
- ✅ Rooms expire after 24 hours
- ✅ Validates required fields
- ✅ Still allows real-time sync
- ✅ Prevents spam/abuse

---

## 📱 Mobile Testing Tips

**For best mobile experience:**

1. **Use HTTPS** (required for some features)
   - Deploy to Firebase Hosting or Netlify
   - Or use ngrok for local testing

2. **Test vibration:**
   - Works on iOS Safari and Android Chrome
   - Won't work on desktop

3. **Use real devices:**
   - Simulators don't support all features
   - Test on actual phones for best results

---

## ✅ Checklist

Before sharing with your Valentine:

- [ ] Firebase project created
- [ ] Realtime Database enabled
- [ ] Security rules published
- [ ] Firebase config updated in all 3 files
- [ ] Tested on local server
- [ ] Tested with 2 devices/browsers
- [ ] Customized love letter message
- [ ] Added couple photo (optional)
- [ ] Deployed to production
- [ ] Tested on mobile devices

---

## 🆘 Need Help?

**Firebase Issues:**
- Firebase Docs: https://firebase.google.com/docs/database
- Stack Overflow: https://stackoverflow.com/questions/tagged/firebase

**Code Issues:**
- Check browser console (F12) for errors
- Verify all files are in the same folder
- Make sure using a local server, not `file://`

**Testing Issues:**
- Use Chrome DevTools Network tab to see requests
- Check Firebase Console → Database → Data to see live data
- Enable Firebase debug mode in console: `localStorage.setItem('debug', 'firebase:*')`

---

**Good luck! May your hearts sync perfectly! 💕**
