# Admin Panel + Orders + Courier — Complete Setup Guide

Ye guide aapko teen cheezein set up karna sikhayegi:
1. **Admin Panel Login** — secure email/password se aap khud login kar sakein
2. **Orders aapke paas aayein** — jab customer website pe order kare, Admin Panel me dikhe
3. **Shiprocket se Automatic Courier Booking** — ek click me pickup book ho jaye

⏱️ Total time: ~45-60 minutes (one-time setup)

---

## ⚠️ Pehle Ye Samjhein

Aapne pehle **Video Reviews** ke liye Firebase setup kiya tha (FIREBASE-SETUP-GUIDE.md). **Isi Firebase project ko hum yahan bhi use karenge** — naya account nahi banana. Agar wo setup abhi nahi kiya hai, **pehle wo guide follow karein**, fir yahan aayein.

---

## PART 1: Firebase Authentication Enable Karna (Apna Login Banane Ke Liye)

1. **https://console.firebase.google.com** → apna project kholein (jo Video Reviews ke liye banaya tha)
2. Left sidebar → **Build → Authentication**
3. **Get started** dabayein
4. **Sign-in method** tab me, **Email/Password** pe click karein → **Enable** karein → **Save**
5. Ab **Users** tab pe jaayein → **Add user**
6. Apna admin email aur ek strong password daalein (ye Admin Panel me login karne ke liye use hoga)

   Example: `admin@diamondmaker.in` / ek strong password

✅ Ye aapка login ban gaya.

---

## PART 2: Admin Panel Me Firebase Config Daalna

1. `admin.html` file kholein kisi text editor me
2. **Ctrl+F** se `YOUR_API_KEY` dhundhein
3. Wahi `firebaseConfig` block daalein jo aapne `index.html` me bhi daala tha (Firebase Console → Project Settings → General → "Your apps" se milta hai)
4. Save kar dein

**Test karne ke liye:** `admin.html` ko double-click karke browser me kholein → apna email/password se login karein → Admin Panel khulna chahiye.

---

## PART 3: Firestore Security Rules Update Karna

Orders me customer ka naam, phone, address hota hai — **ye sensitive data hai**, isे properly secure karna zaroori hai (sirf aap dekh sakein, koi aur nahi).

1. Firebase Console → **Firestore Database → Rules**
2. Pura rules block ye se **replace** kar dein:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Video reviews — public read, anyone can add/like, only admin deletes
    match /videoReviews/{docId} {
      allow read: if true;
      allow create: if request.resource.data.name is string
                    && request.resource.data.name.size() < 50
                    && request.resource.data.likes == 0;
      allow update: if request.resource.data.diff(resource.data).affectedKeys().hasOnly(['likes']);
      allow delete: if request.auth != null;
    }

    // Orders — anyone can CREATE an order (checkout), but only logged-in
    // admin can READ, UPDATE, or DELETE orders. This protects customer data.
    match /orders/{docId} {
      allow create: if request.resource.data.name is string
                    && request.resource.data.phone is string
                    && request.resource.data.amount is number;
      allow read, update, delete: if request.auth != null;
    }

    // Products — anyone can read (so website shows current prices),
    // only logged-in admin can change them.
    match /products/{docId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

3. **Publish** dabayein

Ye ensure karta hai:
- Customer order place kar sakta hai (login ke bina)
- **Sirf aap** (logged-in admin) orders dekh/update kar sakte hain
- Customer data safe rehta hai

---

## PART 4: Test Karein — Ek Test Order Place Karein

1. Apni website (`index.html`) kholein, koi bhi product pe "Buy Now" dabayein
2. Form bharein (apna hi naam/number daal dein testing ke liye), Razorpay test payment complete karein
3. `admin.html` kholein, login karein → **Orders** tab me wo order dikhna chahiye

Agar dikh raha hai — **bas, system kaam kar raha hai!** Ab courier wala part karte hain.

---

## PART 5: Shiprocket Account Setup

1. **https://app.shiprocket.in/register** pe jaake free account banayein
2. Company details, bank account (COD ke liye) bhar kar verify karayein
3. Login karke **Settings → Pickup Addresses** me jaayein → apna warehouse/shop address add karein, ek nickname dein (jaise `Primary`) — ye yaad rakhein, code me use hoga

> 💡 Shiprocket par charges courier use karne par lagte hain (per-shipment), account banana free hai.

---

## PART 6: Shiprocket Ko Connect Karna (Cloud Function Deploy Karna)

Ye thoda technical step hai kyunki Shiprocket ka password secret rakhna zaroori hai — seedhe website/admin panel me nahi daal sakte (security risk). Iske liye ek chhota "bridge" function deploy karenge Firebase pe.

### 6.1 — Tools Install Karein (One-Time, Computer Pe)

Computer me **Node.js** install hona chahiye (https://nodejs.org se LTS version download kar lein agar nahi hai).

Phir terminal/command-prompt kholke:

```bash
npm install -g firebase-tools
firebase login
```

(Google account se login karein jisमे Firebase project banaya tha)

### 6.2 — Project Folder Setup

1. `cloud-functions` folder (jo aapko diya gaya hai) ko kisi jagah save karein
2. Terminal me us folder me jaayein:

```bash
cd path/to/cloud-functions
firebase init functions
```

3. Jab pucha jaye:
   - **"Use an existing project"** → apna Firebase project select karein
   - **Language**: JavaScript
   - **ESLint**: No
   - **Install dependencies now**: Yes
   - Agar pucha jaye ki existing files overwrite karni hain — **No** kahein (humare files already sahi hain)

### 6.3 — Shiprocket Credentials Securely Set Karein

Terminal me ye command chalayein (apna Shiprocket email/password daal kar):

```bash
firebase functions:config:set shiprocket.email="aapka-shiprocket-email@example.com" shiprocket.password="aapka-shiprocket-password" shiprocket.pickup_location="Primary"
```

> 🔒 Ye credentials Google ke secure servers pe store hote hain — kabhi bhi website ya admin panel ke code me nahi dikhte.

### 6.4 — Deploy Karein

```bash
firebase deploy --only functions
```

2-5 minute lagenge. End me ek URL milega jaisा:

```
✔  functions[bookShiprocketOrder]: https://us-central1-your-project.cloudfunctions.net/bookShiprocketOrder
```

**Ye URL copy kar lein.**

### 6.5 — Admin Panel Me URL Daalna

1. `admin.html` file kholein
2. **Ctrl+F** se `YOUR_CLOUD_FUNCTION_URL` dhundhein
3. Us jagah par, file ke `<script>` tag ke shuru me ye line add kar dein:

```html
<script>
  window.SHIPROCKET_FUNCTION_URL = "https://us-central1-your-project.cloudfunctions.net/bookShiprocketOrder";
</script>
```

(Isे `<script src="...firebase-app-compat.js">` line se **pehle** add karein)

4. Save kar dein

---

## PART 7: Courier Booking Test Karein

1. Admin Panel → **Orders** tab → kisi order pe **"Book Courier"** button dabayein
2. Agar sahi se connect hua hai, kuch second me tracking number mil jayega aur order status automatically **"Shipped"** ho jayega

Agar error aaye, error message screenshot kar ke bata dijiye — main fix kar dunga.

---

## Roz Ka Workflow (Setup Hone Ke Baad)

1. Customer website pe order place karta hai → payment hota hai → order **Admin Panel → Orders** me apne aap aa jata hai
2. Aap order detail check karte hain (**View** button)
3. Jab pack ho jaye, **"Book Courier"** dabate hain → Shiprocket automatically pickup schedule karta hai, tracking number milta hai
4. Status khud-ba-khud "Shipped" ho jata hai; jab deliver ho jaye, aap manually "Delivered" select kar sakte hain
5. **Dashboard** tab pe total orders, revenue, pending shipments ek nazar me dikhते hain
6. **Customers** tab me dekh sakte hain kis customer ne kitna order kiya
7. **Products** tab se price/stock update kar sakte hain
8. **Video Reviews** tab se spam/galat videos delete kar sakte hain

---

## Koi Dikkat Aaye To

- **Login nahi ho raha** → Firebase Console → Authentication → Users me check karein email/password sahi se add hua hai
- **Orders nahi dikh rahe** → Firestore Rules dobara check karein (Part 3), aur browser console (F12) me error dekhein
- **Courier booking fail ho raha** → Shiprocket credentials sahi hain ya nahi check karein, pickup location ka naam exactly match hona chahiye jo Shiprocket dashboard me set kiya tha
- Koi bhi error ho, screenshot ke saath bata dijiye, turant fix kar denge

---

## Free Limits Yaad Rakhein

- **Firebase**: Spark (free) plan me roz 50K reads + 20K writes — chhote-medium business ke liye bahut hai
- **Cloud Functions**: Free tier me 2 million invocations/month free — courier booking ke liye kabhi khatam nahi hoga
- **Shiprocket**: Account free hai, sirf actual shipment pe per-order charge lagta hai (jaisा normal courier costs)
