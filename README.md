# Epiphan Encoders & Epiphan EC20 — Stream Deck plugins

Control Epiphan **encoders** (Pearl-2, Pearl Mini, Pearl Nano, Pearl Nexus and newer models) and the Epiphan
**EC20** PTZ camera from an Elgato Stream Deck. Keys show live status — recording, streaming, active
layout, audio levels, camera state, live previews — and Stream Deck + dials drive gain, zoom, focus
and pan/tilt.

Two plugins, installed separately:

| Plugin | For | Package |
|---|---|---|
| **Epiphan Encoders** | Epiphan encoders (Pearl-2, Pearl Mini, Pearl Nano, Pearl Nexus and newer) | [`com.epiphan.pearl.streamDeckPlugin`](plugins/com.epiphan.pearl.streamDeckPlugin) |
| **Epiphan EC20** | the EC20 PTZ camera | [`com.epiphan.ec20.streamDeckPlugin`](plugins/com.epiphan.ec20.streamDeckPlugin) |

This repository holds the **installable plugins only** — download, double-click, done.

---

## Requirements

- **Stream Deck app 7.1 or newer** on **Windows 10+** or **macOS 12+** (the plugins run on the Node 24
  runtime the Stream Deck app manages — nothing else to install).
- Any Stream Deck controlled by the desktop app. Dial actions (gain, zoom, focus, pan/tilt, exposure,
  picture, white balance) need a **Stream Deck +**.
- An Epiphan encoder on **firmware 4.24.1 or newer** and/or an EC20, reachable from this computer over the network,
  with an admin login.

## Install

1. Download the plugin you need from the [latest release](../../releases/latest) (or from the
   [`plugins/`](plugins/) folder).
2. Double-click the `.streamDeckPlugin` file. The Stream Deck app asks to install it — confirm.
3. The plugin's actions appear in the Stream Deck actions list under **Epiphan Encoders** or
   **Epiphan EC20**.

Installing a newer version over an older one keeps every key you have already configured.

## Adding devices

Devices are stored once, in the plugin's settings, and shared by every action.

1. Drag any Epiphan action onto a key.
2. In the property inspector, the **Device** drop-down lists saved devices of the right kind. Open
   **Manage devices** underneath it.
3. Enter a **Name**, **Host** (IP or host name, no `http://`), **Port** (leave empty for 80 / 443),
   **HTTPS** (encoders only), **Username** and **Password**.
4. Press **Test** — the plugin checks the login against the device and reports the result.
5. Press **Add device**. It now appears in the Device drop-down of every action of that kind.

**Scan network** finds Epiphan devices on the same IPv4 subnet as this computer and fills in the form
for you. Devices behind a router or VPN must be added by address. The first scan may prompt Windows
Firewall to allow Stream Deck's node process to receive replies — allow it, or discovery finds nothing.

## Device setup

**Encoders.** Use an admin account (or a user allowed to use the REST API) — the *operator* role cannot
control recorders. Plain HTTP is the default and matches the device's factory setting. If HTTPS is
enabled on the encoder, enable **Use HTTPS** for the device. Factory certificates are self-signed, so
leave **Accept self-signed certificate** on unless you installed a CA-signed certificate.

**EC20.** Factory login is `admin` / `admin`. The camera is always addressed over plain HTTP.

## Actions — Epiphan Encoders

"2-state" actions light up while the thing they control is active, so they also work as on/off
indicators in a Multi Action.

| Action | What it does |
|---|---|
| **Recorder** (2-state) | Start, stop, pause, resume or toggle a recorder (or all). Shows recorder name, elapsed time and REC / PAUSED / ERR. |
| **Stream** (2-state) | Start, stop or toggle a channel's stream(s). Badge LIVE / STARTING / ERR with the device's message. |
| **Layout** (2-state) | Switch a channel to a layout; lit while that layout is active. Optionally shows the layout's own picture. |
| **Single Touch** (2-state) | Press the encoder's Single Touch control (record and stream together). Shows how many recorders / streams are running. |
| **Bookmark** | Add a bookmark to the channel's recording, optionally stamped with the time. |
| **Preview** | Live picture of a channel, input or output on the key — or spread across a 2 × 2 / 3 × 3 block of keys (**Mosaic**). |
| **Output Source** | Switch what an HDMI/SDI output shows: multiview, device info, console, a channel or an input. |
| **Apply Preset** | **Hold** to apply a configuration preset. |
| **Event** (2-state) | Control scheduled CMS events: status, start, stop, pause, resume, extend. Blue **CMS** tag, state badge and countdown. |
| **System Status** | CPU load and temperature, storage space, automatic file upload state, or product / firmware / name. |
| **Reboot / Shutdown** | **Hold** to reboot or shut down. Never fires on a tap. |
| **Audio** (dial) | Live stereo VU meter of an input; dial nudges gain or audio delay. |
| **Storage** | Free / total space and state; **hold** to eject removable media. |

## Actions — Epiphan EC20

"Hold" actions move while the key is held and stop on release.

| Action | What it does |
|---|---|
| **Pan / Tilt** (dial) | Move in 8 directions or home; hold or step. Dial: rotate pans, press-rotate tilts, push homes. |
| **Zoom** (dial) | Zoom in / out while held or in steps. Dial: rotate zooms, push runs one-push autofocus. |
| **Focus** (2-state, dial) | Near / far, auto / manual, one-push. Lit in auto. |
| **Camera Preset** | Recall a preset (1–50) and show its snapshot; hold to save the current position; clear needs a hold. |
| **Auto Tracking** (2-state) | Enable AI presenter tracking; tracking or zone mode. |
| **Presenter Select** | Pick the presenter to track when several people are in view. |
| **Preview** | The camera's own live picture on the key — or spread across a 2 × 2 / 3 × 3 block (**Mosaic**). |
| **Tally** (2-state) | Tally light on / off / blink, red or green; the key takes the tally's colour. |
| **White Balance** (dial) | One-push calibration, mode, or nudge colour temperature / gains / saturation / hue. |
| **Exposure** (dial) | Adjust shutter, iris, gain, brightness, compensation, gain limit or DRC; switches to the right exposure mode for you. |
| **Picture** (dial) | Luminance, contrast, sharpness, 3D noise reduction. |
| **Flip** (2-state) | Vertical image flip. |
| **Stream** (2-state) | Enable / disable RTMP 1, RTMP 2 or SRT; badge LIVE / ERR. |

## Troubleshooting

**"Test" fails with 401 / Unauthorized.** Encoders use HTTP Basic auth with the admin account (the
password is blank on a factory device; the *operator* account cannot control recorders). EC20 uses
HTTP Digest auth — handled automatically, but the username is case-sensitive and the default is
`admin` / `admin`.

**Encoder over HTTPS with a self-signed certificate.** Turn on **Use HTTPS** for the device and leave
**Accept self-signed certificate** on (the default). Turn it off only if you installed a CA-signed
certificate on the encoder.

**Keys stay on "Device offline".** Check the host and port, that the device is reachable from this
computer (VPN!), and that the API is enabled. Encoder commands are asynchronous — the key updates on the
next status poll rather than instantly.

**Recorder / stream key does not change state right after pressing.** Expected: the encoder acknowledges
the command before the recorder actually starts. Allow a second or two.

**Nothing works after upgrading the Stream Deck app.** The plugins need Stream Deck 7.1+ and its
bundled Node 24 runtime. Plugin logs are in the Stream Deck app's `Plugins/com.epiphan.*.sdPlugin/logs/`
folder.

## Known limitations

- **Output Source has no live state** — the encoder API can set an output's source but not read it.
- **Encoder actions are asynchronous** — the key follows the next status poll, not the acknowledgement.
- **Previews are JPEG snapshots** polled at the configured interval (minimum 250 ms), not video.
- **Multi Actions** exclude hold-to-confirm actions (Apply Preset, Reboot / Shutdown, Storage eject) and
  pure status displays, because a Multi Action cannot hold a key.
- **Discovery is IPv4 and same-subnet only.** Devices behind a router or VPN, or reachable only over
  IPv6, must be added by address.
- **Digest auth is MD5 only**, as the EC20 implements it.

## Privacy

The plugins talk only to the devices you add, from your computer, and store their addresses and
logins in the Stream Deck app's own settings. Nothing is sent to Epiphan or anyone else.

## Support

Epiphan support: <https://www.epiphan.com/support/> · Product pages: <https://www.epiphan.com>

Problems with the plugins themselves: open an issue in this repository.

## Licence

MIT — see [LICENSE](LICENSE). Bundled third-party components are listed in
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).
