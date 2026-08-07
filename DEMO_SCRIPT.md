# TikTok Developer Review Demo Script

## Recording goal

Show the complete FC Haber Botu user journey without exposing credentials: open the site, authorize TikTok, return to the app, choose an example MP4, and confirm that the upload is accepted as a TikTok draft.

## Before recording

1. Serve this folder from the review/demo host.
2. Use a test TikTok account and a test developer app.
3. Configure the registered redirect URI to the review backend, never to a local secret or a static HTML page.
4. Keep `TIKTOK_CLIENT_KEY`, `TIKTOK_CLIENT_SECRET`, access tokens, and refresh tokens on the backend only.
5. Prepare one vertical MP4 with rights-cleared football footage. Do not use a real user’s private information.
6. If recording a real API run, replace the demo button handlers with the backend endpoints described below. Do not alter the production render pipeline.

## Spoken narration and on-screen actions

### Scene 1 — Open FC Haber Botu

**Action:** Open the FC Haber Botu website and show the landing page.

**Narration:** “This is FC Haber Botu, an automated football-news publishing tool. The user can connect a TikTok account and submit an approved vertical video.”

**Show:** Product name, TikTok Developer Review Demo label, and the privacy/terms links.

### Scene 2 — Start TikTok OAuth

**Action:** Click **Login with TikTok**.

**Narration:** “I’ll connect a test TikTok account using TikTok OAuth. The application does not ask for or display the TikTok password.”

**Live implementation:** The backend creates the authorization URL with the registered redirect URI and `video.upload` scope, then redirects the browser to TikTok. Never put the client secret in the browser.

### Scene 3 — Authorize on TikTok

**Action:** On TikTok’s authorization screen, show the app name, requested permission, and test account. Click **Authorize**.

**Narration:** “TikTok shows the permission request. The user explicitly approves access for the upload workflow.”

### Scene 4 — Return to FC Haber Botu

**Action:** Let TikTok redirect back to the FC Haber Botu callback. Show the connected state on the website.

**Narration:** “The authorization callback returns to FC Haber Botu. The backend exchanges the code for tokens and stores them securely; the browser receives only the connected state.”

**Show:** “Authorization complete. Returned to FC Haber Botu.” Do not show token values.

### Scene 5 — Select an MP4

**Action:** Open the example video selector and choose `fc-haber-example-short.mp4`.

**Narration:** “Next I select a prepared 1080 by 1920 MP4. This file has already passed the application’s review and quality checks.”

### Scene 6 — Upload to TikTok

**Action:** Click **Upload to TikTok**.

**Narration:** “I’ll now submit the approved video to TikTok through the Content Posting API. The upload is authenticated with the stored access token.”

**Live implementation:** The backend initializes the TikTok upload, transfers the MP4 to the returned upload URL, retries only transient failures, and records the returned `publish_id`. Do not record access tokens or upload URLs containing query credentials.

### Scene 7 — Confirm success or draft creation

**Action:** Show the success state: “TikTok draft created.” If available, show only the publish ID or a platform-safe status link.

**Narration:** “The upload succeeded and TikTok created a draft. The creator can open TikTok to finish editing and publish it according to TikTok’s workflow.”

## Closing narration

“This completes the FC Haber Botu TikTok integration: explicit OAuth consent, secure server-side token handling, a user-selected MP4, and a confirmed TikTok upload.”

## Review safety notes

- The checked-in demo page is simulation-only and deliberately contains no credentials or live API calls.
- A live recording needs a small backend route for `/auth/tiktok`, `/auth/tiktok/callback`, and `/api/tiktok/upload`.
- Do not claim a live upload when the page is running in demo mode; show the on-screen demo notice or use the real review backend.
- Use TikTok’s current Content Posting API scopes, redirect URI, rate limits, and review requirements.
