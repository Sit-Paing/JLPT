# 藍 Ai — Mobile App Build Guide

This folder is a **Capacitor** project. Capacitor wraps your existing `index.html`
(in `www/`) inside a real native iOS and Android app shell. The web app code
itself is untouched — you keep updating `www/index.html` exactly like before.

## One-time setup (do this first, on any computer with Node.js)

```bash
cd aiapp
npm install
npx cap sync
```

This downloads the native Capacitor runtime into `android/` and `ios/`.

---

## Android — build an APK with NO Android Studio (GitHub Actions, recommended)

This project includes `.github/workflows/build-android.yml`, which builds
the APK for you in the cloud, for free. You never install anything.

1. Push this whole `aiapp` folder to a GitHub repo (a new one, or a
   subfolder of your existing site's repo — either works).
   ```bash
   cd aiapp
   git init
   git add .
   git commit -m "Add Capacitor mobile app"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
2. On GitHub, open your repo → **Actions** tab. You'll see "Build Android
   APK" running automatically (you can also re-trigger it manually with
   the "Run workflow" button).
3. When it finishes (green checkmark, ~2–3 minutes), click into that run →
   scroll to **Artifacts** → download `ai-jlpt-debug-apk.zip`.
4. Unzip it — inside is `app-debug.apk`. Transfer that file to any Android
   phone (email, Google Drive, USB, whatever) and tap it to install.
   You may need to allow "Install from unknown sources" the first time
   Android asks — that's normal for an APK not from the Play Store.

That's the whole process — no SDK, no Android Studio, no local build tools.
Every time you push updated code, a fresh APK is built automatically.

---

## Android — alternative: build an installable APK locally (needs Android Studio, any OS)

1. Install **Android Studio**: https://developer.android.com/studio
2. Open the `android/` folder in Android Studio as a project (it will prompt
   to download the Android SDK the first time — let it).
3. Once Gradle sync finishes, go to **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
4. The signed-for-testing APK lands in `android/app/build/outputs/apk/debug/`.
   Copy that `.apk` to your phone (email it to yourself, or use Android
   Studio's "Run" button with your phone plugged in via USB debugging) and
   install it directly — no Play Store needed for personal/family use.

**To publish on the Play Store instead** (so a public link works for anyone):
you'll need a one-time $25 Google Play Developer account, then Android
Studio's **Build → Generate Signed Bundle** to create a release `.aab`,
which you upload through the Play Console.

---

## iOS — build an installable app (needs a Mac + Xcode)

1. On a Mac, install **Xcode** from the App Store.
2. Install CocoaPods if you don't have it: `sudo gem install cocoapods`
3. In the `ios/App` folder, run: `pod install`
4. Open `ios/App/App.xcworkspace` (not `.xcodeproj`) in Xcode.
5. Sign in with your Apple ID under **Xcode → Settings → Accounts**, then
   select your team under the project's **Signing & Capabilities** tab.
   A free Apple ID lets you install on your own devices for 7 days at a
   time (re-install to refresh); a paid **Apple Developer Program**
   membership ($99/yr) removes that limit and enables TestFlight/App Store.
6. Plug in your iPhone/iPad, select it as the run target, hit **Run** (▶).
   The app installs directly on the device.

**To share with others without the App Store:** use **TestFlight**
(needs the $99/yr developer account) — upload a build from Xcode, invite
people by email, they install via the TestFlight app. Up to 10,000 testers,
no App Store review needed for internal-ish distribution, but review IS
required for public TestFlight links.

**To publish on the App Store:** same $99/yr account, submit through
App Store Connect — subject to Apple's review process.

---

## Updating the app after future edits to index.html

Every time you get a new `index.html`:

```bash
cp new-index.html www/index.html
npx cap sync
```

Then re-build in Android Studio / Xcode as above.

---

## Realistic recommendation for "everyone can use it"

- **Family/friends with Android**: send them the debug `.apk` directly — free, takes minutes, no store needed.
- **Family/friends with iPhone**: TestFlight is the easiest path — needs the $99/yr Apple Developer account, but then anyone with the link can install it like a store app.
- **True public App Store / Play Store listing**: doable with the accounts above, but Apple's review process can take a few days and has content guidelines to satisfy (fine for a study app, just budget the time).
