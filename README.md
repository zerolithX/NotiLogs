# NotifLogger

A minimal Android app that logs every notification posted by every app on
your phone (app name, title, text, and timestamp), keeps a permanent local
history, and can export that history as an Excel-compatible `.xls` file.

## How it works

- `NotifListenerService` uses Android's `NotificationListenerService` API to
  observe notifications system-wide. This requires the user to manually
  grant "Notification access" in system settings (the app has a button that
  opens that settings screen directly).
- Every notification is written to a local SQLite database (`DbHelper`), so
  the log persists across app restarts and reboots.
- `MainActivity` shows a minimal scrolling list of logged notifications,
  newest first.
- The **export** button writes an HTML table saved with an `.xls`
  extension. Excel, Google Sheets, and LibreOffice Calc all open this
  format natively as a real spreadsheet — this avoids bundling a heavy
  spreadsheet library (like Apache POI) that doesn't play well on Android.
  The file is written to app-private external storage and shared via a
  `FileProvider` share sheet so you can save it, email it, or drop it in
  Drive.
- The **clear** button wipes the local log.

## Building it yourself with GitHub

1. Push this whole folder to a new GitHub repository (keep the folder
   structure as-is — `.github/workflows/android-build.yml` must stay at
   that path).
2. GitHub Actions will automatically run on every push to `main`/`master`,
   or you can trigger it manually from the **Actions** tab
   ("Run workflow").
3. When the build finishes, open the workflow run and download the
   `notiflogger-debug-apk` artifact — that's your installable APK.
4. Sideload it onto your phone (you'll need to allow installs from unknown
   sources), open the app, tap **Grant Notification Access**, and enable
   NotifLogger in the list that opens.

## Building locally (optional)

If you have Android Studio installed, just open this folder as a project
and run it — no extra setup needed, there are no external dependencies
beyond standard AndroidX/Material libraries.

## Notes / things to know

- `minSdk 24` (Android 7.0+), `targetSdk 34`.
- No network permissions are requested — everything stays on-device until
  you choose to export/share.
- Notification access is a sensitive permission; Android will show a
  system warning when you grant it. That's expected — it's what lets the
  app see notifications from other apps.
- The app icon is generated from the image you provided.
