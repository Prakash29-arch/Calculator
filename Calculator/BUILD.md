# ExactCalculator – standalone Android Studio / Gradle project

Circular-key redesign (One UI style): pure-black canvas, deep-purple digit circles, lighter-violet
function keys (C, ( ), %, +/-), vivid-purple operator circles, an orange-to-deep-gold "=" key,
gold scientific drawer, orange operators inside the formula. Android 15–17 edge-to-edge handling.  minSdk 26, compileSdk/targetSdk 37.  applicationId
`com.android.calculator2.purple` (so it never clashes with a system calculator).

## Build (Android Studio)
1. Use a recent Android Studio (Panda 3 / 2025.3.3 Patch 1 or newer) with JDK 17+.
2. File > Open… and pick this folder, let Gradle sync (needs internet for AndroidX).
3. Build > Build Bundle(s) / APK(s) > Build APK(s).
   Output: app/build/outputs/apk/debug/app-debug.apk

## Build (command line)
Gradle wrapper jar can't be bundled here, so create it once with an installed Gradle:
    gradle wrapper --gradle-version 8.13
    ./gradlew assembleDebug        (Windows: gradlew.bat assembleDebug)

## Pinned versions
Android Gradle Plugin 8.11.2, Gradle 8.13, JDK 17.
Do NOT accept Android Studio's "Upgrade AGP" prompt: this code uses switch(R.id.*) which needs
android.nonFinalResIds=false, and AGP 9 ignores that setting.

## Changes vs. the AOSP module
- Gradle files, manifest `package` attr removed (uses `namespace`).
- CalculatorResult: BuildCompat.isAtLeastN() -> Build.VERSION.SDK_INT >= N (removed from AndroidX).
- Calculator/Licenses: window-inset padding for enforced edge-to-edge (API 35+/36+/37).
- res/values/color.xml: new palette; res/values/styles.xml + dimens: circular key styles
  (the per-size styles.xml overrides were removed, sizes now come from dimens).
- Keypad is a 4x5 grid: C ( ) % / 7 8 9 / 4 5 6 / 1 2 3 / +/- 0 . with the operator column on the right.
- New row under the display: history, scientific-page toggle, backspace (layout/action_row.xml).
- Calculator.java: smart ( ) key, +/- key (CalculatorExpr.toggleTrailingSign), operator colouring,
  backspace after a result clears, onActionClick().
- Phone portrait (>=768dp tall) now uses the swipeable pager layout instead of the tablet layout.

## Build in the cloud (no Android Studio needed)
1. Create a GitHub repository and upload the contents of this folder (including `.github/`).
2. Open the Actions tab > "Build APK" > Run workflow.
3. When it finishes, download the `ExactCalculator-debug-apk` artifact (contains app-debug.apk).
The debug APK is signed with a debug key and can be installed directly on a phone.
