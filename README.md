# Sermon Scribe

Browser-based live sermon transcription, AI study-note generation, and optional SharePoint/Power Apps integration. This is a static HTML application; API keys configured in the app are kept in browser storage.

## Input options

- **Live microphone:** browser speech recognition displays rough dictation in the editor.
- **Also record audio:** a checkbox enabled by default. When you start live dictation, a separate MediaRecorder captures compressed audio into local IndexedDB in roughly 10-second pieces. **Stopping or losing live speech recognition does not stop the original audio.** Use the dedicated **⏹ Stop recording** control (or the audio button) to stop it.
- **Record audio only:** records a lecture without needing live speech recognition; the same button becomes **⏹ Stop audio recording** while recording is active. Turning the **Also record audio** checkbox off only changes future starts; it never silently stops a recording already in progress.
- **AI transcript of recording:** after stopping, transcribes saved audio with the configured Gemini key and appends the AI version below the live text. Recorded files can be downloaded or discarded separately. Recording is not automatically uploaded to SharePoint.
- **Attach audio:** existing file-transcription path for MP3, M4A, WAV, WebM and compatible formats. Failed audio slices are kept locally for manual retry.
- **Public Media Link:** accepts public YouTube, TikTok, Instagram, X/Twitter, Facebook media posts, and direct HTTPS file URLs (e.g., MP3/M4A/WAV/MP4/WebM). Ordinary webpage/podcast landing URLs, private or login-required links, live streams and unsupported sources do not work.

## Recording reliability and background tabs

- **Original recording is independent.** After starting both microphone dictation and audio capture, the live speech-recognition engine may pause, error, or be stopped manually. The separate audio recorder continues until you explicitly stop it or the browser/device interrupts it. A red recording indicator, elapsed clock (based on actual wall time), and local saved-bytes count help verify capture. They are indicators, not guarantees that every second of audio has been saved.
- **Switching tabs:** Desktop Chrome/Edge generally allow an active MediaRecorder to continue in a background tab. Live dictation may pause; the app attempts to resume live speech recognition when the tab becomes visible again. Tab suspension, browser closure, device sleep, OS permission changes, microphone disconnections and mobile background restrictions can still stop capture. **Test on your device before relying on a long lecture recording.**
- **Screen wake lock:** The app requests a Screen Wake Lock whenever live dictation or original audio recording is active, and attempts to reacquire it when returning to a visible tab. The status strip tells you when the lock is active, unsupported, blocked by power settings, or unavailable in the background. **The Web Wake Lock API cannot keep the screen on while this tab is hidden and cannot override OS-level sleep settings.**
- **Local persistence and recovery:** Saved chunks remain in browser IndexedDB across a refresh; if the browser closes unexpectedly, any chunks successfully persisted may be recoverable. Recordings interrupted unexpectedly are marked as interrupted. The browser is asked for persistent storage on recording start, but this may be denied. Local storage is not a backup; download valuable recordings when finished.
- **Warnings:** The app warns about media-recorder errors, microphone tracks ending, local storage failures, and no recently saved audio chunk when the tab is visible. An active recorder may still be interrupted despite its status indicator. When closing a tab with active audio capture, the app requests a browser confirmation dialog where supported.
- **Desktop power settings:** For more reliable sessions, use external power, configure your computer's sleep timeout appropriately, and keep the Sermon Scribe site active in Chrome/Edge performance settings. The app does not change OS policies or prevent screen savers while you use another tab.

## Public Media Link usage

1. Paste the public media URL and select **Transcribe Verbatim to Editor** or **Generate Expository Study Notes**.
2. Add a personal **Supadata API key** for non-YouTube media. The default mode only fetches native captions. Check **Allow Supadata AI transcription** to generate speech transcripts when captions are unavailable; this uses additional provider credits. Direct public files generally require AI generation.
3. For YouTube, native or generated Supadata transcripts are attempted first when a key is supplied. If not available, the existing Gemini YouTube clip workflow remains, including manual-duration and resume controls. Gemini is also used for AI study notes.
4. Review transcript and generated notes. **Saving to SharePoint is still a separate, manual action.**

You can keep using the previous URL query aliases (`youtubeUrl`, `yt`) or pass `mediaUrl` when deep-linking into the app.

## Safety, privacy and browser limitations

- Ask permission before recording a speaker or sharing their words. Supported links must be publicly accessible; honor content rights and platform restrictions.
- Browser recording requires microphone permission and a supported HTTPS browser. Recorded chunks stay in **this browser and device**, not the GitHub repository or SharePoint. Clearing site data, private browsing, changing devices, or hitting storage limits can remove audio. **Download important recordings promptly.**
- Gemini audio transcription sends the audio chunks to Google after the **AI transcript** action; Supadata processes URLs only after **Start Processing**. Do not put sensitive recordings or secrets in public URLs.
- Supadata's direct transcript API can return an asynchronous job. Longer jobs may take several minutes; closing the browser during a job may interrupt this UI's polling. Provider availability, supported sites, rate limits and credit pricing can change.
- The implementation is tested for JavaScript syntax and source routing; microphone permissions, multi-hour lectures, mobile Safari support, Supadata API calls, and Gemini quotas should be tested in a browser before merging to the public site.

## Suggested manual checks before publishing

1. Start live dictation with **Also record audio** checked. Stop live dictation and confirm the red audio indicator, elapsed time, and local saved-bytes count continue updating; then click **⏹ Stop recording**, refresh, download the recording, and run **AI transcript of recording**.
2. While recording audio, change browser tabs for several minutes, then return: verify recording is still active, the screen-wake indicator shows reacquisition when possible, and live dictation is allowed to resume. Then test **Record audio only** without speech recognition.
3. Start **Record audio only**, stop it independently, and verify downloaded audio plays correctly. Uncheck the paired recorder and ensure live dictation starts without making an additional audio file.
4. Test microphone permission refusal, a disconnected microphone, and a browser refresh while recording. Confirm warnings and any already-saved audio; do not assume uninterrupted audio after a refresh.
5. Attach an MP3, force an incorrect Gemini key to verify failed-slice retry, then retry with a valid key.
6. Test YouTube native captions, Gemini YouTube fallback, and a public non-YouTube media link with Supadata AI-generation opt-in.
7. Verify invalid URLs are rejected and that SharePoint remains unchanged until an explicit save. Perform an extended 60–90 minute test on the intended laptop (plugged in) before relying on the tool.
