# Firebase Setup Guide for 2048 Game

## Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Enter your project name (e.g., "2048-game")
4. Enable Google Analytics (optional)
5. Wait for project creation to complete

## Step 2: Configure Firebase Authentication

1. In your Firebase project, go to **Authentication** → **Sign-in method**
2. Enable **Email/Password** authentication
3. Click on **Email/Password** and toggle "Enable"
4. Save the configuration

## Step 3: Configure Firebase Realtime Database

1. Go to **Realtime Database** in the Firebase console
2. Click "Create Database"
3. Choose your location (closest to your users)
4. Start in **Test mode** (for development) or set custom rules:

```json
{
  "rules": {
    ".read": true,
    ".write": true,
    "users": {
      "$uid": {
        ".write": "$uid === auth.uid"
      }
    },
    "status": {
      "$uid": {
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

## Step 4: Get Firebase Configuration

1. Go to **Project Settings** (gear icon)
2. Scroll down to "Your apps" section
3. Click "Add app" → Web app (</> icon)
4. Register your app with a nickname
5. Copy the Firebase configuration object

## Step 5: Update Your Game Configuration

Replace the Firebase configuration in `index.html` at line ~87:

```javascript
const firebaseConfig = {
  apiKey: "your-actual-api-key",
  authDomain: "your-project-id.firebaseapp.com",
  databaseURL: "https://your-project-id-default-rtdb.firebaseio.com/",
  projectId: "your-project-id",
  storageBucket: "your-project-id.appspot.com",
  messagingSenderId: "your-sender-id",
  appId: "your-app-id"
};
```

## Features Implemented

### Real-time Data Tracking
- **Money**: Users earn money for moves and tile merges
- **Online Players**: Real-time count of currently online users
- **Registered Players**: Total count of registered users

### Authentication
- **User Login/Registration**: Email/password authentication
- **Admin Access**: Special admin account (admin@game.com / admin123)
- **Real-time Status**: Tracks user online/offline status

### Money System
- Starting money: $100 for new users
- Earn $1 per move
- Earn bonus money for tile merges (value/10)
- $10 bonus for completing a game
- $50 bonus for achieving a new high score

### Data Synchronization
- All values sync in real-time across all connected clients
- User data persists between sessions
- Admin can view real-time statistics

## Security Notes

1. **Production Rules**: Update database rules for production:
```json
{
  "rules": {
    ".read": "auth != null",
    "users": {
      "$uid": {
        ".write": "$uid === auth.uid"
      }
    },
    "status": {
      "$uid": {
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

2. **Domain Restrictions**: Add your domain to authorized domains in Authentication settings

3. **API Key Restrictions**: Restrict your API key to your domain in Google Cloud Console

## Testing

1. Open the game in your browser
2. Click "Login" and create a new account
3. Play the game and verify money increases
4. Open multiple browser tabs to test online player count
5. Login with admin account to access admin panel

The game now properly tracks and displays correct values for money, online players, and registered players with full Firebase integration!