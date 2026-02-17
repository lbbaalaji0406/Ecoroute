# EcoRoute — Sustainable Urban Journey Planner
## APK Installation & Flipper Guide

---

## Files Included

| File | Purpose |
|------|---------|
| `EcoRoute.apk` | Android APK (install on device) |
| `index.html` | The complete HTML5 app (also open in browser) |
| `MainActivity.java` | Android WebView wrapper source |
| `build_apk.py` | APK builder script |

---

## Option 1 — Open in Browser (Instant Demo)
Just open `index.html` in Chrome/Safari on your laptop or phone.
Works perfectly as a PWA-style demo.

---

## Option 2 — Install APK on Android Phone

### Enable Unknown Sources
1. Go to **Settings → Security** (or **Settings → Apps → Special app access**)  
2. Enable **"Install unknown apps"** or **"Unknown sources"**
3. On Android 8+: enable it for your file manager app specifically

### Transfer & Install
```bash
# Via USB (ADB):
adb install EcoRoute.apk

# Via WiFi/Bluetooth:
# Copy APK to phone → open file manager → tap EcoRoute.apk → Install
```

---

## Option 3 — Flipper (Recommended for Hackathon Demo)

Flipper can display web apps as mock mobile apps. Since our APK contains
a WebView loading `index.html`, use this workflow:

### Method A — Direct APK Sideload via Flipper
1. Connect phone via USB to your laptop
2. Open **Flipper** → select your device
3. Use **"Device → Install App"** and select `EcoRoute.apk`
4. The app will install and launch automatically

### Method B — Use index.html as Flipper Plugin
1. In Flipper, add a **WebView** or **Browser** plugin
2. Point it to the local path of `index.html`
3. This renders identically to the native app

### Method C — ADB + Flipper combined
```bash
# Install via ADB (Flipper will detect it)
adb install -r EcoRoute.apk

# Launch directly
adb shell am start -n com.ecoroute.app/.MainActivity
```

---

## Signing the APK (for real device install without ADB)

The included APK is **unsigned**. To sign it:

```bash
# Generate a debug keystore (one time)
keytool -genkey -v -keystore debug.keystore \
  -alias androiddebugkey \
  -keyalg RSA -keysize 2048 \
  -validity 10000 \
  -storepass android \
  -keypass android \
  -dname "CN=EcoRoute,OU=Dev,O=Hackathon,L=Chennai,S=TN,C=IN"

# Sign the APK (requires apksigner from Android SDK Build Tools)
apksigner sign \
  --ks debug.keystore \
  --ks-key-alias androiddebugkey \
  --ks-pass pass:android \
  --key-pass pass:android \
  --out EcoRoute-signed.apk \
  EcoRoute.apk

# Install the signed APK
adb install EcoRoute-signed.apk
```

**Or use Android Studio:**
1. Open the project
2. Build → Generate Signed APK
3. Use the debug keystore above

---

## App Features

- **3-screen flow**: Home → Routes → Book
- **Live departure times** calculated from actual current time
- **Via-stop routing**: Add "Nandanam" as a via stop — routes reshuffle
- **First/last mile distances** shown on every leg
- **Live conditions**: Rain, traffic, delays on each route card
- **Carbon comparison**: Every route vs petrol car
- **Booking confirmation sheet** with CO₂ saved + tree equivalence
- **Back button**: Navigates within app correctly

## Tech Stack

- Pure HTML5 + CSS + Vanilla JS (no framework dependency)
- Runs offline after first load
- Android WebView wrapper (MainActivity.java)
- Target: Android 5.0+ (API 21+)

---

## Hackathon Talking Points

1. **The via-stop demo**: Add "Nandanam" → watch all routes change
2. **Live departure board**: Show times updating from current time
3. **Carbon sheet**: Book any route → CO₂ saved + trees popup
4. **First-mile clarity**: Every walk leg shows exact distance (420m, not just "walk")
5. **Petrol cab shown last**: "We show what you're avoiding"
