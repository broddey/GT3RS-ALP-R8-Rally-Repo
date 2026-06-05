# GT3RS-ALP-R8-Rally-Repo 🏎️

> **Tracking the best tools, settings, and configurations for the Anti Laser Pro (ALP) laser defense system + Uniden R8 radar detector installed in a Porsche GT3RS.**
>
> ---
>
> ## 🚗 Vehicle & System Overview
>
> | Component | Details |
> |-----------|---------|
> | **Vehicle** | Porsche GT3RS |
> | **Laser Defense** | Anti Laser Pro (ALP) |
> | **Radar Detector** | Uniden R8 |
> | **Companion App** | Highway Radar (Android) |
> | **App Version** | v3.x |
>
> ---
>
> ## 📱 Highway Radar App – Uniden R8 Integration
>
> The Uniden R8 is natively supported by the Highway Radar app via Bluetooth. Integration provides:
>
> - View radar alerts on a large phone screen with full signal details
> - - Intelligent low-speed muting based on current speed limit
>   - - Custom signal processing and filtering rules
>     - - One of the best auto-lockout systems available
>       - - Modify R8 internal settings from the phone UI
>         - - Define different radar setting profiles via Settings Packs
>           - - Per-alert mute/unmute control
>            
>             - ### Pairing the R8 with Highway Radar
>            
>             - 1. On the R8: enable Bluetooth → **Menu → Bluetooth → On**
>               2. 2. On Android: **System Settings → Connected Devices → Pair new device**
>                  3. 3. On R8: enter pairing mode → **Settings → BT pairing**
>                     4. 4. Select your device (appears as `R8@XX` where XX = two symbols)
>                        5. 5. Wait for "Pairing Succeeded" on the R8, then **restart the R8**
>                           6. 6. In Highway Radar: **Settings → Radar detector integration → R8 connection manager → Scan**
>                              7. 7. Select your R8 and wait 5–10 seconds to connect
>                                
>                                 8. > ⚠️ Always restart the R8 after pairing — failure to do so can cause Bluetooth connection issues.
>                                    >
>                                    > ---
>                                    >
>                                    > ## ⚙️ Recommended R8 Settings (Highway Radar Integration)
>                                    >
>                                    > ### Operational Settings
>                                    > *(Settings → Radar detector integration → Operational settings)*
>                                    >
>                                    > | Setting | Recommended Value | Notes |
>                                    > |---------|-------------------|-------|
>                                    > | **Muting Mode** | Unmuted when unmuted alerts present | Keeps R8 quiet when HR has muted an alert |
>                                    > | **Signal Latch Interval** | 2–3 seconds | Prevents alerts from disappearing too quickly |
>                                    > | **Announce Radar Frequency** | Enabled | Useful for identifying BSM vs real threats |
>                                    > | **Push Internal Settings On Start** | Enabled | Ensures R8 always uses your configured profile |
>                                    >
>                                    > ### Threat Classification (High vs Low Threat)
>                                    > *(Settings → Radar detector integration → Operational settings → High threat signals)*
>                                    >
>                                    > | Band | Recommended Classification | Rationale |
>                                    > |------|---------------------------|-----------|
>                                    > | **Ka-band** | High Threat | Primary police radar in the US |
>                                    > | **Laser** | High Threat | Requires immediate response |
>                                    > | **K-band** | Low Threat | High false positive rate (BSMs, door openers) |
>                                    > | **X-band** | Low Threat | Rare police use; mostly false alerts |
>                                    >
>                                    > **High Threat behavior:**
>                                    > - Displayed higher in alert list
>                                    > - - Shown in red (vs green for low threat)
>                                    >   - - Auto-lockouts DISABLED (you want to be alerted always)
>                                    >     - - Not muted before GPS fix
>                                    >      
>                                    >       - **Low Threat behavior:**
>                                    >       - - Displayed in green
>                                    >         - - Auto-lockouts ENABLED
>                                    >           - - Muted until GPS fix acquired
>                                    >            
>                                    >             - ---
>                                    >
>                                    > ## 🔧 Custom Processing Rules
>                                    >
>                                    > *(Settings → Radar detector integration → Custom processing rules)*
>                                    >
>                                    > ### Rule 1: K-Band BSM Filter (False Alert Suppression)
>                                    > Filters out common blind-spot monitor (BSM) frequencies at low signal strength.
>                                    >
>                                    > **Conditions:**
>                                    > - Signal band: K-band
>                                    > - - Frequency ranges: `24.120–24.124`, `24.157–24.163`, `24.197–24.204` MHz
>                                    >   - - POP signals: Normal signals only
>                                    >     - - Signal strength: 0–55%
>                                    >      
>                                    >       - **Actions:**
>                                    >       - - Signal visibility: Show as inactive (don't alert)
>                                    >         - - Auto-lockouts: Disabled
>                                    >           - - Signal latch: No latch
>                                    >            
>                                    >             - ### Rule 2: Reduce Rear Sensitivity
>                                    >             - Reduces alerts from weaker signals approaching from behind.
>                                    >            
>                                    >             - **Conditions:**
>                                    >             - - Signal band: All except Laser
>                                    >               - - Signal direction: Rear
> - Signal strength: 0–35%
>
> - **Actions:**
> - - Signal visibility: Show as inactive (don't alert)
>   - - Signal latch: No latch
>    
>     - ### Rule 3: Ka-Band Rear Alert (Custom Tone)
>     - *(Optional)* Set a distinct tone for rear Ka-band alerts.
>    
>     - **Conditions:**
>     - - Signal band: Ka-band
>       - - Signal direction: Rear
> - Signal strength: 0–100%
>
> - **Actions:**
> - - Override beeper: Custom tone for rear Ka alerts
>  
>   - ---
>
> ## 🔒 Lockout System
>
> Highway Radar uses its own lockout system independent of the R8's built-in lockouts.
>
> ### Auto-Lockout Settings
> *(Settings → Radar detector integration → Lockouts)*
>
> | Setting | Recommended Value |
> |---------|-------------------|
> | **Number of hits for auto-lockout** | 3 (conservative) or 2 (aggressive) |
> | **Consecutive misses to delete auto-lockout** | 3 |
> | **Consecutive misses to delete manual lockout** | 5 |
> | **Min interval between hits/misses recording** | Several hours |
> | **Lockouts provided by detector** | Respect detector lockouts |
>
> ### Lockout Tips
> - Lockouts are **frequency + strength based** (± 0.005 MHz, up to ref + 30% stronger)
> - - Lockout shape follows the road, not just a circle
>   - - To **manually add** a lockout: tap and hold on an active alert → "Add lockout"
>     - - To **remove** a lockout: tap and hold → "Remove lockout"
>       - - **Export lockouts** regularly for backup: Settings → Lockouts → Export
>        
>         - ---
>
> ## 📍 Alert Settings (Crowdsourced Police Alerts)
>
> *(Settings → Crowd-sourced alerts → Police)*
>
> | Setting | Recommended Value | Notes |
> |---------|-------------------|-------|
> | **Alerting Strategy** | Combined | Uses both same-street and bearing methods |
> | **Max distance ahead** | 2 miles | Adjust based on highway vs city driving |
> | **Max distance behind** | 0.5 miles | Covers median threats |
> | **Max distance side** | 0.25 miles | Cross-street awareness |
> | **Same street distance** | 5 miles | Highway trap detection |
> | **Alert on opposite direction** | Enabled | Catches median-parked officers |
> | **Severity Algorithm** | Exponential or Fibonacci | More dots = more urgency near the alert |
> | **Fadeout interval** | 10–15 minutes | Removes stale reports automatically |
>
> ### Crowdsourced Server
> Connect via **Settings → Crowd-sourced alerts → Server hostname** or install a SABRE plugin.
>
> > 📝 Note: Waze data can be accessed via hostname but may violate ToS. Use a dedicated RDForum-recommended SABRE plugin for best results.
> >
> > ---
> >
> > ## 🛫 Aircraft Alerts
> >
> > *(Settings → Aircraft alerts)*
> >
> > | Setting | Recommended Value |
> > |---------|-------------------|
> > | **Aircraft to show** | Suspicious + Unknown (at proximity) |
> > | **Show foreign aircraft** | Disabled |
> > | **Show grounded aircraft** | Disabled |
> > | **Show unknown aircraft** | Enabled (monitor for surprises) |
> > | **Alerting sensitivity** | Default (adjust as needed per region) |
> > | **Zoom out on active aircraft** | Enabled |
> >
> > ---
> >
> > ## 📡 Settings Packs (Profiles)
> >
> > *(Settings → Settings packs)*
> >
> > Recommended profiles for GT3RS use cases:
> >
> > ### Profile 1: Highway Mode
> > - Alerting distances: Front 3 mi, Back 1 mi, Side 0.5 mi, Same street 8 mi
> > - - Muting mode: Unmuted when beeper-active alerts present
> >   - - Aircraft: All categories shown
> >     - - Map zoom: Auto-zoom enabled
> >      
> >       - ### Profile 2: City/Urban Mode
> >       - - Alerting distances: Front 0.75 mi, Back 0.25 mi, Side 0.15 mi, Same street 2 mi
> >         - - K-band BSM filter: More aggressive (lower threshold)
> >           - - Muting: Lower activation speed threshold
> >             - - Map zoom: Tighter view
> >              
> >               - ### Profile 3: Track/Event Day
> >               - - All bands: High threat
> >                 - - Auto-lockouts: Disabled (new environments)
> >                   - - All notifications: Maximum sensitivity
> >                     - - Beeper: Always active when alerts present
> >                      
> >                       - ---
> >
> > ## 🔊 Sound & Notification Settings
> >
> > *(Settings → Sound)*
> >
> > | Setting | Recommended Value |
> > |---------|-------------------|
> > | **Audio output** | Default (routes through car speakers via Android Auto) |
> > | **Reset system volume on start** | Enabled at ~80% |
> > | **Speech rate** | 1.0–1.2x |
> > | **Volume normalization** | Basic |
> >
> > ### Alert Notifications by Type
> >
> > | Alert Type | Bogey Tone | Voice Announcement | Beeper | Activation Speed |
> > |------------|-----------|-------------------|--------|-----------------|
> > | Ka-band (High) | Yes | Yes | Yes | 5 mph over limit |
> > | Laser (High) | Yes | Yes | Yes | Any speed |
> > | K-band (Low) | Yes | Optional | No | 10 mph over limit |
> > | X-band (Low) | Optional | No | No | 15 mph over limit |
> > | Police (crowdsourced) | Yes | Yes | Yes | 5 mph over limit |
> > | Aircraft | Yes | Yes | No | Any speed |
> > | Camera | Yes | Yes | No | Any speed |
> >
> > ---
> >
> > ## 🗺️ Map & Display Settings
> >
> > *(Settings → Display)*
> >
> > | Setting | Recommended Value |
> > |---------|-------------------|
> > | **Dark theme** | Based on sunset/sunrise |
> > | **Map style** | Night (dark) / Silver (light) |
> > | **Tilted map view** | Enabled (3D perspective) |
> > | **Show street ahead** | Enabled |
> > | **Map circles** | 0.5 mi (red), 1 mi (yellow), 2 mi (green) |
> > | **Max alert badges** | 5 |
> > | **Collapse inactive badges** | Enabled |
> > | **Auto-zoom** | Enabled |
> > | **Display from sleeping** | When running OR charging |
> >
> > ---
> >
> > ## ☁️ Weather Radar
> >
> > *(Settings → Weather radar)*
> >
> > | Setting | Recommended Value |
> > |---------|-------------------|
> > | **Data source** | RainViewer (best forecast) |
> > | **Overlay opacity** | 60% |
> > | **Forecast type** | Speed-based (shows what you'll drive into) |
> > | **Zoom out on weather** | Enabled, ~100 mi range |
> >
> > ---
> >
> > ## 📊 Risk Estimation
> >
> > *(Settings → Risk estimation)*
> >
> > - Uses 365 days of crowdsourced historical data
> > - - Scoring model: **25-score** (more granular)
> >   - - Shows probability of encountering police by location, time, and day
> >     - - High risk (20+/25): Exercise significant caution
> >       - - Medium risk (10–19/25): Normal alertness
> >         - - Low risk (1–9/25): Lower enforcement probability
> >          
> >           - ---
> >
> > ## 🏁 ALP (Anti Laser Pro) Integration Notes
> >
> > The ALP system is a laser defense (jammer) system independent of the Highway Radar app. Key integration notes for GT3RS install:
> >
> > ### ALP Head Placement (GT3RS Specific)
> > - **Front heads**: Mounted behind front license plate frame or in front grille openings
> > - - **Rear heads**: Mounted at rear bumper/diffuser area
> >   - - Ensure heads are **level and unobstructed**
> >    
> >     - ### ALP + Highway Radar Workflow
> >     - 1. ALP handles **laser** (LIDAR) threats independently
> >       2. 2. Highway Radar + R8 handle **radar** bands and crowdsourced alerts
> >          3. 3. Use Highway Radar's **Laser alert** classification as a secondary confirmation when ALP fires
> >             4. 4. Set R8 Laser sensitivity to complement ALP (ALP is primary laser defense)
> >               
> >                5. ### ALP Best Practices
> >                6. - Test heads post-installation with a laser gun or VEIL test
> >                   - - Check for firmware updates periodically
> >                     - - Register jamming-to-gun distance in your area
> >                       - - Use Highway Radar's **aircraft alerts** to anticipate VASCAR enforcement zones
> >                        
> >                         - ---
> >
> > ## 📋 Issues / Tracking
> >
> > Use the GitHub Issues tab to track:
> >
> > - [ ] Optimal ALP head angle for GT3RS front bumper
> > - [ ] - [ ] Best BSM filter frequencies for local K-band sources
> > - [ ] - [ ] Lockout database — exported and version-controlled
> > - [ ] - [ ] Settings pack configurations (export via Highway Radar backup)
> > - [ ] - [ ] Firmware versions (R8 and ALP)
> > - [ ] - [ ] Best SABRE plugin recommendation for crowdsourced alerts
> >
> > - [ ] ---
> >
> > - [ ] ## 📂 Repo Structure (Planned)
> >
> > - [ ] ```
> > - [ ] GT3RS-ALP-R8-Rally-Repo/
> > - [ ] ├── README.md                  # This file — overview and settings
> > - [ ] ├── settings/
> > - [ ] │   ├── highway-radar-backup.md    # Highway Radar settings backup key
> > - [ ] │   ├── settings-packs.md          # Detailed settings pack configs
> > - [ ] │   └── r8-internal-settings.md    # R8 detector internal settings
> > - [ ] ├── lockouts/
> > - [ ] │   └── lockouts-export.json       # Exported lockout database
> > - [ ] ├── alp/
> > - [ ] │   ├── install-notes.md           # GT3RS-specific ALP install notes
> > - [ ] │   └── head-positions.md          # Head placement documentation
> > - [ ] ├── logs/
> > - [ ] │   └── trip-notes.md              # Per-trip observations and adjustments
> > - [ ] └── resources/
> > - [ ]     └── highway-radar-manual-ref.md # Key manual references
> > - [ ] ```
> >
> > - [ ] ---
> >
> > - [ ] ## 🔗 Resources
> >
> > - [ ] - [Highway Radar Book (Full Manual)](https://book.highwayradar.com/)
> > - [ ] - [Highway Radar Settings Online Viewer](https://highwayradar.com/settings/)
> > - [ ] - [ALP (Anti Laser Pro) Official Site](https://www.antilaserpro.com/)
> > - [ ] - [Uniden R8 Product Page](https://www.uniden.com/)
> > - [ ] - [RDForum Community](https://www.rdforum.org/) — SABRE plugins, settings sharing
> > - [ ] - [Vortex Radar](https://www.vortexradar.com/) — Radar detector education
> >
> > - [ ] ---
> >
> > - [ ] ## 📝 Changelog
> >
> > - [ ] | Date | Change |
> > - [ ] |------|--------|
> > - [ ] | 2026-06-05 | Initial repo creation — framework and settings documented |
> >
> > - [ ] ---
> >
> > - [ ] *This repo is for personal use tracking GT3RS countermeasure setup. Always comply with local laws regarding radar detectors and laser jammers.*
