# Cafe Corner — Customer Menu + Admin Dashboard

This project is one website with two views:

- Customer menu: `https://YOUR-VERCEL-DOMAIN/`
- Admin dashboard: `https://YOUR-VERCEL-DOMAIN/?admin=1`

## Files

- `index.html` — complete customer ordering site and staff dashboard; keeps the existing Cafe Corner menu and prices.
- `database.rules.json` — Firebase Realtime Database security rules.
- `firebase.json` — Firebase Hosting configuration (optional if deploying with Vercel).
- `.firebaserc` — Firebase project selection for the Firebase CLI.
- `.gitignore` — ignores local Firebase/Node files.

## 1. Firebase project already configured

The website is configured for project `cornercafe-dcc11` and its Realtime Database in `asia-southeast1`.

Firebase web configuration is included in `index.html`. These web config values identify the Firebase project; they are not admin passwords or service-account secrets. Never put a Firebase service-account JSON/private key in this website.

## 2. Create the staff login

1. Open Firebase Console: https://console.firebase.google.com/project/cornercafe-dcc11/authentication/users
2. Confirm **Email/Password** sign-in is enabled.
3. Add your staff/admin email and password under **Users**.
4. Open that user and copy the **UID**.
5. In `database.rules.json`, replace both occurrences of `REPLACE_WITH_ADMIN_UID` with the exact UID.
6. In Firebase Console → Realtime Database → Rules, paste the completed contents of `database.rules.json` and click **Publish**.

Important: do not publish the rules while `REPLACE_WITH_ADMIN_UID` is still present. With the placeholder unchanged, admin read/update/delete access will not work.

These rules let customers create new orders without signing in. Only the configured admin UID can read, update, or permanently delete orders. Basic database rules do not provide rate limiting or bot protection; monitor public orders before going live.

## 3. Deploy on Vercel using GitHub

1. Upload `index.html` to the root of your existing GitHub repository. Also upload `README.md`, `database.rules.json`, `firebase.json`, `.firebaserc`, and `.gitignore`.
2. Commit the changes.
3. In Vercel, open the Cafe Corner project and confirm it is connected to that GitHub repository.
4. Wait for the deployment to finish.
5. Open the customer URL. For staff, open the same URL with `?admin=1` at the end.

No build command is required for this plain HTML website. If Vercel asks, leave Build Command empty and set Output Directory to `.`.

## 4. Test before accepting customers

1. Open the customer website, add an item, and place a test Cash order.
2. Open `/?admin=1`, sign in with the staff account, and confirm the order appears.
3. Test Accept/Preparing/Ready/Completed, order history, sound alert, CSV export, and permanent delete.
4. Delete the test order after checking it.

## 5. PhonePe QR payment

`index.html` still contains `const CAFE_UPI_ID = "YOUR_UPI_ID";` because the actual merchant UPI ID has not been provided. Replace `YOUR_UPI_ID` with the cafe's real UPI ID before enabling PhonePe QR payments. Cash orders can be tested without it. PhonePe QR generation does not verify payment automatically; staff must verify payment in the merchant app before marking it paid.

## 6. Useful URLs

- Firebase project: https://console.firebase.google.com/project/cornercafe-dcc11/overview
- Realtime Database: https://console.firebase.google.com/project/cornercafe-dcc11/database
- Authentication users: https://console.firebase.google.com/project/cornercafe-dcc11/authentication/users

## Notes

- The menu data and prices are carried forward from the Cafe Corner menu file already provided.
- Menu cards currently use category-level image URLs; they are not individual photographs of every dish.
- Admin sound alerts work while the dashboard is open and sound has been enabled in the browser.
