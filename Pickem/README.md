# The Line — Setup Guide

Your app is one file: `index.html`. It's fully built — you just need to do two **free, one-time** setup steps before it can go live: create a database (Firebase) and put the file somewhere with a public link (GitHub Pages). Both take about 10 minutes combined, and you only ever do this once.

---

## Step 1 — Create your free Firebase database

This is what lets your picks/scores/leaderboard sync between everyone's phones.

1. Go to **https://console.firebase.google.com** and sign in with your Google account (mccarthy.kev20@gmail.com works fine).
2. Click **Add project**. Name it anything (e.g. "pickem-league"). You can turn off Google Analytics for this project — not needed.
3. Once the project loads, click **Build → Firestore Database** in the left sidebar.
4. Click **Create database**. Choose any location close to you, and select **Start in test mode**. Click Create.
5. Go to the **Rules** tab inside Firestore and replace the contents with this, then click **Publish**:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /pickem/{doc} {
         allow read, write: if true;
       }
     }
   }
   ```
   (This keeps things open so the app works with no login system beyond the one built into the app itself — fine for a private friend group, just don't share the Firebase project link publicly.)
6. Click the **gear icon → Project settings** (top left, next to "Project Overview").
7. Scroll to "Your apps" and click the **`</>`** (web) icon to register a new web app. Name it anything, click Register.
8. Firebase will show you a code block with a `firebaseConfig` object — six lines like `apiKey`, `authDomain`, `projectId`, etc. **Copy those six values.**

Send me those six values (or paste them into `index.html` yourself — see below) and I'll wire them in.

### If you're pasting them in yourself:
Open `index.html`, find this block near the top of the `<script>` section, and replace the `"REPLACE_ME"` values with your real ones:

```js
const FIREBASE_CONFIG = {
  apiKey: "REPLACE_ME",
  authDomain: "REPLACE_ME.firebaseapp.com",
  projectId: "REPLACE_ME",
  storageBucket: "REPLACE_ME.appspot.com",
  messagingSenderId: "REPLACE_ME",
  appId: "REPLACE_ME"
};
```

---

## Step 2 — Put it online with a free link (GitHub Pages)

1. Go to **https://github.com** and create a free account if you don't have one.
2. Click the **+** in the top right → **New repository**. Name it `pickem` (or anything), keep it **Public**, click **Create repository**.
3. On the new repo page, click **uploading an existing file**, drag in `index.html` (and `README.md` if you want), then click **Commit changes**.
4. Go to the repo's **Settings** tab → **Pages** (left sidebar).
5. Under "Build and deployment", set Source to **Deploy from a branch**, Branch to **main** / `/ (root)`, then Save.
6. Wait about a minute, refresh the page — GitHub will show you a live URL like `https://yourname.github.io/pickem/`.

That URL is what you send to your friends. Anyone who opens it can sign up and join your league.

---

## Notes / known limitations

- **Security is casual, not bank-grade.** Passwords are hashed before storage, but the database itself is openly readable/writable by design (see Step 1) — fine for a private group of friends, not for anything sensitive.
- **Live updates are real-time**, not polling — when the commissioner enters a score, everyone's screen updates within a second or two automatically (Firebase pushes the change).
- Team logos are pulled live from ESPN's public logo CDN, so they'll always look current.
- If a friend types a team name that isn't in the built-in FBS list, the app still accepts it — it just shows a colored initials badge instead of a logo.

Let me know once you've got your Firebase config values (or once you've pasted them in yourself) and I'll do a final check before you send the link out.
