# B-Launcher — Setup Guide

For people who already have the app files — no building, no GitHub, no code.
You should have two things before starting:

- **The `B-Launcher` folder** (containing `B-Launcher.exe`) for the Windows PC
- **`BLauncherRemote-unsigned.ipa`** for the iPhone

You'll also need:
- A free **Apple ID** (a throwaway/secondary one is fine — it never touches
  your main Apple account's purchases or payment info)
- A **USB cable** to plug the iPhone into the PC, once

---

## Step 1 — Run B-Launcher on the PC

Unzip the `B-Launcher` folder somewhere permanent (not Downloads — it writes
its settings next to itself) and run `B-Launcher.exe`.

Windows may show a SmartScreen warning since the app isn't code-signed —
click **More info → Run anyway**.

The first time it runs, go to **Settings → Add Profile** and point it at
your `blender.exe`, so it knows what to launch.

Running the exe again while it's already open just brings the existing
window to the front — it won't start a second copy.

---

## Step 2 — Install Tailscale (on both the PC and the phone)

The PC and the phone need to be able to find each other privately over the
internet, and Tailscale is what does that — it's a free, encrypted private
network that only your own devices join.

**On the PC:**
- B-Launcher notices Tailscale isn't installed and offers to install it the
  first time you run it — accept the prompt, then sign in through
  Tailscale's own window when it opens.
- Already have Tailscale? It's picked up automatically, nothing to do.
- Prefer to do it yourself: [tailscale.com/download](https://tailscale.com/download).

**On the iPhone:**
- Install **Tailscale** from the App Store.
- Sign in with the **exact same account** you used on the PC.

That's it — once both are signed in, the PC and phone can reach each other
privately without any port forwarding, static IPs, or router configuration.

---

## Step 3 — Sideload the app onto your iPhone

Apple doesn't allow app installs outside the App Store without either a paid
developer account or a tool like **Sideloadly**, which signs the app with a
free Apple ID instead.

1. Download and install **Sideloadly**: [sideloadly.io](https://sideloadly.io)
2. If Windows doesn't already recognize your iPhone over USB, install
   **Apple Devices** from the Microsoft Store (or install iTunes) — this
   gives Windows the driver it needs.
3. Plug the iPhone into the PC with a cable, unlock it, and tap **Trust
   This Computer** if it asks.
4. Open Sideloadly and drag `BLauncherRemote-unsigned.ipa` into it.
5. Sign in with your Apple ID when Sideloadly asks, then click **Start**.
   It signs the app and installs it — this can take a minute or two.

If B-Launcher is also open on the same PC, its **Send App to iPhone**
button does the same thing with a couple of extra conveniences: it tells you
whether the phone is actually detected over USB, remembers where Sideloadly
is installed, and puts the `.ipa`'s file path on your clipboard in case you
need to paste it into Sideloadly by hand.

### First open on the phone
iOS will likely refuse to open the app the first time, calling it an
**"Untrusted Developer"**. Fix it once:
**Settings → General → VPN & Device Management** → tap the developer
profile tied to the Apple ID you signed in with → **Trust**.

### The 7-day catch
Apps signed with a **free** Apple ID stop opening after **7 days** (a paid
Apple Developer account, $99/year, extends that to a year). When it expires,
just repeat step 4–5 with the same `.ipa` — your saved servers and settings
inside the app aren't affected, since none of that depends on the signature.

---

## Step 4 — Connect the app to the PC

1. On the PC, open B-Launcher and go to **Connection Info**. It shows a QR
   code (plus the machine name, host, port, and access token underneath, in
   case you'd rather type it in).
2. On the phone, open **B-Launcher Remote**, tap **+**, then **Scan QR
   Code**, and point the camera at the PC's screen. It fills in the whole
   form for you.
3. Tap **Test Connection**, then **Save**.

You should now see the PC's running Blender instances (if any) on the
dashboard, which refreshes automatically every few seconds. From there you
can start renders, stop/pause/resume, and use the **Tools** tab for scene
selection, the render queue, history, and scheduling.

If you use more than one PC, repeat Steps 1–2 and 4 for each — they all
need to be signed into the **same Tailscale account** to show up as
options when splitting a render across machines.

---

## Optional — Render from inside Blender (BetterLauncher)

B-Launcher can install a Blender add-on that lets you send the file you're
working on straight to another machine, without switching windows.

1. In B-Launcher, click **Install Blender Add-on…**
2. Tick the Blender versions you want it in (or **All profiles**) and click
   **Install**. Each Blender opens briefly in the background while it's set
   up — that's expected.
3. Open Blender, press **N** in the 3D viewport, and pick the **Better
   Blender** tab.

Your machines are already filled in, so just choose one from the dropdown,
press the refresh icon to connect, and use **Render on Machine** or **Add to
Queue**.

By default it sends a copy of your `.blend` over the network to that machine's
**base folder** — the one set in B-Launcher's main window. If both machines
can already see the same shared drive, untick "Send this file to the machine"
and it'll open the file in place instead. Blender saves your file
automatically before sending it.

---

## Optional — Notifications with the app closed

The app can only alert you while it's open on screen — Sideloaded apps
aren't allowed real push notifications from Apple. To get notified when
your phone is locked or the app's in the background, B-Launcher relays
through **ntfy**, a free notification service:

1. Install **ntfy** from the App Store.
2. On the PC, in B-Launcher, open **Phone Notifications**, tick **Send
   notifications to my phone**, then scan the QR code shown there with the
   ntfy app.
3. Click **Send Test** to make sure it arrives.

You can choose which events you want to hear about — renders finishing,
crashes, out-of-memory, or frames landing in the wrong folder.

---

## Optional — AI crash diagnosis

If a render crashes, B-Launcher can explain the crash log in plain English
using [Groq](https://console.groq.com/keys) (free). Get an API key from
that link, then paste it into B-Launcher's **AI Settings**. The key stays
on your PC — it's only ever sent to Groq itself, never anywhere else.

---

## Troubleshooting

| Problem | Likely fix |
|---|---|
| iPhone won't open the app ("Untrusted Developer") | Settings → General → VPN & Device Management → trust the profile |
| App on the phone can't find/connect to the PC | Confirm both are signed into Tailscale with the **same account**, and that B-Launcher is actually running on the PC |
| Sideloadly doesn't see the iPhone | Unlock the phone and tap Trust; if that doesn't fix it, install/reinstall the Apple Devices app for the USB driver |
| App on the phone stopped opening after about a week | The free signature expired — reinstall via Sideloadly with the same `.ipa` |
| Windows blocks `B-Launcher.exe` on first run | Click "More info" → "Run anyway" in the SmartScreen prompt |
| No "Better Blender" tab in Blender's N panel | Enable it in Blender's Preferences → Add-ons (search "BetterLauncher"), or re-run Install Blender Add-on… |
| The add-on says the token was rejected | Re-run **Install Blender Add-on…** — it rewrites the connection details |
