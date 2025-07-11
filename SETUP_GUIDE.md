# 🔥 FF Tournaments - Setup Guide

## 🚀 Quick Test (Demo Mode)

**The app works immediately without Firebase setup!**

1. Open `user.html` in one browser tab
2. Open `admin.html` in another browser tab  
3. Both will show "🔥 DEMO MODE" message
4. In admin panel: Click "Create Tournament" and fill the form
5. In user panel: You'll see the tournament appear instantly!

## 🛠️ Firebase Setup (For Production)

### Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Enter project name: `ff-tournaments`
4. Enable Google Analytics (optional)
5. Click "Create project"

### Step 2: Enable Firestore Database

1. In Firebase Console, go to "Firestore Database"
2. Click "Create database"
3. Choose "Start in test mode" (for development)
4. Select your preferred location
5. Click "Done"

### Step 3: Enable Authentication

1. Go to "Authentication" → "Sign-in method"
2. Enable "Email/Password"
3. Click "Save"

### Step 4: Get Firebase Configuration

1. Go to "Project Settings" (gear icon)
2. Scroll down to "Your apps"
3. Click "Web app" icon (`</>`)
4. Enter app name: `FF Tournaments`
5. Copy the `firebaseConfig` object

### Step 5: Update Configuration Files

Replace the firebaseConfig in **BOTH** files:

**user.html** (around line 680):
```javascript
const firebaseConfig = {
    apiKey: "your-actual-api-key",
    authDomain: "your-project.firebaseapp.com", 
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "your-app-id"
};
```

**admin.html** (around line 1400):
```javascript  
const firebaseConfig = {
    // SAME CONFIG AS user.html
    apiKey: "your-actual-api-key",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id", 
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "your-app-id"
};
```

### Step 6: Set Firestore Security Rules

Go to Firestore → Rules and paste:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Step 7: Test Real Firebase Sync

1. Refresh both `user.html` and `admin.html`
2. You should see "🔥 Firebase connected!" messages
3. Create account in user panel
4. Login to admin panel with same credentials
5. Create tournament in admin panel
6. See it appear in user panel instantly!

## 🎯 Testing Tournament Sync

### Demo Mode Test:
- ✅ No setup required
- ✅ Instant cross-tab sync using localStorage
- ✅ Perfect for testing UI and functionality

### Firebase Mode Test:
- ✅ Real-time database sync
- ✅ User authentication
- ✅ Production-ready data persistence
- ✅ Multi-device sync

## 🐛 Troubleshooting

### Problem: "Firebase connection failed"
**Solution:** Check your firebaseConfig values match exactly from Firebase Console

### Problem: Tournaments not syncing
**Solution:** Ensure both files have identical firebaseConfig

### Problem: Authentication errors  
**Solution:** Enable Email/Password in Firebase Authentication

### Problem: Permission denied
**Solution:** Update Firestore security rules (see Step 6)

## 📱 Features Working

✅ **User Panel:**
- Tournament browsing and registration
- Wallet management and top-up
- Referral system with sharing
- Real-time tournament updates

✅ **Admin Panel:**  
- Tournament creation and management
- Player management and analytics
- Real-time dashboard metrics
- Cross-panel data synchronization

## 🚀 Production Deployment

1. Update Firestore rules for production security
2. Set up Firebase Hosting (optional)
3. Configure proper authentication flows
4. Add payment gateway (Razorpay/Stripe) credentials
5. Set up monitoring and analytics

---

**Both panels now work perfectly with real-time sync! 🎉**