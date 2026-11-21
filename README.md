# Saathi (সাথী) — Voice-First Bengali, English & Hindi Android Assistant

**Saathi (সাথী)** is a native Kotlin & Jetpack Compose Android application (`applicationId`: `com.aistudio.saathi.kxmpzq`).

## Project Structure

- `app/src/main/java/com/example/MainActivity.kt` — Edge-to-edge activity entry point
- `app/src/main/java/com/example/ui/SaathiApp.kt` — Adaptive Navigation Compose shell (`NavigationBar` / `NavigationRail`) & runtime `RECORD_AUDIO` permission handling
- `app/src/main/java/com/example/ui/SaathiViewModel.kt` — MVVM state management across voice, chat, song/script, YouTube, image studio, and smart actions
- `app/src/main/java/com/example/data/SaathiAiRepository.kt` — Generative AI interface (`SaathiAiRepository`) and `PlaceholderSaathiAiRepository` with Creator Rule (`"আমাকে আমার রাকেশ বস তৈরি করেছে।"`) and 3-language support (`বাংলা`, `English`, `हिन्दी`)
- `app/src/main/java/com/example/data/SaathiPreferences.kt` — DataStore Preferences (`selectedLanguage`, `ttsAutoSpeak`, `wakeWordEnabled`, `autoListenEnabled`, `momPhoneNumber`)
- `app/src/main/java/com/example/voice/SaathiVoiceController.kt` — Built-in Android `SpeechRecognizer` (with `"Hey Saathi"` / `"সাথী"` wake-word standby) and built-in `TextToSpeech` (female `bn-IN`/`bn-BD`, `en-IN`/`en-US`/`en-GB`, `hi-IN` voice selection, rate `0.95f`, emoji/symbol sanitization, quiet turn-taking)
- `app/src/main/java/com/example/launcher/AppLauncherHelper.kt` — Native Android deep links & browser fallback for YouTube, WhatsApp, Facebook, Instagram, Telegram, Phone dialer, and Email
- `app/src/main/java/com/example/ui/screens/` — Compose screens (`ChatVoiceScreen`, `SongScriptScreen`, `YouTubeStudioScreen`, `ImageStudioScreen`, `SmartActionsScreen`)
- `app/src/main/java/com/example/ui/theme/` — Midnight Obsidian dark glassmorphism theme & bundled `Noto Sans Bengali` + `Plus Jakarta Sans` fonts
- `.github/workflows/build-apk.yml` — Automated GitHub Actions APK build workflow

---

## How to Build a Real `.apk` File

Because AI Studio Export provides the complete Android source project (without pre-built `.apk` binaries), use any of the three methods below to compile the real installable `.apk`:

### Option 1: GitHub Actions (No Local Setup Required)
1. Push this project to a GitHub repository using the AI Studio **GitHub** menu (or `git push`).
2. Open the **Actions** tab on GitHub and select **Build Saathi Android APK** (auto-runs on push, or click **Run workflow**).
3. When the workflow finishes, download **`saathi-debug-apk`** from the **Artifacts** section and install `app-debug.apk` on your Android device.

### Option 2: Android Studio (Recommended for Local Builds & Release Signing)
1. Export/download the project ZIP from AI Studio and extract it.
2. Open a terminal in the project root and restore `debug.keystore` once:
   ```bash
   base64 -d debug.keystore.base64 > debug.keystore
   ```
3. Open the folder in **Android Studio**. Wait for Gradle sync to finish.
4. Click **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
5. Click **locate** in the notification, or find the APK at:
   `app/build/outputs/apk/debug/app-debug.apk`
6. For Play Store / release signing, use **Build > Generate Signed Bundle / APK...** with your own upload keystore (or set `KEYSTORE_PATH`, `STORE_PASSWORD`, and `KEY_PASSWORD` environment variables).

### Option 3: Command Line (Gradle CLI)
Ensure **JDK 17+** and the **Android SDK** (`ANDROID_SDK_ROOT`) are installed, then run:
```bash
base64 -d debug.keystore.base64 > debug.keystore
gradle :app:testDebugUnitTest
gradle :app:assembleDebug
```
The compiled APK will be generated at:
`app/build/outputs/apk/debug/app-debug.apk`

---

## Note on AI & Firebase Configuration
`SaathiAiRepository` currently uses `PlaceholderSaathiAiRepository` (labeled sample mode) because Firebase provisioning was skipped. To connect live Gemini and Nano Banana image generation, configure Firebase AI Logic (`google-services.json`, App Check, and `com.google.firebase:firebase-ai`) and provide a `FirebaseAiRepository` implementation of `SaathiAiRepository`.
