# স্বপ্ন সারথী যুব সংঘ — Football Tournament Registration

Mobile-friendly Bengali tournament registration, Firebase admin approval, and player card download.

## Firebase configuration
- `firebase.js` contains the Firebase Web App config.
- Firestore and Email/Password Authentication are required.
- Player photos upload to ImgBB from `app.js`; Firebase Storage is not required.
- The admin account is restricted to the configured Auth UID in both `admin.js` and `firestore.rules`. If changing admin users, update the UID in both files and republish rules.

## Setup
1. In Firebase Console, enable Firestore Database and Authentication → Email/Password.
2. Create the admin user under Authentication → Users. The configured admin UID must match that user's UID.
3. Publish `firestore.rules` in Firestore → Rules.
4. The registration page accepts public submissions and uploads player photos to ImgBB. Keep in mind that the ImgBB key is visible in browser code and photos are public URLs; monitor the key and service quota.
5. Serve over HTTPS or localhost (ES modules do not work from `file://`). For local testing run `python -m http.server 8080` from this folder.

## GitHub upload
Create an empty GitHub repository, then run in this folder:

```bash
git init
git add .
git commit -m "Build tournament registration app"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

Never commit service account keys, private keys, or passwords. Firebase web config is intended for browser use; security is enforced by Firestore rules.

## Workflow
Registrations are written to `registrations` as pending. Admin approve/reject updates the status. Approval publishes a limited `publicCards/{registrationId}` record (player name, team, position, jersey and photo only). Public status/card lookup reads this limited record, not payment information. The registration ID is used as the lookup key.

## Before launch
- Confirm tournament name/year, fees and payment instructions.
- The form records a payment transaction reference; it does not automatically verify payment.
- Test photo upload, submission, admin sign-in, approval, rejection, status lookup and PNG card download. Add App Check/abuse controls before broad public use.
