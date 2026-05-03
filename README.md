# LocaLore

> Stories from where you stand.

LocaLore is a single-page web app that uses your phone's location to deliver AI-narrated overviews of where you are: history, culture, biome, things to see and do, weather, food, architecture, notable people, language tips, and local customs. Output is read aloud (browser-native voice) or shown on screen.

It's designed to install to your iPhone home screen as a Progressive Web App (PWA) and run like a native app. Hosted free on GitHub Pages.

---

## What's in here

```
localore/
├── index.html            # The whole app — HTML + CSS + JS in one file
├── manifest.json         # PWA manifest (Add to Home Screen metadata)
├── icon.svg              # Source icon
├── icon-180.png          # iOS apple-touch-icon
├── icon-192.png          # PWA icon (small)
├── icon-512.png          # PWA icon (large, "any" purpose)
├── icon-512-maskable.png # PWA icon (Android adaptive)
└── README.md             # this file
```

No build step. No dependencies. Works on any static host.

---

## Deploy to GitHub Pages

1. Create a new repo on GitHub. Public is fine — no API key is in the code.
2. Upload all the files in this folder to the root of the repo.
3. Go to **Settings → Pages**.
4. Under "Build and deployment", set **Source** to *Deploy from a branch*, **Branch** to `main` (or `master`), folder `/ (root)`. Click **Save**.
5. Wait ~30 seconds. GitHub gives you a URL like `https://<your-username>.github.io/<repo-name>/`.
6. Open that URL on your iPhone in **Safari**.

---

## Install on iPhone

1. Open the GitHub Pages URL in **Safari** (not Chrome — only Safari can install PWAs on iOS).
2. Tap the **Share** icon (square with up arrow) at the bottom of Safari.
3. Scroll down and tap **Add to Home Screen**.
4. Tap **Add**.
5. The LocaLore icon appears on your home screen. Tap to launch — it opens fullscreen, no Safari chrome.

The first time you launch it, it asks for your **Anthropic API key** and your **location permission**. Both are stored on the device only.

---

## Get an Anthropic API key

1. Go to [console.anthropic.com](https://console.anthropic.com), sign in.
2. Add a payment method under **Billing**.
3. Go to **API Keys → Create Key**.
4. **Recommended:** create the key inside a **Workspace** (Workspaces tab → New Workspace → name it "LocaLore"). Then create a key scoped to that workspace.
5. **Set a monthly spend limit** on the workspace (e.g. $5/month) so the app can never blow past your budget.
6. Copy the key, paste it into LocaLore on first launch.

### Cost expectations

LocaLore uses **Claude Haiku 4.5** for quick reads and **Sonnet 4.6** for deep dives. Approximate per-request cost:

| Mode | Model | ~Input tokens | ~Output tokens | Cost per request |
|---|---|---|---|---|
| Quick | Haiku 4.5 | 400 | 200 | **~$0.0014** |
| Deep | Sonnet 4.6 | 500 | 1000 | **~$0.017** |

100 quick reads ≈ 14¢. 100 deep dives ≈ $1.70. Cached re-reads of the same place + topic + depth cost $0.

The app's Settings panel shows your local cumulative tokens and estimated cost. Cross-check against your Anthropic Console for the source of truth.

---

## Tracking spend across multiple apps

If you have multiple apps using your Anthropic account:

1. **One workspace per app.** Create a separate workspace in the Anthropic Console for each (LocaLore, Nutri Tracker, etc.). Generate a workspace-scoped API key inside each. Set a per-workspace monthly spend cap.
2. **Console → Usage page.** Filter by workspace or by API key, by date, by model. Export to CSV.
3. **Per-app local stats.** LocaLore tracks its own usage in Settings. Replicate the pattern in your other apps if you want at-a-glance per-device spend.

---

## Privacy & security

- The API key is stored in the browser's `localStorage` on **your device**. It is never uploaded to GitHub, to any server, or shared with other users.
- Your location is sent to **BigDataCloud** (free reverse geocoder, no key needed) to convert coordinates to a place name, and to **Open-Meteo** (for weather). Neither requires identifying information.
- Generated story content is sent to **Anthropic** along with your API key and the place name.
- No analytics, no tracking, no cookies. The app stores only: API key, your prefs, a local usage counter, and cached stories — all on your device.

To wipe everything: in iOS Safari settings → Advanced → Website Data → search "github.io" → remove. Or in the app's Settings panel, clear cache and reset the key field.

---

## Customising

The whole app is in `index.html`. Roughly:

- **Categories:** edit the `CATEGORIES` array at the top of the `<script>` block.
- **Prompts:** edit `buildPrompt()` to change the system instructions.
- **Models:** change the model strings in `callClaude()` (`claude-haiku-4-5`, `claude-sonnet-4-6`, or upgrade to `claude-opus-4-7` if you want flagship quality at higher cost).
- **Colors:** edit the CSS variables in `:root` at the top of the `<style>` block.

---

## Limitations / known quirks

- **iOS audio:** Safari requires a user tap before audio plays. The first "Play" tap may take a beat — that's iOS unlocking the speech engine.
- **No offline LLM:** if you're somewhere without data, only previously-cached stories will work. The cache is per-place + per-topic + per-depth, rounded to ~1km.
- **localStorage limits:** ~5 MB total per origin. Hundreds of cached stories before that's an issue.
- **Geolocation:** indoor accuracy can be poor. The app falls back to manual place entry.
- **First-launch flash:** the Anthropic key prompt appears immediately on first launch. That's expected.

---

## License

Build it, fork it, ship it. No warranty — your API key, your spend.
