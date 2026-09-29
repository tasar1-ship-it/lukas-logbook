# Logbook - handover

Shared checklist for Lukas (sections: Sailing, School, Daily life). Dad's phone and Lukas's phone read and write the same list. Dad's changes are collected and sent to Lukas with one tap on WhatsApp.

## Current version
- App 1.0.0, service worker `logbook-v1` (29 Sep 2026). Not yet deployed.
- Intended repo: tasar1-ship-it/lukas-logbook, live at https://tasar1-ship-it.github.io/lukas-logbook/

## Files
- index.html - the whole app (inline CSS and JS, ES5 only)
- sw.js - service worker, precaches the shell and the Firebase SDK
- manifest.json - no start_url on purpose, so Add to Home Screen keeps the ?j= setup link
- icon-180.png, icon-512.png
- firebase-app-compat.js, firebase-auth-compat.js, firebase-database-compat.js - Firebase compat SDK 9.23.0, vendored from npm (no CDN). These three are minified ES2017, not ES5; they run on iOS 15 and later, which includes the iPhone 13. The ES5 rule covers the app's own code.
- database.rules.json - Realtime Database rules to paste into Firebase

## Constants
- Firebase: own project, Realtime Database in europe-west1, Anonymous sign-in, compat SDK 9.23.0
- Data path: logbook/<24-character family key>/ with items, days (tick counts per day for the streak), meta (names)
- localStorage prefix: lb1_ (cfg, key, role, pending, phone, filter, sec, seen, hintHidden)

## How setup works
- Dad's phone: first open shows a setup page; paste the Firebase web config; the app creates the family key.
- Settings -> "Send Lukas his copy" opens WhatsApp with a link containing ?j=<code> (family key, Firebase config, role) and the same code as text.
- Lukas opens the link in Safari, then Share -> Add to Home Screen. If the home-screen app opens empty, he pastes the code on the setup page.

## WhatsApp
- whatsapp:// link with the message pre-written; Dad presses Send. Not automatic by design (no paid messaging provider).
- Lukas's number is stored on Dad's phone only, never in Firebase.

## Voice
- The mic button focuses the text field and tells Lukas to use the iPhone keyboard microphone. No browser speech API.

## Security
- Rules: signed-in (anonymous) users only, and only under a family key of 20+ characters. Anyone who has the join link or code can read and write the list; treat the link like a house key. To revoke: Disconnect both phones and set up again (new key); delete the old node in the Firebase console.

## Known limits
- Changes by Lukas do not notify Dad; Dad sees them (marked with Lukas's name) when he opens the app.
- If iOS ignores the ?j= link on Add to Home Screen, the pasted code is the fallback.

## Last change
- 1.0.0 first build.
