# 🔫 Light Gun Co-op: no-server edition

The whole game is **one file, `dist/index.html`**. There's no laptop and no server.
- The device that shows the game is the **host**: either your TV's browser, or phone 1 cast to the TV. The host runs the game *and* acts as the "server".
- Phones open the same page with a room code from the QR code and connect **directly to the host over your Wi-Fi** (WebRTC).
- A free public matchmaking service (PeerJS) is used only for the first "hello" between devices, so you need internet when joining. All game traffic stays on your Wi-Fi.

---

## Step 1: Put the file online once (free, about 5 minutes)

Phones only allow motion sensors on **https** pages, so the file needs a secure web address. GitHub Pages is free and works from a phone browser too:

1. Create a free account at github.com and tap **+ → New repository**. Name it `lightgun`, set it to **Public**, and create it.
2. Tap **Add file → Upload files** and upload `index.html`.
3. Go to **Settings → Pages**. Set Source to *Deploy from a branch*, choose branch *main* and folder */ (root)*, then save.
4. After about a minute your game is live at `https://<your-username>.github.io/lightgun/`.

Any other static https host works too, for example Netlify or Cloudflare Pages.

---

## Step 2: Pick how to get it on the TV

### Option A: LG TV browser (recommended, no lag)
1. On the LG C4, open the **Web Browser** app and go to your GitHub Pages address. Bookmark it.
2. The QR code appears. **Both phones** scan it, enter a name, tap **JOIN**, point at the center of the TV and tap to calibrate.
3. Press **FIRE** on a phone to start.

Bonus: the **LG Magic Remote** is a pointer, so it can be a third gun. Tap "Use mouse / Magic Remote as a gun" and click to shoot.

> The Fire TV Cube's **Silk Browser** should work the same way, but I haven't tested it. Try it if the LG browser gives you trouble.

### Option B: Pixel 8 Pro casts to the LG TV (Google Cast)
1. On the Pixel, open the game address in Chrome. It becomes the host and shows the QR code.
2. Pull down Quick Settings, tap **Screen Cast**, and pick the LG TV.
3. Tap **"📱 I'm playing on this phone too"**. Hold the Pixel sideways **like you're taking a photo of the TV**, point it at the center of the TV, and tap to calibrate.
4. The S20 scans the QR code from the TV and joins as player 2.
5. On the Pixel, **tap anywhere on the screen to shoot** and shake it to reload.

### Option C: Samsung S20 mirrors to the Fire TV Cube (Miracast)
1. On the Fire TV, go to **Settings → Display & Sounds → Enable Display Mirroring**.
2. On the S20, open the game in Chrome, then use **Smart View** from Quick Settings and pick the Fire TV.
3. Continue as in Option B, with the S20 as player 1 and the Pixel joining by QR code.

The Pixel can't mirror to the Fire TV because it doesn't support Miracast.

**About options B and C:** screen mirroring adds roughly 0.1–0.3 s of delay. When phone 1 is the host, the game automatically makes enemies slower and bigger to compensate. Still, Option A feels noticeably snappier.

---

## Controls
| | Controller phones | Phone 1 (when it's the host) |
|---|---|---|
| Aim | Point the phone (remote grip or camera grip) | Camera grip, pointing at the TV |
| Shoot | Tap FIRE | Tap anywhere on the screen |
| Reload | Shake, or the Reload button | Shake |
| Re-center | 🎯 button | 🎯 button, bottom right |

## Troubleshooting
- **"Game not found" on a phone:** the host page must be open. Rescan the QR code. The room code stays the same when you reload the TV page.
- **Phone shows "connection problem":** check that everyone is on the *same* Wi-Fi. Guest networks and routers with "AP/client isolation" block devices from talking to each other.
- **No aiming:** the page must be opened via **https** (the GitHub Pages address).
- **Crosshair drifts:** tap Re-center. Adjust "Aim speed" if it moves too much or too little for your TV size.
- **No sound on the TV:** browsers block sound until you press something. Click anything once with the Magic Remote.

## For developers
- `src/index.html` is the source. `npm install && npm run build` inlines PeerJS and the QR library into `dist/index.html`.
- To run fully offline, run your own PeerJS broker (`npx peerjs --port 9000`) and add `?peerhost=<ip>&peerport=9000` to the URL.
- The host page holds all game state and player slots. Controllers send `aim`, `fire` and `reload`. The host replies with `ammo`, `score`, `hit`, `kill`, `miss`, `empty`, `reloaded`, `hurt` and `state`.
