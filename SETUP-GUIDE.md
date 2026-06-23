# Diamond Maker Website — Master Setup Guide

## 📂 Sab Files Yahan Hain — Pehle Ye Padhein

| File | Kya Hai |
|---|---|
| **`index_standalone.html`** | Pura website, ek file me (images bhi andar embedded) — turant dekhne ke liye, double-click karein |
| `index.html` + `images/` folder | Hosting (Netlify) pe daalne ke liye — dono saath upload karein |
| `admin.html` | Aapka Admin Panel — orders, products, customers manage karne ke liye |
| `cloud-functions/` | Shiprocket courier booking ka backend code |
| `SETUP-GUIDE.md` | Ye file — website live karna, domain, Razorpay |
| `FIREBASE-SETUP-GUIDE.md` | Video Reviews feature setup |
| `ADMIN-SETUP-GUIDE.md` | Admin Panel + Orders + Shiprocket courier setup |

**Sabse pehle turant website dekhni ho:** `index_standalone.html` pe double-click karein.

---

## STEP 1: Website ko FREE me LIVE karna (Netlify)

1. **https://app.netlify.com/signup** → Google se login
2. Dashboard me box dikhega: **"Drag and drop your site output folder here"**
3. Apna poora `diamond-maker-website` folder drag-drop karein (`index.html` + `images` dono saath hone chahiye)

   ⚠️ `admin.html` bhi isी folder me hai — wo bhi automatically upload ho jayega, alag se kuch nahi karna.

4. Link milega: `https://random-name.netlify.app`
5. Aapka Admin Panel yahan milega: `https://random-name.netlify.app/admin.html`

### Site naam better karna:
Netlify → **Site settings → Change site name** → jaise `diamondmaker` → `diamondmaker.netlify.app`

---

## STEP 2: Apna Domain Lena (diamondmaker.in)

1. **https://www.namecheap.com** pe `diamondmaker.in` check karein
2. Already hai → domain connect karein; nahi hai → ₹600-900/year me le lein

### Connect Karna:
1. Netlify → **Domain settings → Add a domain** → `diamondmaker.in`
2. Jo DNS records milein, unhe Namecheap/GoDaddy ke DNS settings me daal dein
3. 1-24 ghante me live ho jayega

Admin Panel access hoga: `diamondmaker.in/admin.html`

> 🔒 **Important:** `admin.html` ka link kisi ko share na karein. Ye public hai (URL jaanne wala koi bhi khol sakta hai) lekin **login ke bina andar nahi ja sakta** (Firebase Authentication se protected hai — Part 1, ADMIN-SETUP-GUIDE.md me).

---

## STEP 3: Razorpay Payment Connect Karna

1. **https://dashboard.razorpay.com** → KYC complete karein (company PAN, GST/incorporation, bank details — 2-5 din lagte hain)
2. KYC ke baad: **Settings → API Keys → Generate Key** → `Key ID` milegi
3. `index.html` me **Ctrl+F** se `rzp_test_YOUR_KEY_HERE` dhundh kar replace karein (2 jagah hai — checkout flow me)
4. Save karke Netlify pe re-upload karein

Jab tak KYC pending hai, WhatsApp order button already kaam karta hai.

---

## STEP 4: Video Reviews Feature

Customers 15-second clip upload kar sakte hain, sabko dikhता hai, like kar sakte hain. **`FIREBASE-SETUP-GUIDE.md`** follow karein (15-20 min).

---

## STEP 5: Admin Panel + Orders + Courier

Jab koi order place karta hai (payment ke saath), wo automatically **Admin Panel → Orders** me aa jata hai. Wahan se:
- Order details dekh sakte hain
- Status update kar sakte hain (Pending/Shipped/Delivered)
- Ek click me **Shiprocket se courier book** kar sakte hain
- Products ki price/stock edit kar sakte hain
- Customers ki list dekh sakte hain
- Video reviews moderate (delete) kar sakte hain

**Pura setup — `ADMIN-SETUP-GUIDE.md` follow karein** (45-60 min, ek baar karna hai). Iske bina bhi website normal kaam karegi, bas Admin Panel "Firebase setup pending" dikhayega.

---

## Sab Setup Ka Sahi Order (Recommend)

1. ✅ Pehle **Netlify pe live karein** (Step 1) — turant ho jayega
2. ✅ **Firebase project banayein** (FIREBASE-SETUP-GUIDE.md, Part 1-2) — ye Video Reviews aur Admin Panel dono ke liye common base hai
3. ✅ **Video Reviews complete karein** (FIREBASE-SETUP-GUIDE.md, baaki parts)
4. ✅ **Admin Panel + Orders setup karein** (ADMIN-SETUP-GUIDE.md, Part 1-4) — isी Firebase project me
5. ⏳ **Razorpay KYC** apply kar dein (2-5 din lagते hain, parallel me chal sakta hai)
6. ⏳ **Shiprocket + Courier booking** (ADMIN-SETUP-GUIDE.md, Part 5-7) — thoda technical hai, sabse last me karein

---

## Website Me Ab Kya Kya Hai

✅ **GLOWE Face Serum** (₹1,199), **ORIVA7 Capsules** (₹2,299), **Combo Kit** (₹3,498)
✅ **Lifestyle Gallery** — premium unboxing aur product display photos
✅ **Checkout flow** — naam, phone, address leke order Firebase me save hota hai
✅ **Video Reviews gallery** — upload, likes, "Top Review" badge
✅ **Admin Panel** (`admin.html`) — orders, products, customers, courier booking
✅ Mobile-friendly, WhatsApp button, FAQ, testimonials

---

## Update Karna Ho (Price, Text, Photo)

`index.html` aur `index_standalone.html` dono text files hain — text editor me khol kar edit karein. **Dono me same change karna na bhoolein.**

Khud karne me dikkat ho to bata dijiye.

---

## Koi Sawaal Ho To

Yahin chat me bata dijiye — agla step, error, ya design change, sab kar denge.
