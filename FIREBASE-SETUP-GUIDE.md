# Video Reviews — Firebase Setup Guide

Website me ek **Video Reviews** section hai (Lifestyle Gallery ke neeche) jaha customers apna 15-second review video upload kar sakte hain, sabko dikhता hai, aur log ❤️ like kar sakte hain. Sabse zyada liked video automatically "🏆 Top Review" badge ke saath highlight hota hai.

**Important:** Isi Firebase project ko hum baad me **Admin Panel + Orders** ke liye bhi use karenge (ADMIN-SETUP-GUIDE.md). To ye setup ek baar achhi tarah karein.

---

## STEP 1: Firebase Project Banayein

1. **https://console.firebase.google.com** → Google login
2. **Add project** → naam: `diamond-maker` → Analytics disable kar sakte hain → **Continue**

---

## STEP 2: Web App Add Karein (Config Values Lene Ke Liye)

1. Project dashboard me **`</>`** (Web) icon click karein
2. App naam: `Diamond Maker Website` → **"Also set up Firebase Hosting"** check mat karein
3. **Register app**
4. Aapko `firebaseConfig` object milega:

```js
const firebaseConfig = {
  apiKey: "AIzaSyABCD1234...",
  authDomain: "diamond-maker-xxxxx.firebaseapp.com",
  projectId: "diamond-maker-xxxxx",
  storageBucket: "diamond-maker-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

5. **Poora block copy kar lein** — ye `index.html` AUR `admin.html` dono me daalna hai

---

## STEP 3: Firestore Database Enable Karein

1. **Build → Firestore Database → Create database**
2. **"Start in test mode"** → **Next**
3. Location: **asia-south1 (Mumbai)** → **Enable**

---

## STEP 4: Storage Enable Karein (Video Files Ke Liye)

1. **Build → Storage → Get started**
2. **"Start in test mode"** → Next → Location asia-south1 confirm → **Done**

---

## STEP 5: Config Values Website Me Daalein

1. `index.html` kholein → **Ctrl+F** → `YOUR_API_KEY` dhundhein → pura `firebaseConfig` block replace karein (Step 2 wali values se)
2. **`admin.html` me bhi same karein** (wahan bhi same config chahiye hoga, top me)
3. `index_standalone.html` me bhi same karein agar use kar rahe hain
4. Save karke Netlify pe re-upload karein

**Video review gallery ab live ho jayega!**

---

## STEP 6: Security Rules (Important — 30 Din Ke Andar)

Test mode 30 din me expire ho jata hai. Permanent rules ke liye **ADMIN-SETUP-GUIDE.md → Part 3** dekhein — wahan combined rules hain jo Video Reviews, Orders, aur Products teeno ko cover karti hain (yahan alag se karne ki zaroorat nahi, ek hi jagah set ho jayega).

---

## Spam/Galat Video Hatani Ho To

**Aasaan tareeka:** `admin.html` → **Video Reviews** tab → Delete button.

(Pehle Firebase Console se manually karna padta tha — ab Admin Panel se ho jata hai.)

---

## Free Limit Kitni Hai?

Firebase free tier ("Spark Plan"):
- **1GB Storage** (~200 videos agar har ek 5MB ka ho)
- **10GB/month bandwidth**
- **50K reads + 20K writes per day**

Shuruaat ke liye bahut zyada hai. Business badhने par "Blaze Plan" (pay-as-you-go) pe upgrade kar sakte hain.

---

## Koi Dikkat Aaye To

Error ka screenshot bhej dein ya kya hua describe kar dein, turant fix kar denge.
