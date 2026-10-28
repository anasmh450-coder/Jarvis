# Jarvis for Android TV

A voice assistant for your TV. A glowing orb sits in the top-right corner of the screen, swells when you talk and pulses when Jarvis talks. Powered by Gemini.

Say **"Jarvis"** and ask anything, or tell him to control the TV:

- "Jarvis, open YouTube and search for Formula 1 highlights"
- "Jarvis, turn the volume up"
- "Jarvis, what's on my screen?"
- "Jarvis, who won the match last night?"

After he answers you have about 9 seconds to keep talking without saying his name again.

## Build the APK (GitHub does it for you)

1. Create a new GitHub repository and upload everything in this folder (keep the `.github` folder).
2. In the repo go to **Settings > Secrets and variables > Actions > New repository secret**.
   Name: `GEMINI_API_KEY`, value: your Gemini key.
3. Go to the **Actions** tab, open **Build Jarvis APK**, click **Run workflow**.
4. When it finishes, download the `jarvis-tv-apk` artifact. Inside is `app-debug.apk`.

The key is never written into the source code. If you skip step 2, the app shows a box to paste the key on the TV instead.

## Install on the TV

Copy `app-debug.apk` to the TV (the "Downloader" app, a USB stick, or `adb install app-debug.apk`).

## First run

Open **Jarvis** on the TV and go through the list:

1. Allow microphone
2. Allow floating orb (overlay)
3. Enable TV control (accessibility)
4. Tap **Test AI connection**. It should say OK.
5. Tap **Start Jarvis**

If your TV has no settings screen for these, the app shows the `adb` commands to run from a computer.

## Use the remote's Assistant button

Press the **Assistant / mic button** on your remote and Jarvis pops up at the bottom of the screen with a big orb and captions, and starts listening right away (no need to say his name). Press it again while he is talking to cut him off.

Two ways are built in, and they can work together:

1. **Step 3 (accessibility)** catches the button before Google's Assistant sees it. This is usually enough.
2. **Step 4 (assistant app)** registers Jarvis as the TV's assistant. If your TV has no settings screen for it, run from a computer:
   ```
   adb shell settings put secure assistant com.myassistant.tv/com.myassistant.tv.JarvisInteractionService
   adb shell settings put secure voice_interaction_service com.myassistant.tv/com.myassistant.tv.JarvisInteractionService
   ```
   To go back to Google's assistant later, choose it again in the TV's assistant settings.

Jarvis must be running (tap **Start Jarvis** once; he stays on in the background). Some TVs send the button to a manufacturer app instead of Android, and then neither way can catch it.

## Good to know

- **Microphone:** the app listens through a microphone the TV exposes to apps (built-in mic, USB or Bluetooth mic). The mic inside many remotes is not available to apps.
- **Protected video** such as Netflix shows black in screenshots. Jarvis also reads the visible text on screen, which still works.
- **Limits of control:** Jarvis moves focus and presses OK like a remote, clicks items by their text, scrolls, types, opens apps and controls volume and media. Some apps block accessibility control.
- Jarvis is always listening while he is running. Audio is sent to Google (Gemini) only when it sounds like speech.
- Wake word, model and orb size can be changed in `AssistantService.kt`, `Config.kt` and `OrbView.kt`.
