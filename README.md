# GT3RS-ALP-R8-Rally-Repo 🏎️

> **A practical guide to radar/laser defense, crowdsourced law-enforcement awareness, and multi-device navigation — built on a Porsche GT3RS, fully adaptable to any performance car.**

---

## ⚡ Quick Start (New Here? Start Here)

If you're new to this stack, do these five things first — everything else in this doc is refinement on top of this foundation.

1. **Install Highway Radar** (Android only) — free on Google Play. This is the core app that connects to your radar detector via Bluetooth and manages all alerts.
2. **Install WzSabre** — free plugin from [wzsabre.rocks](https://wzsabre.rocks). Feeds crowd-sourced police/hazard reports into Highway Radar. After installing, **disable battery optimization** for both WzSabre and Highway Radar (Settings → Apps → [app] → Battery → Unrestricted).
3. **Pair your radar detector** via Bluetooth — see the pairing steps in the [Radar Detector Integration](#️-highway-radar-app--uniden-r8-integration) section below. If you're not running a Uniden R8, see the [Other Detectors](#-other-detectors) note.
4. **Set up split screen** (Android) — run Highway Radar on top, Waze on the bottom. On a standard phone, use PIP (picture-in-picture) mode instead. See the [Navigation Stack](#️-navigation-stack--multi-device-setup) section.
5. **Connect to your car display** via Android Auto — set Google Maps as your navigation app. Its voice handles turn-by-turn through your car speakers; Highway Radar handles all alert audio on your phone.

> ✅ **If you only do Steps 1–3**, you already have a significantly better law-enforcement awareness setup than Waze alone. Steps 4–5 are the full build.

---

## 🛠️ Validated Hardware (My Setup)

> ⚠️ **This entire guide was built and tested on the hardware below.** Settings, screenshots, and app behaviors described here reflect this exact stack. Notes throughout flag which steps are hardware-specific vs. universal.

| Component | Details |
|-----------|---------|
| **Vehicle** | Porsche GT3RS (992) |
| **Laser Defense** | Anti Laser Pro (ALP) |
| **Radar Detector** | Uniden R8 |
| **Companion App** | Highway Radar v3.2 (Android) |
| **Primary Phone** | Samsung Galaxy Z Fold 6 |
| **Navigation (Dash)** | Google Maps via Android Auto → Porsche PCM |
| **Navigation (Backup)** | Gaia GPS on iPad (offline topo + routes) |
| **Crowdsource Layer** | Waze + WzSabre 2.2 (split screen on Z Fold 6) |

---

## 🔄 Other Detectors

> 📝 **Not running a Uniden R8?** Most of this guide still applies — Highway Radar supports several detectors via Bluetooth. Here's what changes:

| Your Detector | Supported? | What's Different |
|--------------|-----------|-----------------|
| **Uniden R8** | ✅ Full support | This guide — all settings apply |
| **Uniden R9** | ✅ Full support (added in v3.1) | Same HR integration steps; first BT pairing can be finicky — toggle BT off/on if it fails |
| **Uniden R4** | ✅ Supported | Some R4/R9 internal settings sync was broken in v3.1; fixed expected in v3.2.1 |
| **Valentine One Gen2** | ✅ Full support via V1Connection | Use JBV1 app instead of HR for V1 users; WzSabre + Waze split-screen setup is identical |
| **Escort / Beltronics** | ⚠️ Partial | Escort Detector app handles BT integration; HR used for CSA + aircraft layer on top |
| **No detector** | ✅ Still useful | Highway Radar + WzSabre + Waze alone is a strong crowd-source-only setup |

**Universal settings** (apply regardless of detector): everything in the Alert Settings, Weather Radar, Aircraft Alerts, Risk Estimation, Navigation Stack, and Sound sections.

**Detector-specific settings**: the Custom Processing Rules and Lockout System sections are built for R8 frequency ranges — adjust filter thresholds to match your detector's known false-alert frequencies.

---

## 📱 Highway Radar App – Uniden R8 Integration

The Uniden R8 is natively supported by the Highway Radar app via Bluetooth. Integration provides:

- View radar alerts on a large phone screen with full signal details
- Intelligent low-speed muting based on current speed limit
- Custom signal processing and filtering rules
- One of the best auto-lockout systems available (fixed in v3.2 — lockouts no longer stall at "ALO candidate (1)")
- Modify R8 internal settings from the phone UI
- Define different radar setting profiles via Settings Packs
- Per-alert mute/unmute control

### Pairing the R8 with Highway Radar
1. On the R8: enable Bluetooth → Menu → Bluetooth → On
2. On Android: System Settings → Connected Devices → Pair new device
3. On R8: enter pairing mode → Settings → BT pairing
4. Select your device (appears as R8@XX where XX = two symbols)
5. Wait for "Pairing Succeeded" on the R8, then restart the R8
6. In Highway Radar: Settings → Radar detector integration → R8 connection manager → Scan
7. Select your R8 and wait 5–10 seconds to connect

> ⚠️ **Always restart the R8 after pairing** — failure to do so can cause Bluetooth connection issues.

---

## ⚙️ Recommended R8 Settings (Highway Radar Integration)

### Operational Settings

*(Settings → Radar detector integration → Operational settings)*

| Setting | Recommended Value | Notes |
|---------|------------------|-------|
| Muting Mode | Unmuted when unmuted alerts present | Keeps R8 quiet when HR has muted an alert |
| Signal Latch Interval | 2–3 seconds | Prevents alerts from disappearing too quickly |
| Announce Radar Frequency | Enabled | Useful for identifying BSM vs real threats |
| Push Internal Settings On Start | Enabled | Ensures R8 always uses your configured profile |

### Threat Classification (High vs Low Threat)

*(Settings → Radar detector integration → Operational settings → High threat signals)*

| Band | Recommended Classification | Rationale |
|------|--------------------------|-----------|
| Ka-band | High Threat | Primary police radar in the US |
| Laser | High Threat | Requires immediate response |
| K-band | Low Threat | High false positive rate (BSMs, door openers) |
| X-band | Low Threat | Rare police use; mostly false alerts |

**High Threat behavior:**
- Displayed higher in alert list
- Shown in red (vs green for low threat)
- Auto-lockouts DISABLED (you want to be alerted always)
- Not muted before GPS fix

**Low Threat behavior:**
- Displayed in green
- Auto-lockouts ENABLED
- Muted until GPS fix acquired

---

## 🔧 Custom Processing Rules

*(Settings → Radar detector integration → Custom processing rules)*

> 📝 **R8-specific:** The frequency ranges below are tuned for Uniden R8 BSM false-alert patterns. If you're on a different detector, you'll need to identify your common false-alert frequencies and adjust the ranges accordingly.

### Rule 1: K-Band BSM Filter (False Alert Suppression)

**Conditions:**
- Signal band: K-band
- Frequency ranges: 24.120–24.124, 24.157–24.163, 24.197–24.204 MHz
- POP signals: Normal signals only
- Signal strength: 0–55%

**Actions:**
- Signal visibility: Show as inactive (don't alert)
- Auto-lockouts: Disabled
- Signal latch: No latch

### Rule 2: Reduce Rear Sensitivity

**Conditions:**
- Signal band: All except Laser
- Signal direction: Rear
- Signal strength: 0–35%

**Actions:**
- Signal visibility: Show as inactive (don't alert)
- Signal latch: No latch

### Rule 3: Ka-Band Rear Alert (Custom Tone)

*(Optional)* Set a distinct tone for rear Ka-band alerts.

**Conditions:**
- Signal band: Ka-band
- Signal direction: Rear
- Signal strength: 0–100%

**Actions:**
- Override beeper: Custom tone for rear Ka alerts

---

## 🔒 Lockout System

Highway Radar uses its own lockout system independent of the R8's built-in lockouts.

> ✅ **v3.2 Fix:** A long-standing bug where auto-lockouts got stuck as "ALO candidate (1)" and never promoted to full lockouts has been fixed. Lockouts now work as designed.

### Auto-Lockout Settings

*(Settings → Radar detector integration → Lockouts)*

| Setting | Recommended Value |
|---------|-----------------|
| Number of hits for auto-lockout | 3 (conservative) or 2 (aggressive) |
| Consecutive misses to delete auto-lockout | 3 |
| Consecutive misses to delete manual lockout | 5 |
| Min interval between hits/misses recording | Several hours |
| Lockouts provided by detector | Respect detector lockouts |

### Lockout Tips
- Lockouts are frequency + strength based (± 0.005 MHz, up to ref + 30% stronger)
- Lockout shape follows the road, not just a circle
- To manually add a lockout: tap and hold on an active alert → "Add lockout"
- To remove a lockout: tap and hold → "Remove lockout"
- Export lockouts regularly for backup: Settings → Lockouts → Export

---

## 📍 Alert Settings (Crowdsourced Police Alerts)

*(Settings → Crowd-sourced alerts → Police)*

| Setting | Recommended Value | Notes |
|---------|-----------------|-------|
| Alerting Strategy | Combined | Uses both same-street and bearing methods |
| Max distance ahead | 2 miles | Adjust based on highway vs city driving |
| Max distance behind | 0.5 miles | Covers median threats |
| Max distance side | 0.25 miles | Cross-street awareness |
| Same street distance | 5 miles | Highway trap detection |
| Alert on opposite direction | Enabled | Catches median-parked officers |
| Severity Algorithm | Exponential or Fibonacci | More dots = more urgency near the alert |
| Fadeout interval | 25–30 minutes | See note below |

> 📝 **Fadeout Interval Update (v3.2):** The prior recommendation of 10–15 minutes was too aggressive. Forum discussion (Apr 2026) confirmed that Waze police reports can remain valid for up to 60 minutes. ferius confirmed the fadeout maximum will be raised to 60 minutes in a future update. Until then, set to the current max of 30 minutes to avoid missing stale-but-still-active reports, especially in low-traffic areas.

### Crowdsourced Server / WzSabre Plugin

**WzSabre is strongly recommended** as the primary crowdsourced alert source.

> ✅ **Current version: WzSabre 2.2** (as of June 2026). If you are seeing "CSA Problems" errors, update WzSabre to 2.2+.

**WzSabre Setup & Maintenance Tips:**
- After installing or updating WzSabre, **disable battery optimization** for both Highway Radar and WzSabre
  - Settings → Apps → WzSabre → Battery → Unrestricted
  - Settings → Apps → Highway Radar → Battery → Unrestricted
- Open the WzSabre app periodically — it self-updates when opened directly
- If CSA alerts stop working after an update, open WzSabre first to trigger any pending update, then restart Highway Radar
- WzSabre has been community-reviewed; no security red flags raised. The JBV1 developer confirmed it's trusted.

> ⚠️ **Note:** Waze data can be accessed via hostname but may violate ToS. Use WzSabre (the RDForum-recommended SABRE plugin) for best results.

---

## 🛫 Aircraft Alerts

*(Settings → Aircraft alerts)*

| Setting | Recommended Value |
|---------|-----------------|
| Aircraft to show | Suspicious + Unknown (at proximity) |
| Show foreign aircraft | Disabled |
| Show grounded aircraft | Disabled |
| Show unknown aircraft | Enabled (monitor for surprises) |
| Alerting sensitivity | Default (adjust as needed per region) |
| Zoom out on active aircraft | Enabled |

> ✅ **v3.2 Fix:** A crash affecting 1,000+ users caused by null fields in aircraft data has been fixed. If you had aircraft alerts disabled as a workaround, you can safely re-enable them.

---

## 📡 Settings Packs (Profiles)

*(Settings → Settings packs)*

### Profile 1: Highway Mode
- Alerting distances: Front 3 mi, Back 1 mi, Side 0.5 mi, Same street 8 mi
- Muting mode: Unmuted when beeper-active alerts present
- Aircraft: All categories shown
- Map zoom: Auto-zoom enabled

### Profile 2: City/Urban Mode
- Alerting distances: Front 0.75 mi, Back 0.25 mi, Side 0.15 mi, Same street 2 mi
- K-band BSM filter: More aggressive (lower threshold)
- Muting: Lower activation speed threshold
- Map zoom: Tighter view

### Profile 3: Track/Event Day
- All bands: High threat
- Auto-lockouts: Disabled (new environments)
- All notifications: Maximum sensitivity
- Beeper: Always active when alerts present

---

## 🔊 Sound & Notification Settings

*(Settings → Sound)*

| Setting | Recommended Value |
|---------|-----------------|
| Audio output | Default (routes through car speakers via Android Auto) |
| Reset system volume on start | Enabled at ~80% |
| Speech rate | 1.0–1.2x |
| Volume normalization | Basic |

> 📝 **v3.2 Voice Engine:** The voice recognition engine has been completely rebuilt using OpenAI's Whisper model (on-device, ~75 MB one-time download). Recognition is significantly improved in noisy driving conditions. A one-time model download is required via the start screen when first enabling voice commands.

### Alert Notifications by Type

| Alert Type | Bogey Tone | Voice Announcement | Beeper Activation Speed |
|-----------|-----------|-------------------|------------------------|
| Ka-band (High) | Yes | Yes | 5 mph over limit |
| Laser (High) | Yes | Yes | Any speed |
| K-band (Low) | Yes | Optional | 10 mph over limit |
| X-band (Low) | Optional | No | 15 mph over limit |
| Police (crowdsourced) | Yes | Yes | 5 mph over limit |
| Aircraft | Yes | Yes | Any speed |
| Camera | Yes | Yes | Any speed |

---

## 🗺️ Map & Display Settings

*(Settings → Display)*

| Setting | Recommended Value |
|---------|-----------------|
| Dark theme | Based on sunset/sunrise |
| Map style | Night (dark) / Silver (light) |
| Tilted map view | Enabled (3D perspective) |
| Show street ahead | Enabled |
| Map circles | 0.5 mi (red), 1 mi (yellow), 2 mi (green) |
| Max alert badges | 5 |
| Collapse inactive badges | Enabled |
| Auto-zoom | Enabled |
| Display from sleeping | When running OR charging |

> 📝 **Map Style Note:** As of late 2025, the "Standard" light map style renders as grayscale on some devices. If you notice grayscale maps on light theme, switch explicitly to "Silver" or set dark theme to a static value.

---

## ☁️ Weather Radar

*(Settings → Weather radar)*

> ⚠️ **RainViewer is no longer available** (ceased operations, confirmed in v3.2 release notes, Apr 2026).

| Option | Notes |
|--------|-------|
| **NWS / Iowa Environmental Mesonet NEXRAD** | Free, restored in v3.2 — recommended default |
| **Rainbow Weather API** | Requires your own API key from developer.rainbow.ai; free tier available but polls heavily (~12k calls/session). Monitor your usage tier. |

| Setting | Recommended Value |
|---------|-----------------|
| Data source | NWS NEXRAD (free, no API key needed) |
| Overlay opacity | 60% |
| Forecast type | Speed-based (shows what you'll drive into) |
| Zoom out on weather | Enabled, ~100 mi range |

---

## 📊 Risk Estimation

*(Settings → Risk estimation)*

- Uses 365 days of crowdsourced historical data
- Scoring model: 25-score (more granular)
- Shows probability of encountering police by location, time, and day
- High risk (20+/25): Exercise significant caution
- Medium risk (10–19/25): Normal alertness
- Low risk (1–9/25): Lower enforcement probability

---

## 🗺️ Navigation Stack — Multi-Device Setup

> 📝 **Hardware context:** This section is built around the Z Fold 6 + Porsche PCM with wireless Android Auto. The core audio priority chain and Waze settings apply to any Android phone. For a standard (non-foldable) phone, use **PIP mode** instead of split screen — see the HR Book for setup.

---

### 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     DEVICE ROLES                            │
├──────────────────────┬──────────────────────┬──────────────┤
│  Z Fold 6 (inner)    │  Porsche PCM Screen  │  iPad Mini   │
│  ─────────────────   │  ─────────────────   │  ─────────── │
│  Highway Radar (top) │  Google Maps via     │  Gaia GPS    │
│  Waze (bottom split) │  Android Auto        │  (offline    │
│  WzSabre (bg svc)    │  (turn-by-turn +     │  topo/route  │
│                      │  Waze-sourced LE     │  backup)     │
│  LE Awareness        │  reports on dash)    │              │
│  Radar Alerts        │  Primary Nav Voice   │  Off-road or │
│  CSA Data Source     │  ETA / Rerouting     │  canyon legs │
└──────────────────────┴──────────────────────┴──────────────┘
```

**Core principle:** Each device has one primary job. Establish a clear audio priority chain — never fight for audio between apps.

---

### 📱 Z Fold 6 — Split Screen Setup

#### App Layout (Inner Display)

| Position | App | Role |
|----------|-----|------|
| Top half (55%) | **Highway Radar** | Radar alerts, CSA, aircraft, R8 integration |
| Bottom half (45%) | **Waze** | Visual crowdsource data, community police reports |
| Background service | **WzSabre** | Feeds Waze data into Highway Radar CSA |
| Background (AA) | **Google Maps** | Sends turn-by-turn navigation to Porsche PCM |

#### Method 1 — Highway Radar Automation *(Android 12 and below only)*
1. Settings → General → Action on service start
2. Set to **"Start another app in split screen"** → choose Waze
3. Highway Radar auto-launches both apps in split screen on every Start tap

#### Method 2 — Samsung Multi-Window *(Android 13+ / Z Fold 6)*
1. Open Highway Radar
2. Swipe two fingers inward from the left edge → split screen triggers
3. Choose Waze from the app picker
4. Resize divider: give HR slightly more space (LE awareness > nav)
5. Tap the three center dots → **save as app pair** to Edge Panel or home screen
6. One tap from Edge Panel launches both apps every drive

#### Method 3 — Standard Phone (Non-Foldable)
- Use **PIP (picture-in-picture) mode** instead of split screen
- Settings → General → Back button action → Picture-in-picture
- Highway Radar floats as a small overlay above Waze in the foreground
- Or run Waze on your phone and Highway Radar on a dedicated second device/tablet

---

### 📱 Z Fold 6 Waze Settings (Split-Screen Role)

When Waze runs in the bottom half of the Z Fold 6, its job is **visual data only** — not audio navigation.

| Setting | Recommended Value | Reason |
|---------|-----------------|--------|
| Navigation voice | **Off** | Google Maps/Android Auto handles nav audio on dash |
| Police / hazard alerts audio | **On** | Redundant LE warning layer |
| Speed camera alerts | **On** | Fixed camera coverage |
| Chit-chat | **Off** | Reduces noise |
| Map display | **2D / North up** | Easier to read in small pane |
| Show Wazers | **Off** | Reduces visual clutter |
| Show traffic | **On** | Real-time flow visible |
| Personalize ETA | **On** | Settings → Navigation → Personalization |
| Go invisible | **On** | Reduces privacy exposure (still feeds traffic data) |
| Show speed limit | **Always** | Secondary speed reference |
| Speed alert threshold | 5–10 mph over | Personal preference |
| Battery optimization | **Disabled (Unrestricted)** | Critical — re-check after every Waze update |

> ⚠️ **Audio priority rule:** Waze voice must be **Off** when Android Auto is active. Highway Radar handles phone audio. Google Maps handles car speaker audio. Three audio sources = chaos.

---

### 🗺️ Waze Best Practices

**Alerts & Reports:**
- Police reports: **On**
- Speed cameras: **On**
- Road hazards: **On**
- Report reminder: **Off** (reduces driving distraction)

**Reporting while driving:**
- Use **voice commands** — say "Report police" or "Report hazard"
- Or set Highway Radar's report button to **"Submit report via SABRE"** — files from HR's UI without switching apps

**Privacy:**
- Go invisible: **On** — hides your icon from other Wazers while still contributing to traffic data

**Battery:**
- Battery optimization: **Disabled** (Unrestricted) — re-check after every Waze update; Android silently re-enables it, which is the #1 cause of CSA failures

**Waze + Google Maps cross-feed (2026):**
- As of March 2026, Google Maps and Waze share real-time incident reports bidirectionally
- Google Maps on your PCM now shows Waze-sourced police/hazard reports
- Running both apps simultaneously is the **maximum-coverage configuration**, not redundant

---

### 🚗 Android Auto → Porsche PCM Setup

1. On Z Fold 6: Settings → Connected devices → Connection preferences → Android Auto → enable Wireless
2. In Porsche PCM: Devices → Connect new devices → Connect Android Auto
3. Once paired wirelessly, connection is automatic when entering the car
4. Set **Google Maps as default navigation app** in Android Auto settings
5. Google Maps displays on PCM screen with full turn-by-turn, voice, and Waze-integrated LE reports

> ✅ **PCM note:** Android Auto replaces PCM native navigation entirely when active. This is intentional — let Android Auto own the dash screen.

---

### 🧭 iPad + Gaia GPS — Backup Navigation

**Role:** Fully offline/independent navigation layer for remote roads, canyon routes, track days, and cell-dead zones.

**Before the drive (at home/WiFi):**
- Download offline map tiles: Gaia → Downloads → Download map tiles (zoom levels 12–16)
- For canyon/track routes: add USGS 1:24k topo layer
- Import your planned route as a GPX track
- *Note: offline downloads require a Gaia GPS Premium subscription*

**When to use Gaia as primary:**

| Scenario | Primary Nav | Backup Nav |
|----------|------------|-----------|
| Highway/city | Google Maps (PCM) | Waze (Z Fold 6) |
| Canyon / remote road | Gaia GPS (iPad) | Google Maps offline |
| Track day | Gaia GPS (iPad) | — |
| Cell dead zone | Gaia GPS (iPad) | Google Maps offline |

---

### 🎵 Audio Priority Chain

```
Priority 1 (HIGHEST) — Highway Radar
  └── Radar/laser/aircraft/CSA alerts
  └── Phone speaker or BT — always audible

Priority 2 — Google Maps (Android Auto → Porsche PCM)
  └── Turn-by-turn voice navigation
  └── Route change warnings
  └── Car speakers via AA audio stream

Priority 3 — Waze (Z Fold 6, VISUAL ONLY)
  └── Voice: OFF when Android Auto active
  └── Enable audio only if Android Auto disconnects mid-drive

Priority 4 (LOWEST) — Gaia GPS (iPad)
  └── Muted by default
  └── Enable audio only when used as solo nav tool
```

---

### 🔄 Startup Sequence (Every Drive)

1. **Mount and plug in Z Fold 6** — USB-C charging required; multi-app + GPS drains battery fast
2. **Android Auto connects** wirelessly to PCM (automatic after initial setup)
3. **Open Google Maps** via Android Auto on PCM → set destination
4. **Start Waze** on Z Fold 6 — let GPS lock
5. **Tap Start in Highway Radar** — split screen with Waze launches automatically
6. **Confirm WzSabre is connected** — check HR CSA status indicator (green = active)
7. **Verify audio**: HR alerts audible on phone; Google Maps voice through car speakers; Waze voice off
8. **Open Gaia GPS on iPad** if needed → auto-follow GPS on
9. **Drive** — use voice commands for reports; do not manually switch apps

> ⚠️ **After any app update:** Re-check battery optimization for WzSabre and Highway Radar. Android silently re-enables it during updates — this is the #1 cause of CSA failures mid-drive.

---

### 🛠️ Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| CSA alerts not showing | WzSabre battery-optimized or outdated | Disable battery opt; update WzSabre to 2.2+ |
| HR split screen not launching | Accessibility service (Android 13+) | Use Samsung Multi-Window pairing instead |
| Google Maps not on PCM | Android Auto disconnected | Re-enable Wireless AA in phone settings |
| Three audio sources at once | Waze voice left on | Disable Waze voice when Android Auto active |
| HR audio missing | PIP mode triggered by AA | Disable "Activate PIP on home button" in HR |
| Gaia losing position | Screen timeout killed GPS | Set screen timeout to Never while navigating |
| Z Fold 6 overheating | Multi-app + GPS + charging | Ensure ventilation; avoid direct sun on mount |

---

## 🏁 ALP (Anti Laser Pro) Integration Notes

> 📝 **ALP-specific:** This section applies to Anti Laser Pro users only. If you're running TMG, Escort, or another jammer, head placement and workflow principles are the same but consult your jammer's documentation for angle/placement specs.

### ALP Head Placement (GT3RS Specific)
- Front heads: Mounted behind front license plate frame or in front grille openings
- Rear heads: Mounted at rear bumper/diffuser area
- Ensure heads are level and unobstructed

### ALP + Highway Radar Workflow
- ALP handles laser (LIDAR) threats independently
- Highway Radar + R8 handle radar bands and crowdsourced alerts
- Use Highway Radar's Laser alert classification as secondary confirmation when ALP fires
- Set R8 Laser sensitivity to complement ALP (ALP is primary laser defense)

### ALP Best Practices
- Test heads post-installation with a laser gun or VEIL test
- Check for firmware updates periodically
- Register jamming-to-gun distance in your area
- Use Highway Radar's aircraft alerts to anticipate VASCAR enforcement zones

---

## 📋 Issues / Tracking

- [ ] Optimal ALP head angle for GT3RS front bumper
- [ ] Best BSM filter frequencies for local K-band sources
- [ ] Lockout database — exported and version-controlled
- [ ] Settings pack configurations (export via Highway Radar backup)
- [ ] Firmware versions (R8 and ALP)
- [ ] WzSabre version in use + battery optimization confirmed
- [ ] Rainbow Weather API key rotation schedule (if using Rainbow)

---

## 📂 Repo Structure (Planned)

```
GT3RS-ALP-R8-Rally-Repo/
├── README.md                        # This file — overview and settings
├── settings/
│   ├── highway-radar-backup.md      # Highway Radar settings backup key
│   ├── settings-packs.md            # Detailed settings pack configs
│   └── r8-internal-settings.md      # R8 detector internal settings
├── lockouts/
│   └── lockouts-export.json         # Exported lockout database
├── alp/
│   ├── install-notes.md             # GT3RS-specific ALP install notes
│   └── head-positions.md            # Head placement documentation
├── logs/
│   └── trip-notes.md                # Per-trip observations and adjustments
└── resources/
    └── highway-radar-manual-ref.md  # Key manual references
```

---

## 🔗 Resources

- [Highway Radar Book (Full Manual)](https://book.highwayradar.com)
- [Highway Radar Settings Online Viewer](https://www.highwayradar.app/settings)
- [ALP (Anti Laser Pro) Official Site](https://www.antilaserpro.com)
- [Uniden R8 Product Page](https://uniden.com)
- [RDForum Community — Highway Radar subforum](https://www.rdforum.org/forums/268/)
- [WzSabre Plugin](https://wzsabre.rocks) — recommended crowdsourced alerts plugin
- [Gaia GPS](https://www.gaiagps.com) — offline topo/route navigation
- [Vortex Radar — Radar detector education](https://www.vortexradar.com)

---

## 🤝 Contributing / Corrections

Found a setting that works better for your region or detector? Something that's outdated or wrong?

- **File a GitHub Issue** — use the Issues tab to flag errors, suggest improvements, or share your own settings
- **Fork and PR** — if you want to contribute directly, fork the repo and submit a pull request
- **Regional notes especially welcome** — Ka-band threat levels, common BSM frequencies, and CSA coverage quality vary significantly by state/region

---

## 📝 Changelog

| Date | Change |
|------|--------|
| 2026-06-05 | Restructured for shareability: Quick Start, Other Detectors note, hardware disclaimer, Contributing section |
| 2026-06-05 | Added Navigation Stack section: Z Fold 6 multi-app setup, Waze best practices, Gaia iPad backup, audio priority chain, startup sequence |
| 2026-06-05 | Reviewed RDForum Highway Radar posts (past 12 months); updated for v3.2 release (Apr 2026) |
| 2026-06-05 | Updated app version to v3.2; documented fixed lockout bug (ALO candidate stall) |
| 2026-06-05 | Updated aircraft alerts section — v3.2 crash fix, safe to re-enable |
| 2026-06-05 | Updated WzSabre section: current version 2.2, battery optimization tip, CSA troubleshooting |
| 2026-06-05 | Updated weather radar: RainViewer deprecated, NWS NEXRAD now recommended default, Rainbow API notes |
| 2026-06-05 | Updated fadeout interval recommendation to 25–30 min (from 10–15 min); per forum discussion |
| 2026-06-05 | Added v3.2 Whisper voice engine note (on-device, ~75 MB model download required) |
| 2026-06-05 | Added map grayscale display note (Nov 2025 issue) |
| 2026-06-05 | Initial repo creation — framework and settings documented |

---

*This repo is for personal use and community sharing. Always comply with local laws regarding radar detectors and laser jammers.*
