# Set up your live poll without installing software

You need a GitHub account, a Google account, and a browser. Allow time to test before the meeting. The free Firebase Realtime Database plan allows **100 simultaneous connections total**, including the presenter and any extra browser tabs. Plan for at most 99 attendee connections with one presenter, and leave some room for reconnects. If you expect 100 attendees at once, this free plan is too small. Firebase may also limit bursts of anonymous sign-ups from the same network; test using the venue network. If a limit is reached, attendees may be unable to connect.

## 1. Create the Firebase project

1. Visit https://console.firebase.google.com/ and sign in with your Google account.
2. Select **Create a project** and name it, for example, `meeting-poll`. Follow the prompts. Analytics is not needed. Keep the free **Spark** plan; do not add billing.
3. In the project overview, select the web icon `</>` to add a web app. Give it a name. Firebase Hosting is not needed. Register the app and leave the Firebase configuration screen open. You will copy the configuration later.
4. Go to **Build → Authentication → Get started → Sign-in method**. Enable **Anonymous** and **Email/Password**. Participants are signed in anonymously in the background; they do not create accounts or enter credentials.
5. Under **Authentication → Users**, select **Add user** and create your presenter email and a strong password. Copy the user's **User UID** from the list; you need it for the security rules. Keep your password private.
6. Go to **Build → Realtime Database → Create database**, choose a location, and start in **locked mode**. Note the database URL displayed at the top of the Data tab, e.g. `https://your-project-default-rtdb...firebaseio.com` or `https://your-project...firebasedatabase.app`.
7. Open the **Rules** tab. Open `database-rules.json` from this folder in a text editor, copy its entire contents into the rules editor, and replace `REPLACE_WITH_PRESENTER_UID` with the UID copied in step 5. Click **Publish**. The rules let the presenter manage the poll and each participant submit one answer per question. Responses are readable by poll participants; do not collect sensitive information.

## 2. Add your Firebase configuration

1. In Firebase, open **Project settings → General**, scroll down to your web app, and find the `firebaseConfig` object. You can return to it at any time.
2. Open `config.js` from this folder in a plain text editor such as Notepad. Copy the entries inside Firebase's `firebaseConfig = { ... }` into the braces in this file. Do not paste the surrounding `const firebaseConfig =` or script tags.
3. Ensure there is a `databaseURL` entry using the URL from step 1.6, even if Firebase's snippet does not include it. For example, put a comma after the preceding entry and add `databaseURL: "https://YOUR-DATABASE-URL"` inside the braces. Save the file. Firebase's web configuration is intended to be public; your security rules protect the data. Never put your presenter password in the file.

## 3. Publish with GitHub Pages

1. At https://github.com/new, create a new **public** repository, for example `meeting-poll`. GitHub Free supports Pages for public repositories. Public means anyone can see the files, so do not include confidential information.
2. On the repository page, select **Add file → Upload files**. Drag in `index.html`, `style.css`, `app.js`, and your edited `config.js` from the `github-poll` folder. Upload them directly at the repository root, not inside another folder. Select **Commit changes**.
3. Go to **Settings → Pages**. Under **Build and deployment**, select **Deploy from a branch**, choose `main` and `/ (root)`, then save. Wait a few minutes and refresh; GitHub will display your site address.
4. In Firebase **Authentication → Settings → Authorized domains**, add your GitHub Pages domain, normally `YOUR-USERNAME.github.io` (only the domain, not `https://` or the repository path).

## 4. Set up and run the meeting

1. Open your GitHub Pages address with `?presenter` appended, for example `https://YOUR-USERNAME.github.io/meeting-poll/?presenter`.
2. Sign in with the presenter email and password created above. Open **Edit poll & reset votes**, enter your title, five questions, and four answers each, then save. Saving starts a new poll; previous responses are no longer displayed.
3. Show the presenter screen to your audience. The QR code and link open the participant page. Participants need no login. They should use one browser tab each.
4. Press **Q1** to open the first question. Results update automatically on the presenter screen. Press **Close voting**, then choose the next question. Repeat for all five.
5. Test using several phones and the actual meeting network beforehand. On the free plan, avoid approaching 100 connected devices or tabs. Firebase can reject connections at the limit. If QR code doesn't display, use the link; the QR library is loaded from cdnjs.

## Privacy and branding

The included HP-style mark is a text placeholder, not an official asset, and Arial is a fallback. Replace them with approved HP assets and licensed fonts. The site is public; questions and anonymous votes are visible to connected users and the repository files are public. Votes are linked to anonymous Firebase identities, not names; this is a friendly poll, not a verified election. Participants can potentially vote again using another browser or after resetting their browser data. Old responses remain in Firebase until you delete them in the database console. Check your organization's rules before using personal accounts or sharing meeting content.

