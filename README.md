# Sermon Scribe

Browser-based live sermon transcription, AI study-note generation, and optional SharePoint/Power Apps integration. This is a static HTML application; API keys configured in the app are kept in browser storage.

## Input options

- **Live microphone:** browser speech recognition displays rough dictation in the editor.
- **Also record audio:** a checkbox enabled by default; when the live microphone starts, a separate MediaRecorder captures compressed audio into local IndexedDB in roughly 10-second pieces. Stopping the microphone stops its paired recording.
- **Record audio only:** records a lecture without needing browser live speech recognition; press again to stop.
- **AI transcript of recording:** after stopping, transcribes saved audio with the configured Gemini key and appends the AI version below the live text. Recorded files can be downloaded or discarded separately. Recording is not automatically uploaded to SharePoint.
- **Attach audio:** existing file-transcription path for MP3, M4A, WAV, WebM and compatible formats. Failed audio slices are kept locally for manual retry.
- **Public Media Link:** accepts public YouTube, TikTok, Instagram, X/Twitter, Facebook media posts, and direct HTTPS file URLs (e.g., MP3/M4A/WAV/MP4/WebM). Ordinary webpage/podcast landing URLs, private or login-required links, live streams and unsupported sources do not work.

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

1. Start live dictation with **Also record audio** checked, stop, refresh the tab, verify the recording remains, download it, and run **AI transcript of recording**.
2. Uncheck the paired recorder and ensure live dictation still works without saved audio. Test **Record audio only** separately.
3. Attach an MP3, force an incorrect Gemini key to verify failed-slice retry, then retry with a valid key.
4. Test a captioned YouTube video without enabling Supadata AI generation; confirm Gemini fallback still works if needed.
5. Test a public non-YouTube video post and a direct audio file with generation opt-in; confirm both transcription and study-note paths.
6. Verify invalid `http://` URLs, regular webpage URLs, and private/internal URLs are rejected.
7. Confirm no transcript, notes, audio, or SharePoint item is overwritten/saved without the UI's existing explicit actions.
