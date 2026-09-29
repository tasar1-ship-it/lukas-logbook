# Logbook - handover

Shared checklist for Lukas (sections: Sailing, School, Daily life). Dad's devices and Lukas's iPhone read and write the same list. Dad's changes are collected and sent to Lukas with one tap on WhatsApp.

## Current version
- App 2.0.0, service worker `logbook-v3` (29 Sep 2026). Complete redesign.
- Repo: tasar1-ship-it/lukas-logbook, live at https://tasar1-ship-it.github.io/lukas-logbook/
- Sample view: https://tasar1-ship-it.github.io/lukas-logbook/?demo (in-memory sample entries and two sample photos, nothing saved or sent, button to switch between Dad's and Lukas's view)
- Firebase project: lukas-logbook, Realtime Database lukas-logbook-default-rtdb in europe-west1, Anonymous sign-in on, rules from database.rules.json published, web app "Logbook" registered. Gemini and Google Analytics switched off.

## Design (2.0)
- Race-broadcast look: dark ocean blue, Avenir Next Condensed heavy italic for display type, section colours Sailing cyan #29E0FF, School buoy yellow #FFD23F, Daily life mark orange #FF6B2C.
- Header: rank chip and XP bar, streak flame, "today's race" card: done/total, status word (Start line, On course, Final leg, Bullet!) and a course with one buoy per task and an Optimist that sails along as tasks are ticked.
- Ranks by XP: Deckhand 0, Crew 100, Trimmer 250, Helm 450, Skipper 700, Captain 1000, Commodore 1400, Admiral 2000, Legend 3000. XP per tick: Dad's task 15, own task 10, photo task +5. Unticking takes the same XP back.
- Ticking: burst, "+XP" float, short sound (switchable in Settings). Rank-up and "BULLET!" (everything done) overlays.
- Adding: chat-style bar at the bottom: camera, text field, and a button that is a microphone when empty (points to keyboard dictation) and a send arrow when there is text. Section chips appear while typing.
- Photo tasks: camera opens directly on iPhone (file picker on the Mac), photo shrunk to max 900 px JPEG (about 10-80 KB) and stored in the database under photos/<item id>; shown as a big card with a check button; ticked photos get a DONE stamp. Tap a photo to open it full screen, mark done or edit.

## Files
- index.html - the whole app (inline CSS and JS, ES5 only)
- sw.js - service worker, precaches the shell and the Firebase SDK
- manifest.json - no start_url on purpose, so Add to Home Screen keeps the ?j= setup link
- icon-180.png, icon-512.png
- demo-kit.jpg, demo-homework.jpg - sample photos for ?demo only (drawn, not real photos)
- firebase-app-compat.js, firebase-auth-compat.js, firebase-database-compat.js - Firebase compat SDK 9.23.0, vendored from npm (no CDN). Minified ES2017; runs on iOS 15 and later. The ES5 rule covers the app's own code.
- database.rules.json - Realtime Database rules as published

## Constants
- Data path: logbook/<24-character family key>/ with items, photos, days (ticks per day, for streak and today's race), stats/xp, meta (names)
- Item fields: t text, s section, d done, by dad|kid, c created, dt done time, dk done day, p 1 if photo, xp earned
- localStorage prefix: lb1_ (cfg, key, role, pending, phone, filter, sec, seen, hintHidden, sound)

## Devices and setup
- Dad's first device: setup page, paste the Firebase web config, the app creates the family key.
- Settings -> "Copy link for my other device" gives a Dad link (role d) so every Dad device shares one list.
- Settings -> "Send Lukas his copy" opens WhatsApp with a ?j= link (role l) and the same code as text.

## WhatsApp
- iPhone/Android: whatsapp:// link with the message pre-written. Laptop: https://wa.me/ in a new tab. Dad presses Send. Not automatic by design.
- Lukas's number is stored on Dad's device only, never in Firebase.

## Security
- Rules: signed-in (anonymous) users only, and only under a family key of 20+ characters. Anyone with a join link or code can read and write the list, photos included. To revoke: disconnect all devices, set up again (new key), delete the old node in the Firebase console.

## Known limits
- Changes by Lukas do not notify Dad.
- Photos live in the Realtime Database (Firebase Storage needs the paid plan). Fine for family use; "Clear finished" deletes the photos of finished tasks.
- If iOS ignores the ?j= link on Add to Home Screen, the pasted code is the fallback.

## Last change
- 2.0.0: complete redesign (race course, ranks and XP, streak, celebrations, sounds), photo tasks, chat-style add bar, new icon. Service worker logbook-v3.
- 1.1.0: sample view, laptop layout, wa.me on laptops, Mac dictation hint, link for Dad's other device.
- 1.0.0: first build.
