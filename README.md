# 💕 Heart Sync - Valentine's Day Experience

A beautiful, interactive real-time web experience where two hearts connect across different devices to share a romantic Valentine's Day moment.

![Heart Sync Preview](preview.png)

## ✨ Features

- 🎭 **Real-time Connection**: Two users connect via unique room codes
- 💓 **Synchronized Touch**: Both users must touch and hold together for 3 seconds
- 🎨 **Beautiful Animations**: GSAP-powered smooth animations with particle effects
- 📱 **Mobile-First**: Optimized for touch devices with haptic feedback
- 🌟 **Valentine's Theme**: Romantic pink/purple gradient design with floating hearts
- 💌 **Love Letter**: Customizable romantic message reveal
- 🔄 **Firebase Realtime**: Instant synchronization using Firebase Realtime Database

## 🎯 User Flow

1. **Landing Page** (`index.html`)
   - User creates a new connection or joins via shared link
   - Auto-generates 6-character room code
   - Shows connection status (1/2 or 2/2 hearts)

2. **Sync Page** (`sync.html`)
   - Both users see "Touch & Hold" instruction
   - Real-time sync when both press simultaneously
   - 3-second countdown with progress ring
   - Animated orbs merge into a beating heart
   - Particle burst and haptic feedback

3. **Message Page** (`message.html`)
   - Envelope animation with wax seal
   - Customizable romantic letter
   - Option to add couple photo
   - Download/share functionality

## 🚀 Quick Start

### Prerequisites

- A Firebase account (free tier works)
- A web server (or use Firebase Hosting)
- Modern web browser

### Setup Instructions

#### 1. Clone/Download Files

```bash
# Your project structure should be:
heart-sync/
├── index.html
├── sync.html
├── message.html
└── README.md
```

#### 2. Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add Project"
3. Enter project name: `heart-sync-valentine`
4. Disable Google Analytics (optional)
5. Click "Create Project"

#### 3. Enable Realtime Database

1. In Firebase Console, go to **Build** → **Realtime Database**
2. Click "Create Database"
3. Choose location (nearest to your users)
4. Start in **Test Mode** (for development)
5. Click "Enable"

#### 4. Configure Security Rules

In Realtime Database → Rules tab, paste this:

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true,
        "users": {
          ".indexOn": ["connected"]
        },
        "sync": {
          ".indexOn": ["holding"]
        }
      }
    }
  }
}
```

Click **Publish** to save.

> ⚠️ **Production Note**: These rules are open for testing. For production, implement proper authentication and restrict write access.

#### 5. Get Firebase Config

1. In Firebase Console, click the gear icon → **Project Settings**
2. Scroll to "Your apps" section
3. Click the **</>** (Web) icon
4. Register app name: `Heart Sync Web`
5. Copy the `firebaseConfig` object

It will look like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "heart-sync-valentine.firebaseapp.com",
  databaseURL: "https://heart-sync-valentine-default-rtdb.firebaseio.com",
  projectId: "heart-sync-valentine",
  storageBucket: "heart-sync-valentine.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

#### 6. Update Firebase Config in Code

Replace the placeholder config in **ALL THREE FILES**:

**In `index.html`** (around line 240):
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",  // ← Replace these
    authDatabaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

**In `sync.html`** (around line 320):
```javascript
// Same config here
```

**In `message.html`** (around line 340):
```javascript
// Same config here
```

> 💡 **Tip**: Make sure to use the EXACT config from your Firebase project!

#### 7. Test Locally

You can't open HTML files directly (Firebase won't work). Use a local server:

**Option A: Python**
```bash
# Python 3
python -m http.server 8000

# Then open: http://localhost:8000
```

**Option B: VS Code Live Server**
1. Install "Live Server" extension
2. Right-click `index.html` → "Open with Live Server"

**Option C: Node.js**
```bash
npx serve
```

#### 8. Test the Experience

1. Open `http://localhost:8000` in browser
2. Click "Create Connection"
3. Copy the link
4. Open the link in a **different browser/device** (incognito mode works)
5. Both devices should show "2/2 hearts connected"
6. Click "Continue to Experience"
7. Both users touch and hold the screen together
8. Watch the magic! ✨

## 🎨 Customization Guide

### Change the Love Letter Message

Edit `message.html`, find this section (around line 100):

```html
<p class="text-lg leading-relaxed">
    In this beautiful moment, as our hearts beat in perfect sync...
</p>
```

Replace with your own romantic message!

### Add a Couple Photo

In `message.html`, find this commented section (around line 430):

```javascript
// Uncomment and add your photo URL
const photoSection = document.getElementById('photoSection');
const couplePhoto = document.getElementById('couplePhoto');
couplePhoto.src = 'YOUR_PHOTO_URL_HERE.jpg';
photoSection.classList.remove('hidden');
```

Uncomment and replace with your image URL or local path.

### Change Color Scheme

The theme uses:
- Primary Pink: `#ff006e`
- Secondary Purple: `#8338ec`
- Accent Blue: `#3a86ff`

Search and replace these hex codes in all files to customize colors.

### Modify Sync Duration

In `sync.html`, find (around line 280):

```javascript
const SYNC_DURATION = 3000; // 3 seconds
```

Change to any duration (in milliseconds).

## 🌐 Deployment

### Firebase Hosting (Recommended)

1. Install Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Login:
```bash
firebase login
```

3. Initialize:
```bash
firebase init hosting
```

4. Configure:
   - Select your Firebase project
   - Public directory: `./` (current directory)
   - Single-page app: No
   - Don't overwrite existing files

5. Deploy:
```bash
firebase deploy --only hosting
```

6. Your site will be live at:
```
https://heart-sync-valentine.web.app
```

### Alternative: Netlify

1. Go to [Netlify](https://netlify.com)
2. Drag and drop your folder
3. Done! You'll get a URL like `https://romantic-heart-sync.netlify.app`

### Alternative: Vercel

```bash
npx vercel
```

## 🛠️ Tech Stack

- **Frontend**: HTML5, Tailwind CSS, Vanilla JavaScript
- **Animations**: GSAP 3.12.5
- **Backend**: Firebase Realtime Database
- **Fonts**: Google Fonts (Playfair Display, Poppins, Dancing Script)
- **Icons**: SVG (custom heart shapes)

## 📱 Browser Support

- ✅ Chrome/Edge (latest)
- ✅ Safari (iOS 14+)
- ✅ Firefox (latest)
- ✅ Mobile browsers (Chrome, Safari)
- ⚠️ Haptic feedback: iOS Safari, Android Chrome only

## 🔧 Troubleshooting

### "Connection failed" or data not syncing

1. Check Firebase config is correct in all 3 files
2. Verify Realtime Database is enabled
3. Check Security Rules are published
4. Open browser console for errors

### "Room not found"

1. Make sure both users use the EXACT same room link
2. Clear browser cache
3. Try creating a new room

### Animations not working

1. Ensure GSAP CDN is loading (check console)
2. Try different browser
3. Disable browser extensions (ad blockers)

### Touch not registering on mobile

1. Make sure you're holding for full 3 seconds
2. Try refreshing the page
3. Check if both users are actually connected (shows 2/2)

## 🎁 Features to Add (DIY)

Want to enhance this project? Here are ideas:

- [ ] Add background music toggle
- [ ] Multiple language support
- [ ] Custom countdown timer
- [ ] Photo upload from device
- [ ] Voice message recording
- [ ] Multiple message templates
- [ ] Save connection history
- [ ] QR code for easy sharing
- [ ] Dark/light mode toggle

## 📄 License

This project is open source and available for personal use. Feel free to customize for your Valentine! 💕

## 🤝 Contributing

Want to improve this? Ideas:

1. Better animations
2. Sound effects
3. More interactive features
4. Accessibility improvements
5. Performance optimizations

## ❤️ Credits

Created with love for Valentine's Day 2026

**Made by**: Claude (Anthropic)  
**For**: Couples celebrating love  
**Theme**: Valentine's Day Romance

---

## 💌 Final Notes

This is meant to be a special, intimate experience between two people. Take time to:

1. **Customize the message** - Make it personal!
2. **Add your photo** - Memories matter
3. **Test it first** - Make sure everything works
4. **Share with love** - Timing is everything

Happy Valentine's Day! May your hearts always beat in sync. 💕

---

## 📞 Support

Need help? Check:
- Firebase documentation: https://firebase.google.com/docs
- GSAP documentation: https://greensock.com/docs
- Tailwind CSS: https://tailwindcss.com/docs

---

**Version**: 1.0.0  
**Last Updated**: February 2026  
**Status**: Ready for Valentine's Day! 🌹
