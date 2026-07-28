# Firebase cloud sync setup

This page can sync records through Firebase Cloud Firestore while still working locally when Firebase is not configured.

## Create the free Firebase project

1. Open https://console.firebase.google.com/ and create a project.
2. Use the Spark plan.
3. Add a Web app and copy the `firebaseConfig` object.
4. In `index.html` and `versions/splitwise-retro/index.html`, replace the empty `firebaseConfig` values.

## Enable Firestore

1. In Firebase, open Firestore Database.
2. Create a database.
3. Start in production mode, then publish rules like this:

```txt
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /splitwiseLedgers/trip-ledger/entries/{entryId} {
      allow read, write: if true;
    }
  }
}
```

These rules make this one shared ledger publicly writable. For a private ledger, use Firebase Authentication or a less guessable ledger id and update `cloudLedgerId` in the page.

## Deploy

After filling the config, commit and push to GitHub Pages. Everyone opening the same page will see synced entries in real time.
