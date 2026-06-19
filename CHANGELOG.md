# Changelog

All notable changes to MultiScope ATC are documented here.
Format: one section per release, newest first. Each section mirrors the GitHub release notes exactly.
When a new version is tagged, add a section here and create the GitHub release in the same step.

---

## [v0.20.4] — 2026-06-17

### Radar
- **Do-not-cross marker aligned to the localizer.** The spacing crossbar now orients to the assigned runway's localizer course (rather than the aircraft's instantaneous track), so it lies square across the final approach course and clearly indicates where the next aircraft may sit. The crossbar is also drawn thinner (1px).

### Build / Packaging
- **Voice recognition now ships in the Windows build.** `System.Speech` was gated on the build host (`IsOsPlatform('Windows')`), so a Windows build cross-compiled from macOS silently compiled push-to-talk voice control out. It is now gated on the export target (matching the Steamworks block), so `WINDOWS_SPEECH` is defined and `System.Speech` is bundled for any Windows-targeted export.
- **Piper neural TTS now ships in the Windows build.** `piper.exe` and the `.onnx` voice models are not packed by Godot's resource exporter; `scripts/steam/upload.sh` now copies `assets/piper/` into the export root (where the game resolves `res://assets/piper/` at runtime), so pilot read-backs are no longer silent.
- **`upload.sh` can set a build live** via `SET_LIVE_BRANCH` (the public `default` branch must still be promoted from the Steamworks dashboard).

---

## [v0.20.3] — 2026-06-17

### Radar
- **Spacing indicator redrawn as a "do-not-cross" marker.** The ROT/blue spacing line is no longer a line drawn from the aircraft blip. It is now a short horizontal crossbar positioned behind the aircraft at the spacing distance, with a gap between the blip and the marker. The crossbar turns with the aircraft's track and only appears once the aircraft is established on the localizer.

### UI
- **Developer Menu no longer runs off-screen.** The XWIND readout now lists only the active arrival/departure runways instead of all ~12 runways, keeping the panel within the visible width.
- **Runway picker height fixed.** The crosswind and validation warning rows now collapse fully when hidden (the wrapping MarginContainers are toggled, not just the labels), so the picker no longer reserves dead space and is back to its original height.

---

## [v0.20.2] — 2026-06-16

### Crosswind
- **Out-of-limits arrival runways force a go-around after 30 s.** An aircraft established on the ILS to a runway whose crosswind is out of limits (≥30 kt) goes around once it has been out of limits for 30 seconds — a grace window for the controller to reduce the crosswind (the timer resets if the wind eases below the limit).

---

## [v0.20.1] — 2026-06-16

### Go-around & Missed Approach
- **ROT-spacing overshoot now drives go-arounds.** The 0.5 NM decision uses the higher of the crosswind and ROT-overshoot probabilities: when a preceding arrival crosses the threshold, the succeeding aircraft's overshoot of the spacing (blue) line is measured and run through the published overshoot table. A preceding aircraft still occupying the runway when the succeeding reaches 0.5 NM forces a 100% go-around.
- **Missed approaches cross fixes at their published levels** (e.g. 36R: AM637 @ A015 → AM163 @ A020) instead of holding the initial climb level throughout.

### Saves
- Landing-rollout, missed-approach and general-aviation state now persist across save/load (saving mid-rollout or mid-go-around restores correctly).

---

## [v0.20.0] — 2026-06-16

### Landing & Runway Occupancy
- **Landing rollout** — arrivals no longer vanish at the threshold; they flare to VREF (90% of FAS) by the touchdown point, hold 2 s, brake to 40 kt, pick the next usable exit (>200 m ahead, by commercial/GA traffic type), slow to 30/15 kt, turn off and taxi 100 m before vacating. On radar a rolling aircraft is a bare grey REL blip; it disappears at the exit.
- **Runway Occupancy Time (ROT)** is predicted per arrival cleared for the approach (least-time exit), and a **blue spacing line** is drawn behind it showing where the next arrival should sit to land once the runway has vacated + 30 s.

### Crosswind
- Crosswind component (`V·sin(Wd−Rh)`) drives runway-selector limits (≥30 kt blocked "out of limits"; 20–30 kt "close"; parallel 18R+18C / 36R+36C ≥15 kt prohibited), reactive to wind changes.
- Departures are suppressed from any runway ≥30 kt crosswind; a 100% crosswind go-around probability makes arrivals call **"unable to land … due to crosswind"** (immediately when cleared, or after 1 min inbound).

### Go-around & Missed Approach
- Inside 0.5 NM (random trigger point) an approach may **go around** — currently driven by the crosswind probability table — flying the runway's published missed-approach procedure (initial-heading climb to the climb level at 220 kt, then the after-turn segments), calling "going around" and re-checking in after 40–80 s ready to be TOC'd.
- A go-around aircraft's label shows a blank instructed heading; selecting it on first contact auto-shows the **yellow missed-approach route** (RTE toggles it).

### Data & Dev
- New EHAM nav data: runway touchdown points + rapid-exit taxiways, missed-approach procedures, additional IF/MAF/VIX waypoints; `generalAviation` flag plumbed from the schedule.
- Dev menu shows per-runway crosswind and, per selected aircraft, VREF / chosen exit / ROT / live go-around %.

---

## [v0.19.4] — 2026-06-16

### UI
- **Main-menu title restyled** to match the branding wordmark: **MULTISCOPE** in white with a green **Air Traffic Control** subtitle (was "MULTISCOPE ATC" / "AMSTERDAM CONTROL").
- Added an always-visible **entertainment-only disclaimer** footer to the main menu.

### Packaging
- **PCK encryption reverted** — it requires export templates compiled from source with the key baked in; with stock templates the exported game can't decrypt its own PCK and fails to load. Given the nav data is self-built from public-domain references, encryption is unnecessary and has been turned off so builds load reliably. (Supersedes the "PCK encryption enabled" note in v0.19.3.)

---

## [v0.19.3] — 2026-06-15

### Legal & Packaging
- **About / Legal screen** — a new main-menu entry opening an overlay with the entertainment-only / not-for-real-world-use, no-affiliation / trademark, navigation-data, and no-warranty disclaimers, plus a pointer to the third-party notices.
- **THIRD-PARTY-NOTICES** added (Godot, .NET/GodotSharp, System.Speech, DiscordRichPresence, Piper, fonts) with verified per-voice Piper licenses.
- **Pilot voices pruned to the commercially-cleared set** — only `alba` (CC BY 4.0), `aru` (CC BY 4.0), `cori` (public domain) and `jenny_dioco` (commercial OK, credited "Jenny (Dioco)") are bundled. Removed `alan` / `amy` / `danny` (license unverified), `semaine` (non-commercial), `lessac` (research-only), and `northern_english_male` / `southern_english_female` (ShareAlike).
- **Export hardening** — PCK encryption enabled and the AIRAC / data JSONs explicitly packed into the (encrypted) PCK so they aren't shipped in the clear. (Encryption key is supplied at export time.)

### UI
- New Game Setup: the per-section **ADVANCED** panels now sit within their Traffic / Weather block; added a **TOTAL** stepper to the traffic advanced panel (splits across departures/arrivals, 20–80%); smaller ± buttons and a solid white sum-line above TOTAL.

### Bug Fixes
- The **RTE** button is now disabled when no aircraft is selected (it stays usable pre-TOC once one is selected).

---

## [v0.19.2] — 2026-06-15

### Bug Fixes
- **Fixed a crash** (`ArgumentNullException`) when selecting an aircraft that has no assigned route — e.g. a departure that was turned back, or a generic departure. `NavigationDatabase.GetRoute` now returns `null` for a null/empty key.

### Radar & Labels
- **Turned-back departures show the runway** — when a departure is assigned a landing runway (turning back to the airport), the runway allocator now appears on its leader stick and the runway is shown in the LID, instead of a SID exit fix. Tracked by a new `AssignedLandingRunway` flag (persisted in saves).
- **SID-based label orientation** — departures on a SID default their label to the **left (West)** for west-bound SID exits and the **right (East)** for east-bound exits, derived from the SID exit-fix side.

### Command Panel
- **RTE button is usable before TOC** — it only toggles the route view, so it is no longer disabled while an aircraft is still pending transfer of control.

### Traffic
- **Mid-game runway changes re-allocate pending inbounds** — when the active runways are adjusted in-game, arrivals the player has not yet engaged (awaiting TOC or still pre-contact, not on an ILS, not manually runway-assigned) are re-resolved against the runway allocation table using their inbound IAF.

### New Game Setup
- **Traffic and Weather ADVANCED panels** — each section gains a collapsible **ADVANCED** panel mirroring the in-game side panels: Traffic exposes `DEPARTURES` / `ARRIVALS` ± steppers; Weather exposes `WIND DIRECTION` / `WIND SPEED` / `QNH` ± steppers plus a **RANDOM** button. Editing the weather controls overrides the auto-rolled wind at START; picking a condition re-enables the auto-roll.
- The bottom collapsible section is renamed **OTHER SETTINGS**.
- The CANCEL / START footer buttons get top margin so they sit centered.

---

## [v0.19.1] — 2026-06-15

### Radar & Labels
- **Tuned the inbound runway allocator placement** on the leader stick. The runway indicator added in v0.19.0 now sits correctly against the stem for all 8 label orientations (N / NE / E / SE / S / SW / W / NW), after several rounds of playtested pixel offsets.

---

## [v0.19.0] — 2026-06-15

### New Game Setup
- **Traffic intensity reworked** to QUIET / NORMAL / BUSY / OVERLOAD with the published per-hour rates: 10/10, 20/20, BUSY 20/40 or 40/20, OVERLOAD 40/40. **BUSY** resolves at random to an inbound or outbound peak on selection.
- **Automatic runway combinations** — selecting a traffic load now auto-fills a random published runway combination for that load (every departure runway has SIDs, every arrival runway is ILS-equipped). The player can still adjust runways in the advanced section.
- **Runway-correlated weather** — weather conditions are now CALM / NORMAL / GUSTY (default NORMAL). The surface wind is rolled at START from the chosen condition and the runways in use: CALM is any direction at 0–5 kt; NORMAL / GUSTY combine the wind-direction arc and speed band of every matching runway row (arcs unioned over 5° multiples with wrap-around handling). GUSTY wind is 15–25 kt. QNH stays at its default here (only the in-game WX RANDOM rolls QNH).
- **Section order** is now Traffic → Runways → Weather → Advanced.

### Radar & Labels
- **Inbound runway allocation on the leader stick** — the allocated landing runway is now drawn on the label's leader stick (in addition to the info box and LID bar), positioned per the label orientation across all 8 stem directions. Font is 75% of the label font and scales with the label-size setting. Arrivals only.

### Comms
- **Pilot check-in queue pauses while you work an aircraft** ("shut up while I'm talking") — selecting an aircraft holds incoming check-ins; the queue resumes after 5 s of inactivity, with each LID selection restarting the timer. Pressing EXE holds the queue until that command's readback has finished. Readbacks and in-progress transmissions are never interrupted; held check-ins are queued, not dropped.

### Internal
- Extracted the WX-panel RANDOM weather logic into a single reusable `WeatherRandomizer.Generate()` Core function (behaviour unchanged — same 80/15/5 bands).

---

## [v0.18.1] — 2026-06-14

### Internal
- **AIRAC folder renamed `2405` → `2606`** to match the cycle the EHAM data was updated to. The nav data was already cycle 2606 but lived under the old `AIRAC/2405/` path with the loader constants still set to `2405`. Renamed the folder and updated the `AiracCycle` constant (FrequencyDatabase), the `airacCycle` defaults (NavigationLoader cycle / schedule / route-division loaders), `cycle.json` (effective 2026-06-11, expiry 2026-07-09) and `waypoints.json` metadata, the main-menu loading line, and the wiki. No gameplay or data content change.

---

## [v0.18.0] — 2026-06-14

### Traffic & Navigation
- **AIRAC 2606** — runway 24 SIDs updated to the new cycle (designators bumped, e.g. `ANDIK2S` → `ANDIK3S`) with new waypoints `AM095` / `AM096` / `AM097` / `AM125` / `AM156` / `AM157` added to the affected routes; aircraft performance data refreshed.
- **Inbound runway allocation reworked** to match the published allocation logic (`TrafficScheduler.ResolveArrivalRunway`):
  - Per-runway arrival capacity lowered from 10 to **8** allocated flying aircraft.
  - Double-runway overflow now **cancels the spawn** when both runways are at capacity, instead of overflowing onto a full runway: primary <8 → primary; else other <8 → other; else cancel.
  - New per-IAF **fallback priority lists** (ARTIP / RIVER / SUGOL) used when 3+ arrival runways are active or the active pair has no published double config — picks the first active runway below capacity, else cancels.
  - The six published double-runway IAF maps were already correct and are unchanged.
  - Cancellation is arrivals-only; departures are unaffected.

### Departures
- **36C departures fly their full SID** when 36L is also active for departures. Fixed the `sid_division.json` entry for the 36L+36C combination, which listed non-existent SID IDs and left 36C departures with no route.
- **Vector-SID initial headings are magnetic** — `initial_heading` in `procedures.json` is now converted to true at load, so 36C BERGI/IDRID vector departures climb on the published 330° magnetic (was displaying 328°).
- **Departure score counts at the APP boundary** — a departure is scored when it leaves EHAM APP jurisdiction (TMA volumes), not at the 55 NM despawn radius.

### Radar & Labels
- **Wake-turbulence category is now read from the aircraft performance database** (`WtcIcao` / `WtcRecat`) instead of hardcoded per-type lists, and honours the ICAO/RECAT player setting. New types added to `aircraft_performance.json` are reflected automatically.
- **LID top-right shows the SID exit fix for departures** (was showing the runway). Only the IF for the aircraft's allocated runway is shown, not all active-runway IFs.
- **REL labels** inherit the last full-label orientation instead of snapping to West, and **REL blips are dimmed grey** instead of staying white so released traffic recedes into the picture.

### UI & Settings
- **TOC/REL button** always reads `TOC/REL` (not `REL`) when the display setting is set to TOC/REL.
- **REL works for arrivals with Transfer of Control enabled** — the 2 NM localizer-proximity gate is now applied only in arcade mode.
- **ESC closes the settings menu** instead of resuming the game and leaving the menu floating on screen.
- Settings sliders no longer change value when scrolling the menu with the mouse wheel.

### Audio
- **Shorter pilot frequency pauses** — courtesy gap between transmissions reduced from 3–5 s to 0.8–1.5 s, and trailing TTS silence is trimmed so the squelch cuts in immediately after the last word.

### Internal
- Removed the `DepartOnRunwayHeading` flag (superseded by full-SID 36C departures).
- Dev menu: staging-guides overlay (radar clock axes), section reorder, and airline / clearance-altitude spawn controls.

---

## [v0.17.2] — 2026-06-13

### Bug Fixes
- **Predicted STCA conflicts now display** — the conflict detector already computed Advisory (<120 s) and Warning (<60 s) predicted conflicts, but every radar render path discarded them and drew only actual Loss of Separation. Converging co-altitude traffic therefore showed no alert at all until separation was already lost. All three severities are now surfaced:
  - **Blip colour** — each blip is coloured by its highest active severity: red (LOS) > amber (Warning) > yellow (Advisory).
  - **Conflict line** — predicted alerts draw a severity-coloured line labelled `STCA <seconds>s`; LOS keeps its blinking red `LOS x.xNM` line.
  - **HUD** — adds an amber `⚠ STCA ×N` counter for predicted conflicts, upgrading to the blinking red `⚠ LOS ×N` once separation is actually lost.
- All conflict symbology remains gated on the `FailureSepLoss` setting.

---

## [v0.17.1] — 2026-06-12

### Traffic
- **Departures no longer starve** — when a spawn cycle's chosen type is blocked (RECAT wake-turbulence cooldown, arrival-runway capacity full, etc.) and the other type is enabled, the scheduler now tries the other type in the same cycle instead of wasting the slot. Departures previously sat blocked through the 80–180 s RECAT window while arrivals kept spawning.
- **ACC climb-out restored** — departures handed to the area control centre now actually receive their cruise-climb clearance ~10–20 s after handoff and continue climbing past their initial cleared level. The climb-out check was looking for sector `"EHAM CTR"` but the handoff stores `"EHAM ACC"` (from `frequencies.json`), so it had silently never fired.

### Flight Guidance
- **LOC capture extended to 25 NM** — aircraft now intercept the localizer up to 25 NM out (was 15 NM), even when no localizer line is drawn on the radar. The capture cone is evaluated only against the aircraft's assigned runway.
- **Departure to landing runway drops its SID** — assigning a landing runway (or clearing for the approach) to an airborne departure that is turning back now kills the SID: it is no longer route-followed and no longer shown in the data tag. The aircraft flies vectors until cleared for the approach. Arrivals keep their STAR.

### Conflicts
- **Parallel-approach separation** — no loss-of-separation is flagged between two aircraft that are each geometrically established on the localizers of two different runways (e.g. 18R/18C, 36C/36R), anywhere along the ~25 NM final. The test is purely geometric (on the centreline, heading aligned within 35°); being merely *cleared* for the ILS is not enough — an aircraft still being vectored onto final is separated normally.

### Radar
- **Arcade speed-on-final shows the full countdown** — with SPEED REDUCTION ON FINAL enabled, the IAS label now counts down across the whole established localizer final (through the automatic 180 kt gate at 10 NM and 160 kt at 6 NM down to final approach speed), instead of freezing on the instructed speed until the last 4 NM.

### Settings
- **TOC/REL settings consolidated** — removed the dead "Transfer of Control Label" duplicate. Two distinct settings remain: **TOC/REL** (button display — SHOW FREQUENCY / TOC/REL) and **TRANSFER OF CONTROL** (the mechanic — DISABLED / ENABLED).
- **Arcade TOC/REL button shows frequency only** — once an aircraft is under control, the button shows just the next sector frequency (tower for arrivals, ACC for departures) at the normal button font size, with no "REL" line above it.
- **Realistic label fix** — under-control aircraft in realistic mode now read "REL" instead of "TOC/REL", which had made controlled aircraft look uncontrolled.
- **Disabling TRANSFER OF CONTROL mid-session** now immediately clears the pending-TOC state on every in-sector aircraft, so their data tags return to full/bright without a per-aircraft TOC press.

### Internal
- Removed `driver/enable_input=true` from `project.godot` — fixes Windows microphone contention between Godot's audio input and the System.Speech voice recogniser.

---

## [v0.17.0] — 2026-06-12

### Audio
- **Custom volume mix** — new MASTER / VOICE / EFFECTS sliders in the AUDIO settings. A new audio-bus graph (`src/Godot/Audio/AudioBuses.cs`) splits pilot speech (`PilotRadio` voice bus, carrying the VHF effect chain) from radio effects (`RadioFx` bus — squelch + gap static). Volumes persist in `settings.cfg` and apply at startup.
- **Effects volume covers all radio noise** — the in-speech hiss is baked into the voice PCM (gapless), so `MixStaticNoise` now scales that baked hiss by the EFFECTS volume; the slider governs squelch, gap static, and in-speech hiss together.

### Accessibility
- **Pilot subtitles** — optional bottom-centre captions of pilot transmissions (`CALLSIGN: TEXT`), driven live by the voice state, covering readbacks and check-ins. New ACCESSIBILITY settings section with on/off, size (12–28 px), and background opacity. Persisted under `[accessibility]`.

### Steam
- **Achievements** — 8 achievements wired via a new `Achievements` helper + triggers in `SimulationManager`: first landing, first departure, 15-aircraft rush hour, clean sweep (20 landings / 0 LOS), holding pattern, and 100 / 500 / 1000 cumulative landings. Cumulative totals use a local `TotalLanded` counter persisted under `[stats]` (cloud-synced via Auto-Cloud); no Steam stats required. Source icon art (16 SVGs, locked + unlocked) added under `assets/branding/Steam/Achievements/`.

### Wiki
- Updated Audio-System (bus graph, volume mix, subtitles), Steam-Integration (achievements), and Game-Settings (audio volumes, accessibility, stats).

---

## [v0.16.0] — 2026-06-11

### Saving
- **Automatic saves** — a rolling autosave now runs alongside the three manual slots and never overwrites them. Triggers: every 120 s of active (unpaused) play, on pause, and on exit (return-to-menu, scene change, hard window close). A one-deep backup (`save_autosave_prev.json`) guards against a crash mid-write. New `SaveManager.WriteAutosave` / `ReadAutosave` / `PeekAutosave` / `HasAutosave`; `SimulationManager` refactored to share `BuildSave()` / `ApplySave()` across slots and autosave; `LoadAutosave()` added.
- **Autosave in CONTINUE** — the main-menu load screen shows an AUTOSAVE row at the top (load-only, via a `LoadSlot = -1` sentinel).
- **Autosave settings** — `AutosaveEnabled` (default on) and `AutosaveIntervalSec` (default 120), persisted in `settings.cfg`.

### Steam
- **Steam Cloud (Auto-Cloud) ready** — the project now uses a fixed custom user dir (`use_custom_user_dir` + `custom_user_dir_name="MultiScopeATC"`) so the cloud paths are stable per OS (`%APPDATA%\MultiScopeATC`, `~/Library/Application Support/MultiScopeATC`, `~/.local/share/MultiScopeATC`). Auto-Cloud syncs `save_*.json` + `settings.cfg`. Autosave's during-play disk writes satisfy Dynamic Cloud Sync (Steam Deck suspend/resume). Note: changing the user dir relocates existing local saves/settings.

### Dev Tools
- **Unified SPAWN panel** — the separate ARRIVAL/DEPARTURE force-spawn sections and the "STAGING (screenshots)" section are merged into one SPAWN panel with an `[ ARR | DEP | MANUAL ]` mode switch (active mode highlighted). ARR/DEP perform realistic spawns; MANUAL places an aircraft anywhere with full STATE/ALT/HDG/SPD/TYPE/ROUTE control + PLACE MODE / AT MOUSE. Includes the FREEZE toggle.
- **EDIT SELECTED panel** — HDG/ALT/SPD with a new LOAD button (pull the selected aircraft's current values), APPLY, DRAG, and SNAP TO ILS gathered in one place.
- **History dots follow edits** — editing (APPLY), dragging, and snapping a staged aircraft now re-seed the radar history trail so the dots trail the new heading/position instead of the old one.
- Removed the "(screenshots)" label from the staging button.

### Bug Fixes
- **Readback no longer drops commands when TTS is unavailable** — if Piper/TTS cannot speak (no models/engine), `PilotVoice.Readback` now still applies the validated instruction and fires its completion callback; only the spoken readback is skipped.

### Airspace / Data
- **EHAM approach** — TMA_1–4 altitude labels set to `disabled` in `jurisdictions.json` (were `stayBelow` / ceiling).

### Branding
- Reorganized branding assets into `assets/branding/SVG/` and `assets/branding/Steam/`; removed the legacy root SVGs (`Kopcapsule`, `logo`, `radar`, `wordmark`) and the old raster export. Added Steam store source art (capsule / banner / hero).

### Wiki
- Updated Save-System, Steam-Integration, Game-Settings, and UI-Systems for autosave, Steam Cloud, and the unified dev spawn/edit menu.

---

## [v0.15.0] — 2026-06-10

### Traffic & Setup
- **New-game presets** — the setup menu now offers traffic presets (QUIET 6/6, NORMAL 15/15, BUSY 22/22, HEAVY 30/30 arrivals/departures per hour) and weather presets (CALM/STANDARD/… wind + QNH), replacing the previous manual-only configuration.

### Voice & Phraseology
- **Spoken sector handoff readbacks** — frequency-transfer readbacks now use the `spoken_sector` label from `frequencies.json` (e.g. "amsterdam", "tower") instead of an abbreviation of the raw sector name. New `AircraftState.HandedOffSpokenSector` carries it through; `ContactFrequencyCommand` gained a `spokenSector` argument; `Phraseology.FrequencyTransferReadback` reworked with a spoken-label parameter and fallback. Persisted in saves.
- **EHAM ACC spoken labels** — trimmed "amsterdam radar" → "amsterdam" in `frequencies.json`.
- **Voice recognition is Windows-only, with clean degradation** — on macOS/Linux `VoiceRecognizer.IsAvailable` is now `false`, `Start()` sets a platform-accurate `StartupError`, and the PTT readout shows the Windows mic hint only on Windows.

### Radar & Dev Tools
- **Screenshot staging system** (dev menu, collapsible STAGING section) — freeze aircraft in place while the radar sweep keeps rotating; place aircraft at clicked coordinates in a chosen state (arrival / departure / on-ILS / holding); drag blips to reposition; set a selected aircraft's heading/altitude/speed instantly; snap an aircraft onto the ILS final at a chosen distance; pick aircraft type and SID/STAR route. Staged aircraft appear instantly with a synthetic history trail.
- **Scrollable dev menu** and **zoom buttons** (−/+) in the VIEW row.
- **Departure staged aircraft now render** — they spawn as `Climb` (airborne) rather than `Departure` (ground roll, which the radar skips).
- **Trail recording skipped while frozen** so seeded history dots aren't overwritten.

### Branding
- **Logo redesign** — CTR is now a round circle, CTA is discrete sector boxes reaching the radar rim, and the TWR/CTR · APP/TMA · ACC/CTA text labels were removed. TMA polygon unchanged.
- **New assets** — radar-only SVG (`radar.svg`), standalone wordmark SVG (`wordmark.svg`, real VCR font embedded), and Illustrator-editable layered SVGs (`logo_layers.svg`, `radar_layers.svg`) with every element as a named top-level group. Regenerated `logo.png` / `logo_512.png`; synced `icon.svg`.

### Steam
- **Real App ID provisioned** — `4832540` wired into `steam_appid.txt` and `SteamManager` (was the `480` Spacewar placeholder); verified `SteamClient.Init` against a live client. Windows depot `4832541`.
- **SteamPipe upload scripts** and a **macOS export preset** (universal, ad-hoc signed); enabled ETC2 ASTC VRAM compression (required for universal/arm64 macOS export).

### Internal
- Steam store copy and capsule/screenshot asset briefs added under `docs/`.
- CommandPanel TOC/REL arcade vs realistic refinements.

---

## [v0.14.0] — 2026-06-08

### Simulation
- **Pressure altitude storage** — `AircraftState.Altitude` and `TargetAltitude` are now stored as pressure altitude (1013.25 hPa reference) at all times. Previously stored as QNH altitude, which caused FL display to shift when QNH changed. The stored value never changes with QNH; display paths convert to QNH altitude at render time for below-TL aircraft.
- All command input paths (LID click, voice) convert incoming QNH altitude clearances to pressure altitude at the point of storage (`QnhToPressure`). FL clearances are stored as-is.
- `IlsFollower` converts geometric glideslope targets to pressure altitude (`QnhToPressure`) before writing to `TargetAltitude`.
- `SyncAircraftQnh` now only updates `AppliedQnh`; it no longer shifts `Altitude` (nothing to shift with pressure altitude storage).
- `PressureMode` boundary check updated from `TransitionLevelQnhFt` to `TransitionLevelFt` (consistent with pressure altitude storage).

### Simulation — Holding Pattern
- **Direct Entry arc displacement fix** — `DoDirectEntryExtend` now triggers the outbound turn at `2R - turnHoldDisp` instead of `2R`, accounting for the lateral displacement already accumulated during the initial turn arc from the fix. Prevents outbound leg overshoot on Direct entries.

### UI — LID Input Lock for PendingToc
- **LID locked while aircraft is PendingToc** — when `TocMechanicEnabled` is on, all LID input (column buttons and keyboard navigation) is disabled for the selected aircraft until TOC/REL is pressed.
- PTT and voice commands work normally regardless of `PendingToc` — the aircraft is on frequency; TOC is an administrative action, not a communication gate.
- `EXE`, all HDG/LVL/SPD column buttons, and keyboard arrows are all blocked while pending.

### Audio
- **Readback speed setting** — new slider in the AUDIO section of the Settings menu (1.00×–2.00×, default 1.15×). A ▶/■ preview button speaks a sample phrase at the current rate for live audition. Setting is also accessible from the Main Menu settings screen. Persisted under `[audio]` → `readback_speed` in `settings.cfg`.

### Voice
- **"expect runway" command** — say `expect runway [runway]` to assign an expected runway to the selected aircraft. Runway spoken form follows existing convention (e.g. `"one eight right"`).
- **Accurate platform error** — on macOS/Linux `VoiceRecognizer` immediately sets `StartupError = "Voice recognition is only available on Windows."` rather than silently failing or attempting to load a non-existent engine.

### New Game Setup
- **Runway validation** — the START button is disabled until at least one arrival runway and one departure runway are selected. A warning label appears when the player attempts to start with an incomplete selection.
- The runway picker can no longer be closed without a complete arrival + departure selection.

### Steam
- Steam App ID registered (`AppId = 4832540` in `SteamManager`).

### Internal
- Status bar groups converted to HBox containers for proper collapse/show behaviour when group visibility changes.
- `SetAltitudeCommand.AltitudeFeet` doc comment updated: field is pressure altitude.

---

## [v0.13.1] — 2026-06-07

### Voice Commands
- **QNH grammar** — altitude commands now accept an optional `QNH` + 4-digit suffix in the speech grammar (e.g. `"descend to two thousand QNH one zero one three"`). Previously the grammar rejected the phrase before it could reach the parser, making the QNH Requirement setting non-functional for voice. Flight level commands are unaffected — no QNH accepted or required.
- **"feet" suffix** — altitude commands now optionally accept `"feet"` after the altitude (e.g. `"descend to two thousand feet"`), with or without a trailing QNH.

### Bug Fixes
- **Discord "on freq" count on save load** — switched from counting by `JurisdictionId == "EHAM_APP"` to `!ac.IsHandedOff`. The old method returned 0 on save load and in Arcade mode because aircraft are only assigned `"EHAM_APP"` when TOC/REL is explicitly pressed; the new method correctly counts all aircraft still on the player's frequency.

---

## [v0.13.0] — 2026-06-07

### Voice Commands
- **Direct-To voice command** — say `[callsign] direct [waypoint]` to command an aircraft to proceed direct to any navigational fix. Resolves the route index automatically if the fix is in the aircraft's active STAR/SID; otherwise flies direct to the waypoint coordinates.
- **Structured grammar** — `BuildWindowsGrammar()` rebuilt from a flat bag-of-words into per-command grammar builders (heading, speed, level, ILS, hold, direct, resume). Each command has its own recognised pattern. `MaxAlternates = 3` returns up to 3 hypotheses per utterance.
- **Dutch accent alias** — `"sero"` registered as an alias for `"zero"` (Dutch Z → S pronunciation); normalised to `"zero"` in the parser with no other side effects.

### Settings
- **TOC/REL BUTTON mode** (`TocRelArcadeMode`) — new Arcade/Realistic toggle in GAMEPLAY settings:
  - **SHOW FREQ (Arcade, default)**: button shows the next sector frequency; enabled only within 2 NM of the localizer centerline (arrivals) or immediately (departures).
  - **TOC/REL ONLY (Realistic)**: button always shows "TOC/REL", always enabled — no distance gate, no frequency shown.

### Bug Fixes
- **`ShowFasOnFinal` not persisted** — property was declared but missing from `GameSettings.Save()`/`Load()`; setting reverted to `true` on every restart.
- **RTE route line live updates** — `DrawSelectedRoute` and `DrawHoldApproachPath` now use `_sweptPositions` (frozen radar data) instead of the aircraft's live physics position; the route line stays anchored to the blip like all other label elements.
- **HDG/LVL/SPD columns white on load** — `ApplyHighlights(-1,-1,-1)` now called in `CommandPanel._Ready()` so columns are greyed out on scene load before any aircraft is selected.
- **Deleted aircraft continues checking in** — `DevMenu.DeleteSelectedAircraft` now calls `PilotVoice.SuppressCallsign` + `PilotVoice.AcknowledgeForCallsign` before `RemoveAircraft` to cancel any queued or retrying check-in.
- **Discord "on freq" count** — `DiscordManager` now counts only aircraft whose `JurisdictionId` matches the active jurisdiction instead of all aircraft in the world.

### Internal
- Downgraded Godot SDK from 4.6.3 to 4.6.2 for build compatibility.
- Removed macOS/Linux Vosk stub paths from `VoiceRecognizer`; voice commands are Windows-only with no-op stubs for other platforms.

---

## [v0.12.0] — 2026-06-06

### Discord Integration
- **DiscordManager autoload** — publishes Discord Rich Presence via the managed `DiscordRichPresence` (Lachee) library over Discord's local IPC socket; no native libraries. Connects on launch, pumps callbacks each frame, clears presence on quit, and no-ops gracefully when Discord is unavailable or `ClientId` is `"0"`.
- **Live game status** — presence shows `Controlling EHAM` with on-frequency / landed / departed counts and a session timer, read from the live `SimulationManager` via a cached scene-tree lookup (no coupling). Falls back to `In the menus` outside a game.
- **Multiplayer-ready** — join/invite handlers are stubbed in `RegisterJoinHandlers()` so Discord "Ask to Join" can be added over the same IPC channel when multiplayer lands, with no new dependency. The deprecated native GameSDK is intentionally avoided.
- Registered `DiscordManager` as a Godot autoload; added the `DiscordRichPresence` 1.6.1.70 package. Application ID is set in `DiscordManager.ClientId`.

### Branding
- **Logo** — new radar-scope logo showing EHAM's nested CTR/TMA/CTA airspace forms with swept callsign blips and a `MULTISCOPE` / air-traffic-control wordmark. Rendered at 1024 and 512 px to `assets/branding/`; the 512 px version is sized for the Discord art asset (key `logo`) and padded to survive Discord's circular crop. Generated by a reproducible script (`tools/branding/generate_logo.py`).

### Wiki
- Added `Discord-Integration.md`; linked from `Home.md` (Distribution).

---

## [v0.11.1] — 2026-06-06

### Bug Fixes

- **TOC/REL button height** — restored to 34 px, matching adjacent RWY / LOC / APP / RTE / EXE buttons. Previous VBoxContainer split produced a 24 px clickable area. Button now uses `CustomMinimumSize = (0, 34)` with child labels inside via CenterContainer + VBox.
- **TOC/REL frequency text** — frequency label switched from `Label` to `RichTextLabel` with inline BBCode `[color=#FFFFFF]`. BBCode color is resolved by the TextServer below the theme layer and cannot be overridden by the parent Button's inherited `font_color`.
- **TOC/REL action label color** — top label (`_tocRelTopLbl`) now syncs color with button disabled state at the end of every `UpdateTocRelLabel` call (gray when disabled, white when enabled). `font_disabled_color` on the Button only affects `btn.Text`, not child nodes.
- **TOC/REL disabled on load** — `UpdateTocRelLabel(null)` is now called at panel build time so `_tocRelBtn.Disabled = true` is set on the first frame. Previously the button started enabled on a fresh game or save load because `Deselect` never fires when `_selectedCallsign` was never set.
- **TOC/REL live refresh** — state now updates every frame in `LiveUpdate` rather than only on aircraft selection, so transitions (e.g. entering localizer range) are reflected immediately.
- **TOC/REL deselect reset** — `Deselect()` now calls `UpdateTocRelLabel(null)` so the button returns to disabled `TOC/REL` when clicking empty radar space.
- **REL range gate** — `WithinLocalizerRange` uses perpendicular (cross-track) distance to the infinite localizer centerline, not Euclidean distance to the threshold. Aircraft established on a long final at any along-track distance now correctly trigger REL when within 2 NM of the centerline.
- **FAS / MCS dimmed on PendingToc** — `GreyOutColumns` now also applies the dim color to the FAS and MCS buttons so they match the rest of the speed column in non-controllable states.

### UI

- **Action buttons disabled when no aircraft selected** — RWY, LOC, APP, RTE, and EXE show a disabled appearance (darkened background, gray border, gray text) when no aircraft is selected. `SetActionButtonsEnabled(false)` is called at panel build and on every `Deselect`; `true` is called on every `SelectAircraft`.

### DevMenu

- **ILS glideslope state** — ILS readout now shows `GS: active` or `GS: --` alongside `LOC established` to distinguish full ILS from LOC-only.
- **Jurisdiction / TOC fields** — aircraft readout now shows `JurisdictionId`, `PendingToc`, and `IsHandedOff` on one line.

---

## [v0.11.0] — 2026-06-06

### ATC Frequency & Jurisdiction System

- **`JurisdictionId` on every aircraft** — each `AircraftState` carries a string jurisdiction ID referencing an entry in `jurisdictions.json`. Arrivals spawn with `EHAM_ACC`; departures spawn with `EHAM_TWR`. `ContactFrequencyCommand` updates the jurisdiction on handoff; `TakeControlCommand` resets it to `EHAM_APP` on take-back.
- **`frequencies.json` extended** — TWR entries now carry a `runway_group` array; ACC entries carry a `sid_exits` array and a `sector_label` string. Five named ACC sectors with SID exit-fix ownership: Sector 1 North (ANDIK), Sector 2 East (ARNEM / EDUPO), Sector 3 South (LOPIK / WOODY), Sector 4 South-West (IDRID), Sector 5 North-West (BERGI).
- **`AirportFrequencyResolver`** — new static class (`src/Godot/Navigation/AirportFrequencyResolver.cs`) with two helpers: `ResolveArrivalTower(runwayId)` and `ResolveDepartureAcc(sidExitFix)`. Both fall back gracefully to the first entry of the right type when no specific match exists.
- **`FrequencyDatabase` parser** — updated to read `runway_group`, `sid_exits`, and `sector_label` from JSON into the extended `SectorFrequency` record.
- **`SectorFrequency` record** — extended with `RunwayGroup string[]`, `SidExits string[]`, and `SectorLabel string`. Backward-compatible convenience constructor preserved.
- **TOC/REL button rebuilt** — replaced single-line `Button` with a two-line layout: top label (font 14) shows `TOC` / `BACK` / `REL`; bottom label (font 10, gray) shows the target frequency. Labels are children of the `Button` node (`MakeTocRelBtn` helper).
- **Arrival REL gating** — REL is disabled and shown in gray until `WithinLocalizerRange` returns true (aircraft within 2 NM of assigned runway threshold, on approach side).
- **Departure REL immediately available** — departures show REL + ACC frequency immediately after TOC is accepted.
- **`ContactFrequencyCommand`** — new `JurisdictionId` parameter; `Apply()` writes `aircraft.JurisdictionId` when set.
- **`TakeControlCommand`** — new `TargetJurisdictionId` parameter; `Apply()` writes jurisdiction when set. Take-back (BACK button) passes `"EHAM_APP"`.
- **`GameSave`** — `JurisdictionId` persisted in save/load cycle.
- **DevMenu** — aircraft readout now shows `Jurisdiction`, `PendingToc`, and `HandedOff` on one line.
- **Wiki** — `Command-Panel.md` updated with TOC/REL state table, jurisdiction flow, and frequency resolution docs.

---

## [v0.10.0] — 2026-06-06

### Simulation — QNH / FL / TL altitude reference system (full rebuild)

- **Single global QNH** — the simulation now operates on one global QNH value. Per-aircraft `LastAppliedQnh` and the manual QNH re-reference voice command (`"[callsign] QNH 1013"`) have been removed; QNH is applied uniformly and automatically.
- **`PressureMode` per aircraft** — each `AircraftState` carries an explicit `PressureMode` (Qnh / Std), derived every physics tick from the aircraft's true altitude vs the live transition level. Physics, separation, and radar label all use this field.
- **`AssignedLevelFt`** — stores the nominal indicated level of the last ATC clearance (e.g. 7000 for FL70, 3000 for 3000 ft). Never modified by QNH changes. Used as the invariant displayed instructed level.
- **`AssignedLevelIsFlightLevel`** — persistent flag recording whether the controller's clearance was a Flight Level (as opposed to an altitude). Survives FMS overrides of the transient `TargetAltitudeIsFlightLevel` flag.
- **`AppliedQnh` per aircraft** — the QNH the radar has most recently sensed for each aircraft. Weather changes are deferred: `SetQnh` stores the dialed value but touches no aircraft. Each aircraft adopts the new QNH only when the radar beam next sweeps it (`SyncAircraftQnh`), so blip, label, and altitude update together — never ahead of the sweep.
- **Correct altitude reference rules**:
  - STD/FL traffic (above transition level): displayed Flight Level **stays constant** when QNH changes. Only true altitude shifts (aircraft holds its pressure level).
  - QNH traffic (below transition level): displayed altitude **changes** when QNH changes (aircraft holds its indicated altitude, true altitude shifts accordingly).
  - Instructed/cleared level is **always constant** for all traffic — stored as the nominal clearance value and never recalculated from QNH.
- **Transition level** — computed from the live QNH via the published AIP banding table (kept from v0.9). Acts as a display/phraseology boundary only, not an altitude reference.
- **`SyncAircraftQnh`** — new method called at each radar sweep crossing. Shifts both `Altitude` and `TargetAltitude` by the pressure delta so the aircraft holds its pressure level with zero vertical speed. For FL targets this preserves the Flight Level; for QNH targets this updates the true altitude.

### Radar
- **Sweep-gated weather** — all QNH-related altitude and label updates are deferred to the radar sweep crossing for each aircraft. No label or blip changes ahead of the sweep.
- **`SweptData` extended** — `AppliedQnh`, `TargetAltitude`, `LabelTargetAltitudeFt`, and `AssignedLevelHundreds` are now frozen at sweep time. Instructed level display reads exclusively from swept data.
- **Mode-dependent label display** — above the TL: `QnhToPressure(trueAlt, appliedQnh)` → FL (invariant). Below the TL: true altitude directly → altitude (changes with QNH). Both use the aircraft's per-aircraft `AppliedQnh`, not the live dialed QNH.
- **Cleared level invariance** — the right-hand cleared level in the data tag uses `AssignedLevelHundreds` (nominal) when set; falls back to a QNH-aware conversion that is also invariant for FL traffic.

### Command Panel
- All per-aircraft altitude conversions (button highlight, status-bar cleared level, `GetLiveLvlIdx`) use `ac.AppliedQnh` instead of the dialed `SimManager.Qnh`, preventing transient FL jumps between dial and sweep.
- `FormatLevel` TL boundary uses per-aircraft `AppliedQnh` so the F/A prefix does not flip immediately on QNH dial.
- Instructed level in the status bar reads `AssignedLevelFt` directly (nominal, invariant).

### Internal
- Removed `QnhAutoAdjust` setting and its "QNH ADJUSTMENT" toggle in the settings menu — superseded by the automatic sweep-gated model.
- Removed standalone manual-QNH voice command (`VoiceCommandType.Qnh`), its recogniser branch, and its pilot readback.
- `SimulationWorld.Qnh` — world now owns the global QNH, kept in sync by `SimulationManager.SetQnh`.
- `GameSave`: `LastAppliedQnh` replaced by `AssignedLevelFt`, `AssignedLevelIsFlightLevel`, and `AppliedQnh`.
- Dev tool now shows: `Displayed`, `ActualQNHAlt`, `Mode` (Qnh/Std), and `QNH: applied→dialed (pending sweep)`.

---

## [v0.9.0] — 2026-06-05

### Steam Integration
- **SteamManager autoload** — handles Steam init, per-frame `RunCallbacks`, and shutdown, with a safe "Steamless" fallback when the Steam client or native library is unavailable (the game runs normally outside Steam).
- **Native library resolver** — a `DllImport` resolver loads the native Steam library from the Facepunch assembly directory, so it resolves correctly under the Godot host executable in both the editor and exported builds.
- **UnlockAchievement helper** — stub wired for future achievement support.
- SteamManager registered as a Godot autoload.

### Build
- **Platform-conditional Steam references** — Facepunch Win64 wrapper on Windows, Posix wrapper on macOS, with the matching native redistributable (`steam_api64.dll` / `libsteam_api.dylib`) copied to output.
- **STEAMWORKS_ENABLED** is defined automatically per target (`GodotTargetPlatform`) or host OS; builds fall back to a no-op Steamless path otherwise.
- Vendored Facepunch.Steamworks 2.5.2 managed wrappers and native libraries under `lib/steam/`.

### Release tooling
- **SteamPipe upload scripts** (`scripts/steam/`) — app/depot VDF templates, `steam.conf.example`, and `upload.sh`, which fills the templates from `steam.conf` and drives `steamcmd` (with `--export` and `--dry-run` modes).

### Internal
- gitignore: `steam_appid.txt`, `scripts/steam/steam.conf`, generated VDFs, and `build/`.

### Notes
- Uses the `480` (Spacewar) placeholder App ID until the real App ID is provisioned; swap it in `SteamManager.AppId` and `steam_appid.txt` before any public build.

---

## [v0.8.0] — 2026-06-04

### ILS / LOC Approach UX
- **Blue LOC preview on staging** — pressing APP or LOC immediately draws the localizer centerline in blue on the radar, before EXE is pressed. The preview disappears on EXE.
- **RTE shows LOC after clearance** — holding RTE after APP/LOC is executed shows the blue LOC line instead of the STAR route to SPL hold.
- **DCT IF + APP/LOC** — when a DCT to an IF (e.g. PEVOS) is issued alongside an APP/LOC clearance, the aircraft now flies to the IF first and then captures the localizer from there. Previously the DCT was cancelled by the ILS command.
- **IF amber on DCT + APP** — the IF waypoint (e.g. PEVOS) now shows amber immediately after executing a DCT + APP/LOC, without waiting for the next radar sweep.
- **APP status bar color** — the APP/LOC indicator in the status bar is now blue while staged (before EXE) and turns amber after EXE, consistent with HDG/LVL/SPD behavior.

### REL Label
- **Always close to blip** — REL and pre-contact labels now attach 2 px from the blip edge in all states (selected or unselected), with no stem.
- **Orientation via right-click** — right-clicking while a REL label is selected snaps it to a compass direction; the unselected compact label follows the same orientation.
- **Separate orientation dict** — REL labels always default to W orientation on handoff, independent of any pre-REL label position. Right-clicking after REL stores the override separately.
- **Correct compact height scaling** — compact label height uses actual 2-line dimensions, so it scales correctly at all label sizes without overlapping the blip.
- **Shows aircraft type** — compact REL label now shows aircraft type (e.g. B738) instead of squawk code.

### Radar / Command Panel
- **Forced turn readback** — pilot now reads back "turn right heading X" or "turn left heading X" when a turn direction was forced, instead of plain "heading X".
- **No amber HDG for SID/STAR** — the heading column no longer highlights a heading in amber for aircraft following a SID or STAR, since no heading was explicitly instructed.
- **TOC clears check-in** — pressing TOC suppresses any pending or in-progress first-contact call from that aircraft.

### Dev
- **RESTART button in DevMenu** — F12 now has a RESTART button that reloads the current scene without going through the boot sequence or main menu.

---

## [v0.7.0] — 2026-06-04

### New Game Setup
- **Pre-game configuration screen** — pressing NEW GAME now opens a setup menu before the simulation starts. Configure arrival/departure runways, traffic rates (arr/dep per hour), wind direction and speed, and QNH. Settings are remembered between sessions.

### Simulation
- **Vmo enforcement** — aircraft now hard-cap at their `MaxOperatingSpeedKt`. When instructed above Vmo the pilot responds "unable, maximum speed X knots" and the aircraft settles at its maximum. The player's original speed instruction is preserved internally so it resumes automatically if conditions change.
- **ACC handoff free speed** — aircraft handed off to ACC have all ATC speed restrictions cleared. `NaturalCruiseSpeedKt` is floored at the player's last instructed speed so the aircraft never decelerates on handoff — it holds or accelerates to cruise.
- **Check-in suppression** — first-contact call is suppressed if the controller has already issued any instruction to the aircraft, avoiding a confusing double-call.

### Traffic Scheduler
- **Simplified startup** — removed the initial burst system. First spawn now fires immediately and is always an arrival so the player has something to work with straight away.

### Airspace & Jurisdiction System
- **Jurisdiction system** — introduced `jurisdictions.json` per FIR. Defines what altitude limits apply to a controller's aircraft inside each airspace block. Currently contains EHAM_APP. Adding a new position (e.g. EHRD_APP for multiplayer) is a new entry in the same file — no code changes required.
- **Airspace schema overhaul** — replaced the `altitude` string field (e.g. `"GND-3000"`) with explicit `floor` and `ceiling` numbers. Enforcement rules (`altLabelType`, `altLabelRef`) moved out of `airspace.json` entirely and into the jurisdiction file. `airspace.json` is now pure neutral geometry.
- **EHRD CTR level-bust detection fixed** — CTR volumes were excluded from the bust scan entirely. The check now uses the active jurisdiction's rules, so any volume with a rule is enforced regardless of airspace type.
- **EHRD CTR label now visible** — the CTR drawing layer was hardcoded to skip altitude labels; it now honours the TMA labels setting like other layers.

### Radar / Status Bar
- **Status bar shows cleared values** — altitude and speed in the status bar now reflect what the controller cleared (target), not the aircraft's live readout. Speed display also respects constraint mode (OrLess / OrGreater).

### Internal
- `AltLabelType` / `AltLabelFt` removed from `AirspaceBoundary` — boundaries are plain geometry; enforcement is jurisdiction-driven.
- `LowerFt` / `UpperFt` renamed to `FloorFt` / `CeilingFt` throughout.
- `AirspaceRule` and `Jurisdiction` types added to `NavigationDatabase`.
- `ActiveJurisdictionId` / `ActiveJurisdiction` added to `SimulationManager`.
- Traffic scheduler `InitialBurstRemaining` concept removed; `DevMenu` display simplified.

---

## [v0.6.0] — 2026-06-03

### Radar
- **Heading turn preview** — new arcade/realistic setting; when enabled, selecting a heading in the HDG column draws a dashed cyan turn arc (calculated from TAS and aircraft turn rate) plus a 15 NM straight extension showing exactly where the aircraft will roll out. Disappears the moment EXE is pressed. Disabled by default in realistic mode

### UI — Status Bar
- **Reworked status bar** — HDG / LVL / SPD groups are now hidden when an aircraft is first selected; each group appears only when the player stages a value in that column. Label stays white, value is blue while pending and turns amber after EXE confirming the clearance. Deselecting or reselecting the aircraft clears the bar back to runway-only

### UI — HDG Column
- **Passed waypoints removed** — the STAR/SID fix list no longer shows waypoints the aircraft has already flown past; the list trims automatically each radar sweep as the aircraft advances along the route
- **IF list filtered to active runways** — the IF tab only shows intermediate fixes for the currently active arrival runways; refreshes immediately when the runway picker changes
- **Column header colour fix** — HDG / LVL / SPD headers no longer stay blue after the aircraft is deselected or a new aircraft is selected

### Internal
- **LICENSE** — added all-rights-reserved notice (© 2026 Dalí Klaassen and Pim van Gool)

---

## [v0.5.1] — 2026-06-03

### Bug Fixes
- **Parallel hold entry** — now flies the "opposite teardrop" geometry: the outbound (reciprocal) leg straight from the fix, then a single opposite-hold turn back to inbound. This matches the radar preview and the ICAO standard parallel entry. Previously it forced an opposite turn onto outbound, offsetting to the non-holding side and flying a two-turn path

---

## [v0.5.0] — 2026-06-03

### HDG Column — Turn Direction
- **Inline L / R buttons** — turn-direction buttons flank the staged heading and each VOR/IF/SID fix row; hidden until that row is selected
- **Forced turns** — L forces a left turn, R forces a right turn; no selection takes the shortest turn. A second press of the active L or R cancels it
- **Initial turn only** — a forced turn applies to the first turn, then the aircraft is free; it clears automatically once established on the bearing (direct-to) or after the first waypoint (route)
- **HDG / DCT / HLD header** — the column header and status bar now show DCT when a fix is staged and HLD when a hold is staged, otherwise HDG
- **State resets** — selecting a different item clears a staged L/R; deselecting fully resets the header, red X and turn buttons

### SPD Column
- **Contextual +/-** — the OrLess / OrGreater buttons are hidden until a speed is selected; a second press of the active +/- cancels back to an exact speed
- **Unified styling** — staged +/- buttons match the L/R button styling

### Holding
- **Stage a hold** — press H or long-press a holdable fix (HLD / VorHld / IafHold) to stage a hold clearance; sent on EXE
- **Entry previews** — a staged hold draws a dashed racetrack plus the ICAO entry path (direct, teardrop, or parallel) with small direction chevrons on each leg
- **Teardrop fix** — teardrop entries now fly a true 30° offset from the outbound course toward the holding side (previously continued on the arrival track), so the flown path matches the preview

### Traffic
- **Reciprocal runway guard** — selecting a runway blocks its reciprocal (e.g. 18C / 36C) for both departures and arrivals in the picker, with a scheduler-side safety strip. Mixed departures and arrivals on a single runway remain allowed

### Radar
- **Airspace altitude labels** — stacked upper / lower values with floor and ceiling rule styling

---

## [v0.4.2] — 2026-06-03

### SPD Column
- **VMO cap** — SPD column now hides rows above the aircraft's maximum operating speed (`maxOperatingSpeedKt`). VMO itself is shown as the top row. Defaults to 350 kt when the field is absent so existing types are unaffected

### Airspace Labels
- **Floor / Ceiling label types** — `ShowAltLabel` (boolean) replaced by `AltLabelType` (`floor` / `ceiling` / `disabled`) in airspace.json. Floor draws the altitude with a rule below (aircraft must stay above); Ceiling draws a rule above the altitude (aircraft must stay below). Both always display `UpperFt`
- **Clearer rendering** — labels are now size 11 with a dark drop shadow; positioned by measured string width so both number and rule are properly centred
- **Explicit disable** — `"altLabelType": "disabled"` suppresses the label; absent field also means no label
- **airspace.json** — existing `label: true` TMA entries migrated to `altLabelType: ceiling`

### Performance Data
- **aircraft_performance.json** — RECAT WTC values corrected per EUROCONTROL classification; `maxOperatingSpeedKt` added for all aircraft types

---

## [v0.4.1] — 2026-06-03

### Arrival Descent
- **CDA profile** — aircraft now target FL070 from the first tick; label shows FL070 from initial contact. Spawn altitude is still derived from the per-runway IAF crossing window so aircraft arrive at the correct height above the fix
- **Post-IAF descent** — RouteFollower sets FL070 as target when an aircraft passes through an IafHold without holding, covering the non-held path alongside the existing hold-exit trigger in SimulationWorld
- **Player altitude always wins** — all FL070 auto-triggers are guarded by `IsAltitudePlayerControlled`; a player clearance can never be overridden by the descent logic

### Approach
- **LOC → APP upgrade** — pressing APP while established on LOC now stages a full ILS upgrade (requires EXE) instead of staging a cancel; the aircraft no longer has to be re-cleared twice
- **Instant glideslope on upgrade** — `ClearedILSCommand` preserves `IsLocalizerEstablished` on the LOC→APP upgrade so the glideslope activates immediately without a re-arm cycle; eliminates the climb-to-FL070 artefact

### Voice
- **General runway pronunciation** — `SpokenRunway` replaced with a digit-by-digit parser; any runway designator is spoken correctly without a hardcoded list

### Bug Fixes
- **Time scale persists on menu return** — `Engine.TimeScale` now reset to 1× before returning to the main menu; game speed no longer carries over into subsequent sessions
- **Time scale reset on scene load** — DevMenu resets time speed to 1× each time the game scene loads
- **Left/Right arrow keys on Windows** — column navigation keys are now polled each frame instead of handled via `_UnhandledInput`; eliminates the missed first press and slight delay caused by scroll-container focus stealing

---

## [v0.4.0] — 2026-06-02

### Conflict Detection
- **Dual ILS exemption** — two aircraft established on ILS for different runways are fully exempt from LOS alerts; the localizer ensures they cannot collide
- **Diverging departure rule** — departures below 3000 ft with headings differing ≥30° use a reduced 1 NM horizontal minimum while within standard 3 NM; once 3 NM separation is established the standard minimum resumes (ICAO lateral divergence rule)
- **RECAT default gap raised** — same-runway default gap increased from 60 s to 80 s to prevent fast-following departures from briefly breaching 3 NM
- **1500 ft tower floor** — unchanged; either aircraft below 1500 ft suppresses the entire pair

### Traffic / Spawning
- **Faster retry on conflict** — when all 8 placement attempts fail, the scheduler retries in 15 s instead of waiting the full configured interval
- **Arrival 8 NM check removed** — redundant general horizontal check dropped; IAF hold spacing (5–7 NM) is the only arrival constraint
- **DevMenu diagnostics** — scheduler panel now shows consecutive failure count, total spawned, and seconds since last successful spawn

### Simulation Physics
- **Arrival deceleration multipliers** — ×2.5 below 180 kt (flaps), ×2.0 below MCS, ×1.5 above MCS when reduction ≥50 kt (speedbrakes)

### Radar
- **Blip + marker** — shown only for REL (handed-off) aircraft, not ILS-established
- **Label click priority** — active full data-tag label takes click priority over compact pre-contact label
- **Spacing rings corrected** — 1.5 NM radius (3 NM diameter)
- **Measure labels on top** — always drawn above callsigns and data tags
- **Speed label accuracy** — shows instructed `SpeedConstraintKnots` instead of FL100-clamped `TargetSpeed`
- **Show FAS on Final setting** — Arcade: live IAS to FAS from 4 NM; Realistic: frozen on last instruction

### CommandPanel / Highlights
- **Amber stability fixes** — amber no longer vanishes on SPD click, double-click, or after `CommandApplied` resets the cache
- **FAS/MCS amber** — shown on FAS/MCS button rather than nearest numeric speed row
- **TOC blocked for pre-contact arrivals**

### Voice / Readbacks
- **VOR/VorHold direct-to** — NATO phonetics (80%) or spoken name (20%)
- **FAS/MCS readbacks** — descriptive phrases instead of numeric speed
- **Runway pronunciation** — generalized digit-by-digit parser for all designators

### Save / Pause
- **Pause stops audio** — resumes from start of interrupted item on unpause
- **Check-in queue preserved** — queued check-ins restored on load; interrupted item queued first
- **`InterruptedCheckinCallsign`** added to `GameSave`

### Weather Panel
- **RANDOM requires confirmation** — RANDOM populates fields only; CONFIRM applies and advances ATIS

---

## [v0.3.0] — 2026-06-02

### CommandPanel

- **FAS / MCS staging** — FAS and MCS buttons now stage like regular speed rows (blue, requires EXE) instead of firing immediately; clicking again toggles the stage off
- **Staged runway assignment** — RWY picker now stages the runway in blue; EXE confirms and sends `AssignRunwayCommand`; cancels with another RWY press
- **Staged approach cancel** — pressing APP or LOC a second time while on ILS stages a red cancellation (APP/LOC shown in red in the status bar); EXE confirms; pressing again unstages
- **RWY dropdown redesigned** — compact dropdown anchored above the RWY button, same width as the button, no title bar or X; second press of RWY closes it
- **Status bar restructured** — APP/LOC prefix (`_sbAppPfx`) shown only when approach is active or staged; runway (`_sbRwy`) always visible in amber when assigned, blue when staged
- **Status bar seed fix** — `_cachedRwyStr` populated immediately on `SelectAircraft` so runway label appears on the first frame

### Dev Menu

- **Full UI style refresh** — VCR OSD monospace font applied everywhere; thick white-border buttons; section headers match `CommandPanel` pattern; emojis removed
- **Phraseology 2-column layout** — phrase buttons in `GridContainer(2)` per section, full column width, centred text
- **STATUS section** — live: version, sim speed, paused flag, QNH, wind, ATIS, runways, aircraft count, score, scheduler rate, next-spawn countdown
- **COMMS section** — live voice queue: currently-speaking callsign + text, up to 6 queued items tagged RBK/CHK; phraseology button presses override for 6 s
- **Descent path button guarded** — feedback shown if no aircraft selected or no assigned runway
- **AC info tab expanded** — SID restrictions ahead, forced turn flag, speed/altitude assignment details, hold inbound course and timer, all flags consolidated, per-spawn instance values

### Radar

- **Selected REL aircraft label** — clicking a handed-off aircraft shows the full 4-line label in grey in the fixed W orientation; deselecting reverts to the compact 2-line label

### Simulation

- **SID speed cap released on vector/direct** — `SetHeadingCommand` and `DirectToFixCommand` clear `SidSpeedCapActive` immediately; vectored/off-route aircraft get natural profile speed via `RouteFollower.NaturalProfileSpeed`
- **Route-direct past restriction** — `DirectToFixCommand` clears `SidSpeedCapActive` when jumping past a restricted waypoint

### Performance

- **Cruise altitude rounded** — `CruiseAltitudeFeet` rounded to nearest FL10 at resolve time (previously raw `ceilFl × 0.85`)
- **Cruise altitude synced** — `perf.CruiseAltitudeFeet` updated to actual assigned FL after ACC handoff

### Internal

- `NaturalProfileSpeed` extracted from `EnforceSpeedRestriction` as a public static helper
- `TrafficScheduler` exposes `SpawnTimerElapsed`, `NextSpawnInterval`, `SecondsToNextSpawn`, `InitialBurstRemaining`, `GameTimeSec`
- `PilotVoice` exposes `IsSpeaking`, `CurrentCallsign`, `CurrentText`, `ReadbackQueueCount`, `CheckinQueueCount`, `PeekQueue()`
- `SimulationManager` forwards `.PilotVoice` and `.Scheduler`

### Bug Fixes

- **Status bar runway invisible on selection** — runway label now driven directly in `SelectAircraft`, not waiting for next radar sweep
- **Status bar runway/APP prefix** — replaced `Visible` toggling with text-only control; Godot layout reflow was the underlying cause
- **`AssignedRunwayId` cleared by routing commands** — `DirectToFixCommand`, `ClearedHoldCommand`, and `ResumeOwnNavCommand` no longer clear `AssignedRunwayId`; directing to a fix or clearing for hold does not change the expected landing runway; this also fixes runway assignments becoming null after save/load for any aircraft that received one of these commands

---

## [v0.2.7] — 2026-06-01

### Save / Load Fixes
- **DirectToX/Y now saved** — aircraft instructed to fly direct-to a fix (VOR or IF) survive a save/load and continue steering to that fix instead of reverting to spawn routing
- **Trail history saved per aircraft** — radar trail dots are stored in the save and restored on load via a `TrailHistoryProvider` callback from `RadarScope`
- **Aircraft appear instantly on load** — sweep cache (`_sweptPositions` / `_sweptData`) is pre-populated for all active aircraft on load; no longer need to wait for the beam to pass over each blip
- **Traffic per hour and runway selection correctly synced to UI** — the LVL traffic targets and runway picker button states now reflect the loaded values immediately via the new `OnGameLoaded` deferred event
- **Save slot UI** — clicking a filled slot now shows DELETE and LOAD buttons; selecting another slot or pressing BACK collapses the expanded slot; `SaveManager.DeleteSave()` added

### Radar Sweep / Pause
- **Sweep freezes immediately on pause** — the sweep angle no longer advances while `SimManager.IsPaused` is true; exactly zero extra sweeps happen on pause
- **Trail dots preserved while paused** — trail history is no longer overwritten with stationary positions during pause

### RTE Button (Hold to Reveal)
- **Vectored aircraft** — holding RTE shows an infinite dashed light-blue heading line along `TargetHeading`
- **DCT aircraft** — solid line to the direct-to fix; hold racetrack overlaid only if the aircraft is also cleared to hold at that fix
- **Route-following aircraft** — remaining STAR waypoints shown as a solid line; published hold racetrack drawn at `HLD` / `VorHld` fixes (e.g. SPL); suppressed when the player has given DCT to that fix without a hold
- **Holding aircraft** — racetrack shown at `HoldFixIdent` for any fix type, including `IafHold` (ARTIP, RIVER, SUGOL) when explicitly instructed

### Traffic
- **Initial burst spawn** — first 4 aircraft spawn 15 s apart after loading a new game so the player can process them one by one; normal traffic/hour rate resumes after the burst
- **Press-and-hold +/− buttons** — traffic-per-hour +/− buttons now support press-and-hold: 450 ms initial delay then 80 ms repeat

### LVL Column (Transition-Level Aware)
- **TL inserted as selectable FL** — the transition level (e.g. F045 at QNH 1013) is added to the LVL list; previously only multiples of 10 were available
- **F/A label follows TL** — entries at or above TL show `F` (flight level); entries below show `A` (altitude). F040 becomes A040 when TL ≥ 45, for example
- **Dynamic rebuild on QNH change** — the LVL column rebuilds automatically whenever `CalcTl(qnh)` changes; staged altitude preserved across rebuild

### Airspace Display
- **CTA boundaries** — now solid green (same colour as TMA/CTR) drawn on top of TMA lines
- **FIR boundaries** — solid blue, drawn on top of CTA lines
- **Layered draw order** — TMA → CTR → CTA → FIR; each layer has its own edge-dedup set so shared edges are redrawn in the correct colour for the upper layer

### Weather Randomizer
- **Probability-weighted distribution** — wind direction 80 % from 150°–310°, wind speed 80 % from 4–16 kt with 15 %/5 % tails, QNH 80 % from 1005–1025 hPa with tail bands; all uniform within each band

### Runway Picker / Controls
- **Clickable runway indicators** — the active runway boxes in the HUD now open the runway picker directly; the separate chevron button has been removed
- **Red X closes the picker** — the X button in the picker header closes the panel (previously reset the selection)
- **Map pan button setting** — new CONTROLS section in Settings lets you choose left or right mouse button for map panning (default: right)

### Misc
- **BACK button in save overlay** — "CANCEL" renamed to "BACK" in the in-game save panel

---

## [v0.2.6] — 2026-06-01

### Check-In Queue & Pilot Calls
- **Per-aircraft retry timers** — reworked the check-in queue so one aircraft's "waiting for a reply" window no longer freezes the whole frequency. Each aircraft that calls unanswered runs its own background retry timer; other aircraft can call during the silence, matching a real half-duplex frequency where the channel is only busy while someone is actually transmitting
- **Courtesy gap** — a short 3–5 s beat is left after each check-in before the next one plays, giving the controller time to key up and respond. Readbacks (player responses) bypass and clear this gap
- **Two check-in attempts before "do you read?"** — an unanswered aircraft now replays its full check-in phrase once (second attempt) before escalating to "do you read?"; after two "do you read?" calls the retry window stretches to 2 minutes
- **Randomised pre-speech pause** — base 60–180 ms with an occasional longer hesitation (≤ 100 ms extra, 30 % chance) instead of a fixed delay

### Pilot Readbacks from the LID
- **LID commands now get spoken readbacks** — instructions issued via the command panel (heading, level, speed, direct-to, ILS clearance) produce a pilot readback just like voice commands; a combined EXE plays one readback covering all staged columns
- **FAS / MCS buttons read back** the exact per-aircraft speed assigned

### Phraseology
- **NATO phonetic callsigns** — callsigns with no airline telephony name are now spelled with the NATO alphabet ("Kilo Lima Mike one two three") so the TTS pronounces them correctly; letter suffixes are also spelled (previously dropped)

### Separation
- **No separation loss below A015** — aircraft below 1500 ft are in the Tower's jurisdiction; radar separation alerts (LOS / warning / advisory) are suppressed for any pair where either aircraft is below the threshold

### Command Panel
- **SPD column default scroll** — positions the live speed near the top of the visible window so the MCS row and FAS button stay on screen without scrolling

---

## [v0.2.5] — 2026-05-31

### Aircraft Performance Database
- **Full database rebuild from Excel** — JSON now contains 56 aircraft types (was 49); 7 types were missing and falling back to DEFAULT performance: AT72, CRJ2, E175, A30B, CL30, CL60, E55P
- **Save/load performance bug fixed** — aircraft loaded from a save were losing their type-specific performance (FAS, MCS, climb rates etc.) and silently reverting to the DEFAULT entry; `LoadState` is now saved per-aircraft and the full performance object is re-resolved from the JSON database on load
- **`LoadState` added to save format** — old saves default to `Standard` load on first load under this version

### SPD Column (Command Panel)
- **Speed list filtered by FAS** — rows below the selected aircraft's Final Approach Speed are hidden; the lowest visible row is always strictly above the rounded-up FAS
- **FAS button is now functional** — clicking `FAS 140` issues a `SetSpeedCommand` with the exact per-aircraft Vapp (load-randomised); button label shows the actual computed FAS value
- **MCS row added** — a `MCS 210` row is inserted at its correct position in the speed list and issues the exact per-aircraft Minimum Clean Speed when clicked; hidden when no aircraft is selected or MCS falls below the FAS cut-off
- **Live amber highlight fixed for arrivals** — spawned arrivals at 250 kt with no player speed constraint now show the amber highlight on the 250 row (falls back to `TargetSpeed` when `SpeedConstraintKnots = 0`)

### Airborne Speed Rates
- **Separate airborne acceleration and deceleration rates** — distinct from takeoff/landing performance; randomised per aircraft at spawn within fixed ranges:
  - Departures: accelerate 0.5–1.0 kt/s, decelerate 1.0–1.5 kt/s (heavy, climbing)
  - Arrivals: accelerate 1.0–1.5 kt/s, decelerate 0.5–1.0 kt/s (lighter, descending)
- Ground-roll physics (takeoff roll) continue to use the type-specific `accelerationMs2` from the performance database

---

## [v0.2.4] — 2026-05-31

### QNH Fixes
- **Pilot FL readbacks now correct at non-standard QNH** — `Phraseology.Level()` now accepts a `qnh` parameter and converts QNH altitude to pressure altitude before computing the flight level number; all check-in phrases pass the live QNH
- **Non-player-controlled FL targets re-referenced on QNH change** — STAR aircraft and newly spawned aircraft that had not yet received a player altitude command were not having their `TargetAltitude` updated when QNH changed mid-game; removed the erroneous `IsAltitudePlayerControlled` guard

### Traffic
- **Arrival spawn speed raised to 250 kt** (was 240 kt) — both the profile `TargetSpeed` and the aircraft state `Speed` / `NaturalCruiseSpeedKt` fields updated

### Radar Labels
- **Departure IAS hidden until speed assigned** — departing aircraft in the `Departure` or `Climb` phase show a blank IAS field on line 4 until the player issues a speed instruction; `IsSpeedPlayerControlled` is the gate
- **Label repositioning via right-click** — with an aircraft selected, right-clicking anywhere on the radar map snaps the label to the nearest of the 8 compass positions (N/NE/E/SE/S/SW/W/NW) based on the angle from the blip to the click point; replaces the old left-click-to-cycle behaviour; right-clicking on a blip still toggles its speed vector; right-clicking with nothing selected still pans

### Settings Menu
- **ARCADE / REALISTIC column headers** on the GAMEPLAY and RADAR sections — each section header shows `ARCADE` (amber) and `REALISTIC` (teal) labels aligned above the left and right toggle buttons; toggle pairs reordered so arcade option is always left, realistic is always right
- **DISPLAY section** consolidates label size and airspace opacity controls, previously embedded in the RADAR section; RADAR toggles are now a sub-section within DISPLAY
- **Airspace opacity live preview** — each CTR / TMA / CTA / COASTLINE slider now shows a 50 px coloured line at the slider's current value, using the same border colours as the radar scope
- **Label size preview updated** — NE orientation (blip lower-left, label upper-right) with fixed stem/blip positions; blip rendered as a circle outline (`DrawArc`) matching the in-game style; label text spacing grows/shrinks with the selected size while geometry stays fixed

---

## [v0.2.3] — 2026-05-31

### Transfer of Control (TOC) Mechanic
- **New "TOC MECHANIC" setting** (Settings → Gameplay, default off). When enabled, every spawned aircraft starts *pending TOC* — the controller must press TOC/REL to formally accept it before issuing any command
- Pending-TOC aircraft show a **white callsign with a dimmed data block** (lines 2–4) and a white stem; the blip stays white
- LID columns are **greyed out** while the selected aircraft is pending TOC, handed off, or when no aircraft is selected — the player can still click but commands are silently dropped until control is accepted
- Check-ins still play as normal; TOC is purely about accepting control
- `PendingToc` persisted in saves

### Arrival Descent Logic
- Arrivals now receive a **table-driven IAF crossing altitude** based on their landing runway and IAF (ARTIP / RIVER / SUGOL) — a full 7×3 window table for every EHAM arrival runway (e.g. 18R SUGOL → FL090–FL100)
- When two arrival runways are active simultaneously, **combination overrides** lower specific windows (e.g. 36R RIVER FL080–FL090 → FL070–FL075)
- A random integer FL is chosen within the window (no rounding to 5/10)
- Arrivals spawn exactly **7 NM before the IAF** on the inbound track (was 15 NM with jitter)
- Spawn altitude = chosen crossing FL × 100 + a random 1500–2500 ft, so the aircraft is in a **continuous descent** to the IAF with no level segment at FL100
- After the hold is exited, aircraft default to **descend to FL070** unless the controller issues an explicit altitude

### ILS & Approach
- **Final approach speed fix** — inside 4 NM, aircraft now decelerate to the type-specific Vapp (`FinalApproachSpeedKt`) instead of the higher generic approach speed; the speed-restriction validator was corrected to match

### Holding
- **Hold-exit turn fix** — issuing a heading (or ILS / direct-to / resume-nav) to an aircraft in a hold now turns the shortest way to the new heading instead of carrying over the hold's forced turn direction and taking a huge arc

### SID Departures
- **Speed cap with wings-level release** — once any SID max-speed restriction lies ahead the speed is hard-capped at that limit; after the last restricted fix the cap holds until the aircraft rolls wings-level onto the next leg, then releases to cruise speed

### Pilot Audio
- **Handed-off aircraft go silent** — after the final "contact X on Y" readback, no further check-in or pilot audio plays for that callsign while it is on another frequency

### Aircraft Labels
- **Wake-turbulence categories completed** — B787, A350, A300/310, B757, B747-300, 737 MAX, A320neo family, A220, and CRJ-1000 now show the correct WTC/RECAT letter instead of defaulting to L/D

### Command Panel & Labels — Responsiveness
- **Instant label & status updates** — control/command state (ILS clearance, APP establishment, REL, TOC) now reflects in the radar label and LID status bar immediately instead of waiting up to 4 s for the next radar sweep. Physical radar data (position, altitude, ground speed, trail) remains sweep-paced
- The selected aircraft's allocated-runway **IF appears at the top of the IF list** (e.g. PEVOS first when landing 18R)

### Fixes
- Windows arrow-key scrolling reworked: initial press handled on key-down (echo-filtered), hold-repeat driven by polling — fixes both the over- and under-shoot reported on Windows

---

## [v0.2.2] — 2026-05-30

### QNH & Altitude
- **Spawn altitude bug fixed** — newly spawned arrivals had their pressure altitude converted to QNH altitude at spawn, so labels and physics are correct at any non-standard QNH
- **QNH Adjustment setting** (Settings → Gameplay): choose between *Automatic* (all aircraft above the TA re-reference instantly when QNH changes) or *Manual (Voice)* (each aircraft keeps its old altimeter until the player gives it the new QNH individually)
- **Voice QNH command** — say `[Callsign], QNH one zero zero three` to re-reference a single aircraft in manual mode; pilot reads back the four digits
- `AircraftState.LastAppliedQnh` tracks per-aircraft QNH reference so the manual delta is always exact across multiple QNH changes

### SID/STAR Route Following
- **Speed look-ahead** — aircraft now scan all upcoming waypoints and begin decelerating before a restricted fix rather than only reacting at the fix itself
- `IsAltitudePlayerControlled` no longer auto-clears when following a STAR; player level clearances remain in effect until `RESUME OWN NAV` is issued
- `ResumeOwnNavCommand` now clears both `IsAltitudePlayerControlled` and `IsSpeedPlayerControlled` so FMS restrictions resume correctly after handoff

### Transition Level & Labels
- TL formula replaced with the published AIP lookup table (ICAO standard): `band = ⌈(1032 − QNH) / 18⌉ + 1`, TL = TA_hundreds + band × 5 — QNH 1013 now correctly gives FL045 for EHAM TA 3000 ft
- Radar labels convert QNH altitude to pressure above the TL and show altitude + "A" below it; cleared altitude field and handed-off tag use the same logic
- CommandPanel status bar and LVL column use TL QNH altitude as the threshold instead of the hardcoded TA

### Radar — Dev Menu
- **Per-runway SID overlay** — the SID button in the dev menu now cycles through `OFF → ALL → 18R → 18C → 22 → …`, showing only the SIDs for the selected departure runway; runway list populated dynamically from NavDatabase

### Settings
- **Label size** (S / M / L / XL) with a live label preview added to the Settings menu

### UI Fixes
- HDG drum scroll: fixed Up-direction jump caused by wrong clone-selection threshold
- SPD column keyboard: falls back to `TargetSpeed` when no constraint is set so arrow keys always have a valid starting position
- CommandPanel Up/Down key-repeat: leading-edge fire on first frame + polling only — prevents Windows OS key-echo double-fires

---

## [v0.2.1] — 2026-05-30

### Command Panel
- `\` key stages an APP clearance; Enter executes it via the existing EXE path
- APP button text turns blue when a clearance is staged, matching HDG/LVL/SPD behaviour
- LID amber highlight now shows the **last instructed** value (TargetHeading, TargetAltitude, SpeedConstraintKnots) rather than the current aircraft value
- SPD amber absent until the player explicitly assigns a speed
- LVL column removes altitude levels A025/A015/A010/A005; retains only A030 and A020
- LVL column defaults to FL130 when activated on a departure aircraft
- HDG column tripled to 216 entries for seamless infinite drum scroll

### SID Procedures
- Fixed `min_alt_ft` acting as a ceiling instead of a floor — departures were incorrectly levelling off at noise-abatement gate altitudes
- Added `IsSpeedPlayerControlled` flag: player speed clearances permanently override SID max-speed restrictions; persisted in saves
- Corrected all abbreviated SID identifiers in `sid_division.json` to match `procedures.json` exactly

### Radar — Departure Runway Indicator
- Active departure runways now show a short 2 NM indicator in the same style as ILS approach lines
- Fixed non-ILS runways (36L, 18L, 24, 09, 04) being silently skipped
- 18C/36C mutual exclusion: only one drawn at a time; 18C shown by default

### Radar — Label Accuracy
- Aircraft labels now show raw pressure altitude (Mode C) — QNH conversion removed from main tag, cleared altitude field, and handed-off tag

### Data
- Removed AMS VOR from waypoints.json

---

## [v0.2.0] — 2026-05-28

### Traffic & Spawning
- Schedule-driven spawning: real callsigns, aircraft types, and airport pairs from an offline schedule database
- Entry/exit points resolved via a flight route division table (80/20 split by runway combination)
- Traffic load UI: dep/arr +/- buttons; TOTAL randomises the split between 20/80 and 80/20 limits
- ACC climb-out: delayed climb clearance issued when a departure is handed to EHAM CTR; FL derived from SID maximum, performance ceiling, and ICAO semicircular rule

### Physics & Simulation
- Full IAS → TAS → GS physics chain — aircraft movement and turn radii based on True Airspeed
- Altitude-varying wind model (WindModel.cs)
- Automatic night mode via real wall-clock time (solar declination + civil twilight)
- Upper-wind mode: NORMAL / STRONG / JET, selectable in the WX panel
- 250 kt speed cap applied automatically when descending through FL100 or spawning below it
- Vector SIDs added for EHAM RWY 36C: BERGI and IDRID exits

### Command Panel — Keyboard Navigation
- Tab activates HDG → LVL → SPD; Left/Right switches columns; Up/Down scrolls; Enter executes all staged clearances
- HDG column activates automatically on aircraft selection
- Shift+Up cycles HDG mode in reverse; holding Up/Down repeats
- LVL column scrolls to near-top on activation
- Two-state highlight: browsing = blue text; confirmed (Tab) = top+bottom border
- Confirmed border clears on arrow scroll; mouse click shows it immediately
- Tab stay-on-SPD triggers once when all three columns are confirmed, then cycles freely
- Type-ahead digit jump while a column is active
- Mouse click deactivates keyboard mode
- No auto-deselect on EXE; removed 10-second auto-deselect timer

### Radar & Display
- Animated main menu with live EHAM radar background
- APP/LOC state in the LID status bar; HDG/LVL/SPD hidden when no aircraft is selected
- IAS always shown in the radar label, rounded to the nearest 10 kt
- Heading 360 no longer displayed as 000
- Speed vector (PTL) default set to 1 minute; computation fixed (was using IAS instead of TAS/GS)
- Show All SIDs and Show All STARs overlays in the dev menu

### APP / Direct-To
- APP and LOC clearances staged alongside HDG/LVL/SPD and sent together on EXE
- Direct-to VOR/IF now correctly navigates to the fix; hands off to player vector on capture (0.5 NM)

### Save System
- 3-slot save/load with slot labels and timestamps; auto-save on exit
- Continue button always active; shows empty slots when no saves exist

### Settings & UI
- Settings menu: 10 configurable options (voice label freeze, QNH requirement, TL protection, TMA altitude labels, level-bust detection, opacity sliders)
- Esc pause menu redesigned — deferred add_child, correct z-order, no black screen on quit
- WX panel and RWY picker UI aligned visually with the CommandPanel
- Available runways: 3 px border; unavailable: dim 1 px border
- Fixed RWY selector X button marking all runways as selected
- Lock symbols on menu buttons; multiplayer placeholder

### Bug Fixes
- Fixed LID scroll position resetting while player browses after aircraft selection
- Fixed aircraft blinking on spawn due to false level-bust detection
- Fixed keyboard nav becoming unreliable after clicking panel buttons on Windows
- Fixed add_child-during-_Ready crash on Windows
- Fixed pilot voice infinite retry loop and readback priority ordering
- Fixed "Do you read" capped at 2 calls (15–30 s gap), 2-minute gap thereafter
- Fixed level-bust blink incorrectly flagging uncontrolled aircraft
- Fixed missing commas in the offline schedule JSON
- Removed transition-altitude descent protection (to be redesigned)

### Voice Recognition
- Windows: System.Speech with grammar-constrained recognition
- macOS: Whisper, Vosk, Sherpa-ONNX, and Apple SFSpeechRecognizer all evaluated and removed
- NATO phonetic fallback registered for every callsign
- Orange NO MIC indicator on PTT if microphone is unavailable

---

## [v0.1.12] — internal

### Internal
- Merged five single-purpose stub files into their logical homes; no gameplay changes

---

## [v0.1.11] — internal

### Internal
- Moved SectorFrequency record into NavigationDatabase.cs; no gameplay changes

---

## [v0.1.10] — internal

### Internal
- Removed duplicate EhamFrequencies.cs; frequencies.json is now the sole source of truth

---

## [v0.1.9]

### Radar
- History trail dots are now white, larger, and brighter

---

## [v0.1.8]

### Radar
- Fixed overlapping dashed lines along shared TMA boundary edges near LOPIK and ARTIP

---

## [v0.1.7]

### Simulation
- Fixed FL vs QNH altitude reference at the transition altitude
- Added cancel approach command

### Internal
- Removed runway field from STARs; cleaned up dead code

---

## [v0.1.6]

### Radar
- New VIX waypoint type: fixes that should appear on the radar scope can be flagged explicitly

---

## [v0.1.5]

### Navigation
- IAF-based runway allocation for arriving aircraft
- RWY 22 temporary activation support

---

## [v0.1.4] — internal

### Internal
- Removed dead TrafficPanel and fixed stale slider comment

---

## [v0.1.3] — internal

### Internal
- Full codebase quality pass; no gameplay changes

---

## [v0.1.2]

### Traffic
- Replaced simultaneous-count model with arrivals/departures per hour rate model

---

## [v0.1.1]

### Navigation
- FMS-style descent path prediction with base-turn arrivals and vectored ILS intercepts
- Track miles measured along actual curved path; level-off buffer randomised 1–5 NM per aircraft before the IF

---

## [v0.1.0] — First Playable Build

### Core
- EHAM (Amsterdam Schiphol) — full navigation database: runways, waypoints, airspace, SIDs, STARs, procedures
- Aircraft physics: heading, altitude, speed, wind at 100 Hz
- ATC command system: vectors, altitude, speed, ILS, holds, direct-to, hand-offs
- Route following with fly-by turn anticipation and speed/altitude restrictions
- ILS approach: localizer capture, glideslope descent, distance-based speed profiles
- Holding patterns: all three ICAO entry types, wind-compensated timing
- Conflict detection: STCA with 3 NM / 1000 ft ICAO minima, 2-minute predictive advisory
- 49 aircraft performance envelopes (narrowbody, widebody, heavy)
- Voice control on Windows: grammar-based, offline, no cloud required
- Pilot readbacks with radio audio chain (squelch clicks, static hiss)
- Plan-view radar scope with labels, vectors, ILS lines, airspace boundaries, conflict symbology
