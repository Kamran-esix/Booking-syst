# Dr. Fazal Wajid Clinic: booking website

Public site in English, Urdu and Pashto with a live patient-number board (a number can never be booked twice), payment section, and a staff dashboard (`admin.html`) for the doctor and assistant.

| File | Purpose |
|---|---|
| `index.html` | Public website and booking |
| `admin.html` | Staff dashboard |
| `js/config.js` | **Main file to edit**: phone numbers, hours, number blocks, Firebase keys, staff emails |
| `js/i18n.js` | All English / Urdu / Pashto text |
| `js/db.js` | The only file that talks to the database |
| `firestore.rules` | Security rules that block double booking (paste into Firebase) |
| `logo.svg` | Temporary logo, easy to replace |
| `images/doctor.jpg` | Optional: add the doctor's photo with exactly this name (create the `images` folder) |

## 1. Try it first (demo mode, no Firebase)

The site uses ES modules, so **do not double-click `index.html`**. Either upload the folder to Netlify (below), or run `python3 -m http.server` inside the folder and open http://localhost:8000.

In demo mode data stays in your browser. Open the site in two tabs, book #3 in one, and watch it turn grey in the other. Staff sign-in works with the demo password `demo1234`.

## 2. Connect Firebase (about 20 minutes)

1. https://console.firebase.google.com > **Add project** (Analytics can be off).
2. **Build > Firestore Database > Create database**. Pick a nearby location (for example `asia-south1`, Mumbai). Choose **production mode**.
3. **Build > Authentication > Get started > Email/Password > Enable**. In the **Users** tab, **Add user** twice:
   - `doctor@drfazalwajid.com`
   - `assistant@drfazalwajid.com`
   
   Set the staff password **here, in Firebase**. Never write it in any file. These emails do not need to be real mailboxes. If you use other emails, change them in **both** `js/config.js` and `firestore.rules`.
4. Open `firestore.rules`, copy everything into **Firestore Database > Rules**, press **Publish**.
5. **Project settings (gear) > General > Your apps > Web (`</>`)**. Register an app and copy the values into `firebase: { ... }` in `js/config.js`. Once `apiKey` is filled in, the site is live and the yellow demo bar disappears.
6. **Authentication > Settings > Authorized domains**: add your own domain once you buy it, and the Netlify address.

Create the two staff users **before** publishing the site.

## 3. Test before showing the client

1. Open the site on two phones. Book #3 on phone A. It should grey out on phone B within a second.
2. Try #3 on phone B: it must refuse.
3. Sign in on `admin.html`, check the booking is listed, mark it done, cancel it, and confirm the number is free again on the public page.

## 4. Publish

Drag the whole folder onto https://app.netlify.com/drop, then add your domain under *Domain management*.

## Check with the doctor before going live

- **WhatsApp number**: first given as `0346005541` (10 digits), later as `03460065541`. The site uses the 11-digit one. Confirm.
- **Easypaisa fee amount and account name**: not given, so the page says "ask the clinic for the fee".
- **Services list** (`js/i18n.js`): written as typical orthopaedic care. The doctor should approve it.
- **Urdu and Pashto text**: please have a native reader check the wording.
- **Photo** of the doctor, and the logo if he wants another one.
- **Testimonials**: none shown. Add only real, permitted quotes.

## Good to know

- Open days, hours and number blocks are in `js/config.js`. If you change `maxPatients`, also change `50` in `firestore.rules`.
- The rules block double booking, closed days and invalid data. They cannot check the weekday, so Saturday/Sunday is enforced by the website only.
- Patient names and locations can be read only by the staff emails listed in the rules.
- Firebase config keys are not secrets. The rules are what protect the data.
- The free Firebase plan is enough for 50 bookings a day.
