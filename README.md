# Tekk House Chores

A shared chore board for Pia, Alina, Maryam, Naya and Maya. It shows daily dish check-offs and weekly chores that rotate every Sunday night, with the full schedule through Dec 15. Anyone with the link can open it and check things off. No accounts needed.

- **Page:** `index.html`, hosted free on GitHub Pages
- **Shared checkmarks:** stored in a free Firebase Firestore database

## One-time setup (about 10 minutes)

### 1. Create the Firebase database
1. Go to https://console.firebase.google.com and click **Create a project** (any name, e.g. `tekk-house-chores`). You can turn off Google Analytics.
2. In the left menu open **Build → Firestore Database** → **Create database**. Pick a location near you and start in **production mode**.
3. Open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**.
4. Click the gear icon → **Project settings** → under *Your apps* click the **Web** icon (`</>`), give it a nickname, and register it (no Hosting needed).
5. Copy the `apiKey` value it shows into `.env` (see *Where the API key lives* below).

### 2. Put it on GitHub Pages
1. Create a new **public** repository on https://github.com/new (e.g. `tekk-house-chores`).
2. Add the API key as a repository secret named `FIREBASE_API_KEY` (Settings → Secrets and variables → Actions), then push this folder with git.
3. In the repo go to **Settings → Pages** and set *Source* to **GitHub Actions**.
4. After a minute the board is live at `https://<your-username>.github.io/tekk-house-chores/`. Send that link to your roommates.

## Where the API key lives
The Firebase key is kept out of the repo:
- **On your computer:** it goes in `.env`, which is gitignored (copy `.env.example`). Run `./scripts/make-config.sh` to generate `firebase-config.js` for local testing. That file is gitignored too.
- **On GitHub:** it's stored as the repository secret `FIREBASE_API_KEY` (Settings → Secrets and variables → Actions). On every push to `main`, `.github/workflows/deploy.yml` generates `firebase-config.js` from the secret and publishes the site to Pages. Pages must be set to **Source: GitHub Actions**.

The key still ends up in the published page, because browsers need it to reach Firebase. To make it useless anywhere else, restrict it to this site in Google Cloud Console (APIs & Services → Credentials → the Browser key → Websites → `https://piaatal.github.io/*`).

## Good to know
- The security rules decide what can be written, and they only allow checking and unchecking boxes.
- Anyone who has the link can check boxes. Only share it with the house.
- To change chores, names or the end date, edit the `PEOPLE`, `BUNDLES`, `DAILY` and `END` constants near the top of the script in `index.html`.
