# OrbitsCoder

OrbitsCoder is a local, pass-and-play social deduction game for 8–16 players. It is built with Vue 3, TypeScript, Pinia, and Capacitor. Gameplay runs on the device and does not require an account or game server.

## What is included

- A role-balancing and game engine for 15 roles, night actions, voting, and win conditions.
- Player setup, pass-and-play role reveal, narrator-guided nights, day and voting screens, a role library, and settings.
- Local save data so a game can be resumed after closing the app.
- A GitHub Actions workflow that checks the source, builds the Android debug APK, and publishes it as a downloadable workflow artifact.

## Build an Android APK on GitHub

1. Create or open a GitHub repository and upload the contents of this project.
2. Push the files to the repository's `main` branch. GitHub Actions will start automatically. You can also start **Build OrbitsCoder APK** from the repository's **Actions** tab.
3. Open the successful run and download **orbitscoder-debug-apk** from **Artifacts**. The artifact is available for 30 days.
4. Transfer `app-debug.apk` to an Android device and open it to install. This is a debug APK for testing and direct installation; it is not a Play Store release.

The workflow uses the included `package-lock.json` and runs both the TypeScript/Vite production build and the game-engine checks before building Android. If either check fails, no APK artifact is uploaded.

## Build locally

Install Node.js 22 and Java 17. For local Android builds, install Android SDK Platform 34 and Build Tools 34.0.0. Then run:

```sh
npm ci
npm run test:engine
npm run build
npx cap add android   # first time only
npx cap sync android
cd android && ./gradlew assembleDebug
```

The APK is created at `android/app/build/outputs/apk/debug/app-debug.apk`.

## Project layout

- `src/engine/` contains game rules, role data, role balancing, and win checks.
- `src/stores/` contains the Pinia game state and local save handling.
- `src/views/` contains the game screens.
- `tests/engine.test.ts` contains the engine checks.
- `.github/workflows/build-android.yml` builds and uploads the Android debug APK.

For Play Store distribution, generate and protect a signing key, then create a signed release build. Do not commit signing keys or passwords to the repository.
