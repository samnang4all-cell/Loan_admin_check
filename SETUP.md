# Setup Instructions

You need to configure **3 values** in `index.html` before uploading to GitHub.
Open the file and look for the `CONFIG` block near the top.

---

## Step 1 — Create a free JSONBin account

1. Go to https://jsonbin.io and sign up (free tier is plenty).
2. After signing in, click **Create a Bin**.
3. Paste this starter content into the bin and save:

   ```json
   { "plans": [] }
   ```

4. Copy the **Bin ID** (shown in the bin URL after `/b/`, e.g. `65f8a1c2e41b4d34e8d4b8c7`).
   - Paste it into `JSONBIN_BIN_ID` in the config.

5. Go to **API Keys** in your JSONBin account → copy the **X-MASTER-KEY**.
   - Paste it into `JSONBIN_MASTER_KEY` in the config.

---

## Step 2 — Set your admin password (as a SHA-256 hash)

You'll store the **hash** of your password, not the password itself.

### Easiest way — use the browser console

1. Open any webpage and press **F12** (DevTools).
2. Go to the **Console** tab.
3. Paste this, replacing `your-password-here` with your chosen password:

   ```js
   crypto.subtle.digest("SHA-256", new TextEncoder().encode("your-password-here"))
     .then(b => console.log(Array.from(new Uint8Array(b)).map(x => x.toString(16).padStart(2, "0")).join("")))
   ```

4. Press Enter. You'll see a long hex string like `5e884898da280471...`.
5. Copy that string and paste it into `PASSWORD_HASH` in the config.

### Alternative — use an online generator

Search "SHA-256 generator", paste your password, copy the hex output.

> ⚠️ Use a **strong** password (12+ characters, random). Since this is a static
> site, the hash is visible in the source — a weak password could be brute-forced.

---

## Step 3 — Upload to GitHub Pages

1. Create a new public repository on GitHub (e.g. `payment-planner`).
2. Upload `index.html` to the root.
3. Go to **Settings → Pages**.
4. Under "Build and deployment", set:
   - Source: **Deploy from a branch**
   - Branch: **main** (or `master`), folder **/ (root)**
5. Click **Save**. Your site will be live at:
   `https://<your-username>.github.io/<repo-name>/`

   (Usually live within 1–2 minutes.)

---

## How it works

- **Visitors** → see the page, can view plans and schedules, can copy schedules. No edit buttons shown.
- **You** → click **Admin login**, enter your password. Now all the edit/save/add/delete buttons appear.
- When you click **Save changes**, your edits are pushed to your JSONBin and become visible to everyone.
- Login persists only for your browser tab session — closes when you close the tab.

---

## If something goes wrong

- **"⚠ Save failed: 401"** → wrong/missing JSONBin master key.
- **"⚠ Save failed: 404"** → wrong bin ID.
- **Page shows default plans, not your edits** → bin ID/key isn't configured, or the bin is empty.
- **"Incorrect password"** but you're sure it's right → the hash in `PASSWORD_HASH` doesn't match what you entered. Re-generate the hash and double-check there are no extra spaces.

---

## Rotating credentials later

- **Change password** → generate a new hash, replace `PASSWORD_HASH`, push to GitHub.
- **Rotate JSONBin master key** → in JSONBin → API Keys → regenerate, replace `JSONBIN_MASTER_KEY`, push to GitHub.

Do this immediately if you ever suspect the keys were leaked.
