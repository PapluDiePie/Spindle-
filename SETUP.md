# Spindle setup

## Quick start: put it on your phone as an app

1. Go to https://app.netlify.com/drop and drag the whole **spindle** folder (or unzip `spindle.zip` first) onto the page.
   Sign up for free there so the site doesn't expire.
2. Open the address Netlify gives you on your phone.
3. **iPhone:** in Safari, tap **Share → Add to Home Screen**. **Android:** tap **Install app** in Spindle's top bar, or ⋮ → **Install app** in Chrome.

Spindle then opens full-screen from its own icon, and the status bar takes the colour of the record on the deck.
To install it, the files `manifest.webmanifest`, `sw.js` and the `icon-*.png` images must sit next to `index.html`.
Pasting only `index.html` into CodePen still works for testing, but it can't be installed from there.

To use your Google account, two more things are needed.

## 1. Serve it from a web address

YouTube playback and Google sign-in both refuse to work from a `file://` page.

- **On your computer:** in this folder, run `python3 -m http.server 8000`, then open http://localhost:8000
- **On the web (works on your phone too):** put `index.html` on GitHub Pages, Netlify Drop or Cloudflare Pages.

## 2. Create a Google OAuth Client ID (about 5 minutes, one time only)

Spindle reads your library through Google's YouTube Data API. It runs entirely in your browser and has no server. Google requires a Client ID that you own:

1. Go to https://console.cloud.google.com/projectcreate and create a project (any name).
2. **APIs & Services → Library:** search for **YouTube Data API v3** and click **Enable**.
3. **Google Auth Platform → Branding:** fill in an app name and your email address.
   **Audience:** set it to *External*, leave it in *Testing*, and add your own Google address under **Test users**.
   **Data access:** add the scope `.../auth/youtube.readonly`.
4. **Google Auth Platform → Clients → Create client:**
   - Application type: **Web application**
   - Authorised JavaScript origins: the address Spindle runs at, e.g. `http://localhost:8000` and/or `https://<you>.github.io`
   - Redirect URIs: leave this empty
5. Copy the Client ID (it ends in `.apps.googleusercontent.com`).
6. Open Spindle, click **Connect YouTube**, paste the ID and sign in.
   Google will say the app is unverified. That's expected because it's your own app: click **Continue**.

Spindle only asks for **read-only** access. Your sign-in lasts about an hour, after which you click **Sign in** again.
To change the Client ID later, double-click the account area in the top bar.

## What you get

| Shelf | Where it comes from |
|---|---|
| Your crate | Albums and songs you add by link (stored in this browser) |
| Liked | Songs you've liked on YouTube / YouTube Music |
| One shelf per playlist | Your YouTube Music playlists |
| Trending music | YouTube's music chart for your region |
| Radio · *track* | YouTube's endless mix, started from whatever is playing (**Radio** button) |

## Turntable controls

- **Drag a spine onto the turntable** to play it. The shelf after it plays next. On a phone, press and hold the spine, then drag.
- **Drag the small record next to a shelf name** to play the whole shelf.
- **Play/pause** spins the platter up from stopped, or winds it down.
- **Hold the record** to brake it to a stop. Let go to spin it back up.
- **33 / 45** and the **pitch** fader change speed (double-click the fader to reset it).
- Keyboard: Space = play/stop, ← / → = previous / next track.

## Limits (set by Google, not by Spindle)

- Google's API can't access YouTube Music's personal home feed, *Supermix*, *My Mix* or *Discover Mix*.
  The **Radio** button uses YouTube's public "mix from this song" instead. It's algorithmic, but not personalised to your account.
- YouTube changes speed without changing pitch, and only in steps. So the spin-up and spin-down speed up and slow the music, but it doesn't drop in pitch the way real vinyl does.
  The fade in volume makes up for some of that.
- Some tracks can't play outside YouTube because the label blocks embedding. Spindle skips those.
- The free API quota is 10,000 units a day. Each shelf load costs about 1 unit, so you won't come close.
