# FraudShield AI

A prototype mobile-style web app that checks job posts for common recruitment-scam signals.
Paste text, upload a screenshot, take a photo, scan a QR code or dictate a post, and get a
0-100 trust score with a plain-language explanation of why.

> **Status: prototype.** Not production-ready and not yet suitable for the Play Store as-is.
> See "Before publishing" below.

## What works

- **Trust score + explanation** from a transparent rule-based engine (upfront fees, personal email
  domains, WhatsApp/Telegram-only contact, urgency language, unrealistic pay, shortened/suspicious
  links, brand look-alike domains, and positive signals such as company domains and careers pages).
  It is a keyword/pattern engine, **not** a trained ML model.
- **On-device OCR** (Tesseract.js) and **QR decoding** (jsQR) from a photo or uploaded image.
- **Voice input** (Web Speech API, needs https / a supported browser).
- **Share / copy / save report as image** (html2canvas + Web Share API).
- Dashboard, history, company check (text-based heuristics), community reports with voting,
  assistant chat (canned, rule-based answers).

## What does not exist yet

- **No backend.** History, reports, votes and the profile live in the device's `localStorage` only.
  Community reports are *not* shared between users.
- **The sign-in screen is a local placeholder.** The "verification code" is generated and shown on
  screen; there is no real authentication, password storage or account recovery.
- **No live lookups:** no WHOIS, SSL, company-registry or domain-age checks. The "Company" tab only
  inspects the text you provide, plus a demo estimate of domain age (not real data).
- **PDF text extraction and fetching a job URL's page content** are not implemented.
- No deepfake detection, blockchain verification or browser extension.

## Run locally

```bash
npm install        # only needed for the Capacitor wrapper
npm start          # serves ./www at http://localhost:3000
```

Or just open `www/index.html` with any static server (voice input, clipboard and share need
https or localhost; they will not work from a `file://` URL).

External scripts (lucide icons, Tesseract.js, jsQR, html2canvas) load from the jsDelivr CDN, so an
internet connection is required, including for OCR language data on first use.

## Wrap as an Android app (Capacitor)

```bash
npm install
npx cap add android
npx cap sync android
npx cap open android      # opens Android Studio; build a signed .aab from there
```

Change `appId` in `capacitor.config.json` to your own reverse-domain id before the first build.

## Before publishing to Google Play

1. **Replace the placeholder auth and storage** with a real backend if you want accounts or shared
   community reports (Firebase, Supabase or your own API).
2. **Bundle the CDN libraries locally** (and Tesseract's worker, core and language data) so the app
   works offline and doesn't depend on third-party CDNs at runtime.
3. **Write a privacy policy and complete the Data safety form.** The app handles job-post text,
   photos, names and emails.
4. **Be accurate in the store listing.** The scoring is rule-based; avoid claiming ML/AI detection or
   guaranteed fraud protection. A wrong score can mislead a job seeker.
5. Create a Play developer account, a signed Android App Bundle, icon, screenshots and the content
   rating questionnaire. New personal accounts must run a closed test before production, so check
   the current Play Console requirements.
6. Test on real devices (camera permission, voice permission, different screen sizes).

## Project layout

```
www/index.html          the whole app (HTML + CSS + JS)
capacitor.config.json   Capacitor settings
package.json            scripts and Capacitor dependencies
```
