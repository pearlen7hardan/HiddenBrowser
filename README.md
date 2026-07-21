# HiddenBrowser

> Anti-detect browser launcher for **web scraping** and **multi-accounting**.
> Engine-level fingerprint privacy in **Chromium 149** (WebGL / WebGPU / Client
> Hints / fonts / TLS), **170+ device profiles** bundled, stable **QUIC + WebRTC
> over SOCKS5**.

<p align="center">
  <img alt="Platforms" src="https://img.shields.io/badge/macOS%20·%20Windows-supported-4ade80?style=flat-square">
  <img alt="Engine" src="https://img.shields.io/badge/Chromium-149-a78bfa?style=flat-square">
</p>

<p align="center">
  <img src="site/assets/00-launcher-workspace.png" alt="HiddenBrowser Launcher" width="820">
</p>

---

## What it is

Run hundreds of isolated browser identities side by side, each one a fully-formed
device with its own GPU, screen, fonts, audio stack, timezone, locale,
WebGL/WebGPU caps, TLS ClientHello, UA-CH, WebRTC policy, geolocation and cookies
— every signal coherent with the others, and every signal **masked inside
Chromium's C++ engine** (Blink / V8 / network stack), not via JS injection that
detectors trip on instantly.

You get 170 ready-made device profiles out of the box (mac M1–M5, Windows
desktops/laptops with RTX/GTX/Intel/AMD GPUs), bind a SOCKS5 / HTTP proxy to each
one, and the launcher handles the rest — auto-resolved timezone + locale +
geolocation from the proxy's exit country, isolated `user-data-dir`, persistent
cookies, Widevine pre-warm, QUIC over the proxy's UDP relay, no real-IP leaks via
WebRTC.

### Drive it four ways (same on-disk state, no sync step)

- **Desktop UI** — workspace for day-to-day work (profiles, proxies, cookies, fingerprint editor).
- **Local HTTP API** — Bearer-JWT on `127.0.0.1:40325`; create / start / stop profiles and grab a CDP endpoint from any language.
- **MCP server** — drops into Claude Desktop / Cursor for natural-language profile orchestration (HTTP API + browser-over-CDP).
- **Standalone SDKs** — Python, Node & Rust libraries that ship the engine themselves and need no GUI at all; ideal for scrapers / CI / servers.

---

## What you get out of the box

| Test | Result |
| --- | --- |
| [browserleaks.com/quic](https://browserleaks.com/quic) | QUIC `True`, JA4 matches real Chrome, MTU 1232 over SOCKS5 UDP relay |
| [fingerprint.com](https://fingerprint.com/demo) | Bot / VPN / DevTools / browser-tampering all `Not detected` |
| [browserscan.net](https://www.browserscan.net) | Authenticity **100%** |
| [pixelscan.net](https://pixelscan.net) | Fingerprint **consistent**, no proxy / automation detected |
| [fp.haru.gay](https://fp.haru.gay) | `isBot: false`, every sub-signal `false` |
| [antcpt.com/score_detector](https://antcpt.com/score_detector/) | reCAPTCHA v3 score **0.9** |
| [networktest.twilio.com](https://networktest.twilio.com) | TURN UDP / TCP / TLS + Voice — all **Pass** (no real-IP leak) |

---

## Fingerprint surfaces patched

All overrides live inside the browser engine — there is no JavaScript shim layer
that detectors can spot, so masked values are consistent across iframes, web
workers, devtools and headless inspection.

- **Device identity** — user agent, platform, vendor, CPU cores, RAM, touch points, full Sec-CH-UA stack (brand, version, architecture, bitness, mobile, model) with stable GREASE.
- **Graphics** — WebGL renderer / vendor / extensions / limits, WebGPU adapter + limits, deterministic per-profile noise for Canvas, DOMRect and ClientRects, color gamut and HDR claims.
- **Audio** — sample rate, channel count, optional per-profile noise on raw audio samples.
- **Screen & window** — full resolution + available area + DPR + color depth, max-size cap so the OS won't resize past the claimed dimensions.
- **Locale** — timezone, ICU locale, primary language and the Accept-Language header auto-derived from the bound proxy's country.
- **Geolocation** — coordinates set manually or derived from the proxy's exit IP; host GPS / Wi-Fi is never used.
- **Network capability** — connection type, downlink, RTT, save-data, storage quota, JS heap limit, battery state, media-device counts.
- **TLS ClientHello** — Chrome-149 cipher + signature-algorithm selection, extension shuffling, so JA4 / Akamai / Peetprint fingerprints match real Chrome.
- **HTTP-3 over the proxy's UDP relay** — QUIC works end-to-end through SOCKS5; origin hostnames are resolved proxy-side.
- **WebRTC policy** — `block` / `tcp_only` / `auto`. In `auto` traffic rides the proxy's UDP relay; otherwise WebRTC candidates report the proxy exit IP, never the host. STUN / TURN targets on private networks are dropped.
- **Speech voices** — full per-OS `speechSynthesis.getVoices()` enumeration (200+ macOS voices, SAPI + Google for Windows).
- **Fonts** — system font enumeration pinned to a per-profile set so font-list probes return the claimed device's fonts, not the host's.
- **Google validation headers** — the headers real Chrome adds to Google properties (notably `x-client-data` — its absence is the loudest reCAPTCHA bot signal) are reproduced correctly.
- **WebAuthn** — platform-authenticator availability matches the claimed device.
- **Hardening** — Widevine pre-warmed per profile, headless markers stripped, devtools-protocol side-channels closed, sync hard-disabled, no keychain prompts, no Google account telemetry, no Privacy Sandbox enrollment leaked.

---

## Launcher features

- **Profile workspace** — per-profile `user-data-dir`, persistent Chrome sessions ("Continue where you left off" without the crash-restore bubble), bulk import, folder / tag organisation, pin to top, clone.
- **Fingerprint library** — 170 starter profiles shipped via CDN. Profile editor randomises CPU / RAM / platform-version when you change the GPU.
- **Proxy manager** — SOCKS5 / HTTP / HTTPS, bulk paste-import, per-proxy live test (TCP + UDP_ASSOCIATE probe + geo lookup), bind by id or inline-on-launch. Auto-resolves timezone / locale / geolocation from the proxy's exit country.
- **Auto-runtime** — first launch pulls the patched Chromium build, Widevine CDM and the fingerprint library from CDN, then subsequent launches are zero-network.
- **Local automation API** — axum HTTP server on `127.0.0.1`, JWT-Bearer auth.
- **MCP server bundled** — drop into Claude Desktop / IDE for natural-language profile orchestration.
- **Cookie I/O** — import / export the profile's Chromium Cookies SQLite (v10 on macOS, AES-GCM + DPAPI on Windows).
- **Cross-platform** — macOS arm64, Windows x64; native traffic lights on mac, custom titlebar on Windows.

---

## Screenshots

**Network — QUIC + WebRTC over SOCKS5**

| browserleaks.com/quic — QUIC `True`, JA4 matches Chrome 149 | networktest.twilio.com — every probe `Pass` |
| --- | --- |
| ![QUIC](site/assets/01-browserleaks-quic.jpg) | ![Twilio](site/assets/04-twilio-webrtc.jpg) |

**Bot / automation detection**

| fingerprint.com — `Not detected` | fp.haru.gay — `isBot: false` |
| --- | --- |
| ![FP](site/assets/03-fingerprint-com.jpg) | ![Haru](site/assets/07-haru-bot-detect.jpg) |

**Fingerprint consistency & authenticity**

| pixelscan.net — **consistent** | browserscan.net — Authenticity **100%** |
| --- | --- |
| ![Pixelscan](site/assets/06-pixelscan.jpg) | ![Browserscan](site/assets/05-browserscan.jpg) |

---

## Comparison with other anti-detect browsers

All are patched Chromium forks — the differentiation is in *which* surfaces each
one bothers to patch, *how cleanly*, and what's wrapped around the engine.

| Feature | HiddenBrowser | CloakBrowser | Multilogin / AdsPower / Dolphin |
| --- | --- | --- | --- |
| WebGPU privacy (`navigator.gpu` adapter + limits) | ✅ full | ❌ host GPU leaks | ✅ full |
| Client Hints (Sec-CH-UA-* + GREASE) | ✅ full | ❌ partial | ✅ (❌ Dolphin) |
| Font enumeration pinned per profile | ✅ system-level | ❌ JS-only (host fonts leak) | ⚠️ partial |
| V8 / CDP side-channel hardening | ✅ closed | ❌ open (CDP detectable) | ⚠️ partial |
| TLS ClientHello (JA4) | ✅ matches Chrome 149 | ⚠️ static / drifts | ✅ matches fork |
| QUIC / HTTP-3 over SOCKS5 | ✅ stable via UDP relay | ⚠️ unstable, drops | ❌ disabled with proxy |
| WebRTC over SOCKS5 (no real-IP leak) | ✅ relay or synth candidates | ⚠️ unstable | ⚠️ disable-only |
| Profile coherence (GPU ↔ CPU ↔ RAM ↔ UA ↔ fonts) | ✅ coherent | ❌ frequent contradictions | ⚠️ varies |
| Bundled fingerprint library | 170 real-device profiles | ❌ random generator | catalog (subscription) |
| Management UI | ✅ desktop app | ⚠️ CLI only | ✅ desktop app |

Public "is my browser human?" checkers don't probe most of the surfaces above, so
a weak anti-detect can still light up all green on them. Real production
anti-fraud stacks do check them — that's exactly where accounts get flagged a few
sessions in instead of immediately. HiddenBrowser closes those surfaces at the
engine level.

---

## Download & install

Grab the build for your OS from **[Releases](../../releases)** — `.dmg` for macOS
(Apple Silicon), `.msi` or the installer-free **portable** `.exe` for Windows —
and run it.

The builds aren't code-signed, so the first launch is gated by the OS:

- **macOS** — Gatekeeper blocks the unsigned bundle (*"can't be opened…"* or *"is
  damaged"* on macOS 14+). Strip the quarantine flag once in Terminal, then it
  opens with a double-click:
  ```bash
  xattr -dr com.apple.quarantine "/Applications/HiddenBrowser Launcher.app"
  ```
- **Windows** — SmartScreen shows *"Windows protected your PC"* → **More info** → **Run anyway**. The portable `.exe` just runs (needs the Evergreen WebView2 runtime, preinstalled on Windows 10/11).

### First launch & activation

On first launch HiddenBrowser asks for a **license key**, then downloads the
patched engine (~150 MB), Widevine (~16 MB) and the fingerprint library from CDN
(one time, cached afterward). After activation you're ready to bind a proxy and
launch your first profile.

---

## Security & VirusTotal

The builds are **not code-signed** (no Apple Developer ID or Windows Authenticode
certificate), so macOS Gatekeeper and Windows SmartScreen warn on first launch —
that's a warning about an **unknown publisher**, not about what's inside the file.
Every release is scanned on VirusTotal so you can verify it yourself:

- **macOS (`.dmg`)** — [VirusTotal report](https://www.virustotal.com/gui/file/2c36c19c5904caab945cbd447d4dd3f5bb97d3b88bffc6f30ddd387f58f64fa4)
- **Windows (`.msi`)** — [VirusTotal report](https://www.virustotal.com/gui/file/948a71cb2d9de5c981d5a16682e0e278eb2fcd595b83de0f5026c6f44a544ab9)
- **Windows (`.exe`)** — [VirusTotal report](https://www.virustotal.com/gui/file/a1660357b2200c3a9e68086258db287b0d16cca0f0ada41d86912e139677a599)

**About the "potentially unwanted" flag.** A few antivirus engines heuristically
tag anti-detect / fingerprint / browser-automation tools as **PUA — a Potentially
Unwanted Application**. That's a *category* label, not detected malware: the app
routes traffic through proxies and modifies browser fingerprints, which matches
those engines' generic heuristics, and an unsigned build from a new, unknown
publisher makes them extra cautious. It does nothing malicious — open the reports
above and judge for yourself.

---

## Usage

Four interchangeable ways to drive HiddenBrowser — all read from the same on-disk
state, so a profile created in the UI is reachable from the API, MCP and SDKs with
no sync step.

### 1. Desktop UI

Add a proxy (*Proxies → Add proxy* — paste `socks5://user:pass@host:port` or
bulk-paste, hit *Test* for a TCP + UDP_ASSOCIATE + geo probe), bind it to a
profile, hit *Start*. The launcher resolves timezone / locale / geolocation from
the proxy's exit country, decides QUIC + WebRTC policy from a live UDP probe, and
keeps each profile's `user-data-dir` isolated and persistent.

### 2. Local automation API

axum HTTP server on `127.0.0.1:40325` (port in *Settings → Automation API*).
Every endpoint except `GET /health` needs the Bearer JWT shown in Settings.

```bash
TOKEN="<from Settings → Automation API>"
BASE="http://127.0.0.1:40325"

# Start a profile in CDP mode — returns the browser's DevTools websocket.
curl -s -X POST "$BASE/profiles/win-rtx4060/start?cdp=true&headless=false" \
     -H "Authorization: Bearer $TOKEN" | jq .
# → {"id":"win-rtx4060","cdp_url":"ws://127.0.0.1:53217/devtools/browser/…","pid":48211}

curl -s -X POST "$BASE/profiles/win-rtx4060/stop" -H "Authorization: Bearer $TOKEN"
```

Reuse the `cdp_url` with any CDP client (puppeteer, raw WS, patchright, your own).
Endpoints cover profiles, proxies, fingerprints, folders and cookies.

### 3. MCP server

A [Model Context Protocol](https://modelcontextprotocol.io) server for Claude
Desktop, Cursor and any MCP client — wraps the launcher's HTTP API and
browser-over-CDP (via patchright) so a model can manage profiles/proxies and
navigate / click / type / screenshot in a live profile. Download it from *Settings
→ MCP server*.

---

## Terms

The **browser engine** (the patched Chromium 149 binary the launcher downloads on
first run) is proprietary. Reverse engineering, redistributing a modified engine,
or bundling it into another product are **not permitted**. Personal use, web
scraping, multi-accounting and integration with the automation API are fine.

---

## Links

- **Site** — [HiddenBrowser](https://hiddenbrowser.org)
- **Docs** — [docs](https://pearlen7hardan.github.io/HiddenBrowser/docs.html)
