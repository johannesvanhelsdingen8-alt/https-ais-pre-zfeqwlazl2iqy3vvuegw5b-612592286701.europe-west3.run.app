# Packaging TITAN X: HYPERFORGE React Web App into Android APK with Capacitor

This guide takes your compiled React + Vite + Tailwind CSS bundle and packages it into a native, installable Android APK with Root (`su`) bridge support.

---

### Prerequisites
1. **Node.js** (v18+ or v20+)
2. **Android Studio** (Koala / Ladybug or newer) with Android SDK 35 installed
3. **Java JDK 17 or 21**

---

### Step 1: Install Capacitor in this React Project
In your terminal, run:
```bash
npm install @capacitor/core @capacitor/cli @capacitor/android
```

---

### Step 2: Initialize & Build the Web App
Build the Vite production bundle:
```bash
npm run build
```

Initialize Capacitor (using the provided `capacitor.config.json`):
```bash
npx cap add android
```

---

### Step 3: Copy Native Bridge & Gradle Files
Replace the generated files in the `android/` folder with the pre-configured files from this ZIP:
- Copy `android/app/build.gradle` -> `android/app/build.gradle`
- Copy `android/app/src/main/AndroidManifest.xml` -> `android/app/src/main/AndroidManifest.xml`
- Copy `android/app/src/main/java/com/apextuner/app/MainActivity.java` -> `android/app/src/main/java/com/apextuner/app/MainActivity.java`
- Copy `android/app/src/main/java/com/apextuner/app/RootBridgePlugin.java` -> `android/app/src/main/java/com/apextuner/app/RootBridgePlugin.java`
- Copy `android/app/proguard-rules.pro` -> `android/app/proguard-rules.pro`

Sync assets:
```bash
npx cap sync android
```

---

### Step 4: Build the Debug APK (Quick Test)
Run Gradle directly in the `android` directory:
```bash
cd android
./gradlew assembleDebug
```

Your APK is compiled at:
`android/app/build/outputs/apk/debug/app-debug.apk`

Install directly to your connected device:
```bash
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

---

### Step 5: Build Production Signed Release APK
1. Generate your release keystore:
```bash
keytool -genkey -v -keystore release-key.keystore -alias apextuner -keyalg RSA -keysize 2048 -validity 10000
```
Place `release-key.keystore` inside the `android/app/` folder.

2. Compile optimized release APK:
```bash
./gradlew assembleRelease
```

Production APK output:
`android/app/build/outputs/apk/release/app-release.apk`
