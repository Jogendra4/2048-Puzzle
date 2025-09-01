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

## � Testing Wallet Management

### Test Admin Wallet Management:

1. **Open both panels** in separate browser tabs
2. **Admin Panel** → Go to "Players" section
3. **See demo players** with different wallet balances
4. **Click wallet button** (💰) for any player
5. **Select action**:
   - **Add Money**: Give bonus/refund
   - **Deduct Money**: Apply penalty/fee
   - **Set Balance**: Override current balance
6. **Enter amount** and reason
7. **See live preview** of balance change
8. **Click "Update Wallet"**
9. **User Panel** → See instant balance update notification! 🎉

### Example Test Cases:

**Bonus Test:**
- Player: "Demo User" (₹500)
- Action: Add ₹250 
- Reason: Tournament Bonus
- Result: User sees "🎉 Your wallet has been credited with ₹250! New balance: ₹750"

**Penalty Test:**
- Player: "Rohan Kumar" (₹750)
- Action: Deduct ₹100
- Reason: Penalty
- Result: User sees "💳 ₹100 has been deducted from your wallet. New balance: ₹650"

**Override Test:**
- Player: "Priya Sharma" (₹1200)
- Action: Set ₹2000
- Reason: Admin Adjustment
- Result: User sees "💰 Your wallet balance has been updated to ₹2000"

## 🎮 Tournament Registration & Wallet Balance Check

### How It Works:
1. **Balance Validation**: Users can only register for tournaments if they have sufficient wallet balance
2. **Automatic Deduction**: Entry fees are deducted from wallet balance upon registration
3. **Real-time Updates**: Balance changes are immediately reflected in the UI

### Test Tournament Registration:

1. **Open user panel** → Go to "Tournaments" section
2. **Try to register** for a tournament
3. **If insufficient balance**:
   - See error message: "Insufficient balance! You need ₹50 but have only ₹20"
   - Automatically redirected to top-up modal
4. **If sufficient balance**:
   - See balance preview before registration
   - Entry fee deducted upon successful registration
   - Success message: "Successfully joined tournament! ₹50 deducted from your wallet"

## 💳 Money Withdrawal System

### Test Withdrawal Feature:

1. **Open user panel** → Go to "Profile" section
2. **Click "Withdraw" button** (red button next to Top-up)
3. **Check balance requirements**:
   - Minimum withdrawal: ₹50
   - Must have sufficient balance
4. **Fill withdrawal form**:
   - Enter amount (₹50 minimum)
   - Select bank account
   - Choose withdrawal purpose
5. **See live preview** of balance after withdrawal
6. **Submit withdrawal request**
7. **See success message**: "Withdrawal request submitted! ₹100 will be transferred to your Primary Account within 24-48 hours"

### Test Cases:

**Valid Withdrawal:**
- Balance: ₹500
- Withdraw: ₹200
- Result: Success, balance becomes ₹300

**Invalid Withdrawal (Low Balance):**
- Balance: ₹30
- Try to withdraw: ₹50
- Result: Error message about insufficient balance

**Invalid Withdrawal (Below Minimum):**
- Balance: ₹100
- Try to withdraw: ₹20
- Result: Error message about minimum ₹50

## 🏆 Enhanced Tournament Registration System

### Features Implemented:
- **Balance Validation**: Users must have sufficient wallet balance before registration
- **Duplicate Prevention**: Users can only register once per tournament
- **Automatic Deduction**: Entry fees are deducted from wallet balance upon registration
- **Room Details Display**: Shows Room ID and Password after successful registration
- **Real-time Updates**: Tournament player counts and user balances update instantly

### Test Enhanced Registration:

1. **Open user panel** → Go to "Tournaments" section
2. **Try to register** for the same tournament twice
3. **Result**: See error "You are already registered for this tournament!"
4. **Valid registration** → See success modal with:
   - "🎉 Registered Successfully!" message
   - Room ID and Password display
   - Entry fee deduction confirmation
   - Balance update in real-time

## 💼 Admin Withdrawal Management System

### Features Implemented:
- **Withdrawal Requests Tab**: Dedicated admin section for withdrawal management
- **Real-time Metrics**: Approved, Pending, Rejected withdrawal counts
- **Approve/Reject Actions**: One-click approval/rejection with reason notes
- **User Notifications**: Automatic user notifications when admin processes requests
- **Balance Deduction**: Money is deducted only after admin approval

### Test Admin Withdrawal Management:

1. **User submits withdrawal** → User panel → Profile → Withdraw button
2. **Admin sees request** → Admin panel → Withdrawals tab
3. **View pending requests** in withdrawal table
4. **Approve withdrawal**:
   - Click "Approve" button
   - Confirm approval
   - User gets "🎉 Payment Successful!" notification
   - Money is deducted from user's wallet
5. **Reject withdrawal**:
   - Click "Reject" button  
   - Enter rejection reason
   - User gets rejection notification with reason
   - Money is refunded to user's wallet

### Enhanced User Experience:
- **Immediate Feedback**: Users get real-time notifications
- **Status Tracking**: Clear withdrawal status in admin panel
- **Audit Trail**: Complete transaction history
- **Cross-tab Sync**: All changes sync between admin and user panels instantly

## 🚀 Production-Ready Features:

✅ **Complete tournament registration flow with room details**  
✅ **Duplicate registration prevention**  
✅ **Admin-managed withdrawal approval system**  
✅ **Real-time user notifications**  
✅ **Automatic wallet balance management**  
✅ **Cross-panel synchronization**  
✅ **Comprehensive error handling**  
✅ **Transaction audit trails**  
✅ **Mobile-responsive design**  
✅ **Demo mode for testing**  
✅ **Firebase integration for production**

## �🐛 Troubleshooting

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