# Changelog

Player-facing release notes for **MultiScope ATC**. Newest first.

This is the curated, plain-English changelog — it highlights what changes for you as a controller. Minor internal and tooling work between these versions is not listed.

---

## v0.27.4 Keyboard shortcuts, scroll-wheel pickers, custom difficulty, and voice fixes

### New

- **Mappable keyboard shortcuts for the action buttons.** EXE, RTE, LOC and RWY can now be triggered from the keyboard and rebound in Settings > Controls (defaults: EXE = Enter, RTE = Space, LOC = /, RWY = '). With the runway popup open you can move through the runways with the up and down arrows, press Enter to select one, and Enter again to execute.
- **Scroll wheel on the pickers.** Hover the HDG, LVL or SPD picker and scroll to change the value: scroll up to increase, scroll down to decrease.
- **Peek at a route with a long press.** Hold left-click on an aircraft's blip or label to show its route while you hold, without pressing RTE.
- **Pan with an aircraft selected.** Right-drag now pans the scope even when an aircraft is selected, so you no longer have to deselect first. A quick right-click near the aircraft still moves its label.
- **Custom difficulty.** Change any of the difficulty-linked settings and the preset now shows CUSTOM. Your exact setup is saved and restored on restart, and the four named presets always start you on their settings.
- **Restart in the pause menu.** A new RESTART button (with a confirmation) starts a fresh run with the same settings and new traffic.
- **Smarter heading column for departures.** Selecting a departure that flies a published (RNAV) route now opens the heading column on its route instead of a full list of headings. The moment you start turning it, the heading list opens so you can pick a heading and execute.
- **Departure list tidy-up.** The list stays pinned to the bottom-right corner at any window size, the header row is restyled, and the runway is shown in white. Its heading column is now blank for departures that fly a fixed route, and shows a heading only for departures that depart on a heading.
- **Kuwait Airways (KAC)** added, read back as "Kuwaiti".
- The range and traffic readout in the bottom-left now shows only with the developer menu open, to keep the scope clean.

### Fixes

- Fixed a bug where transferring an aircraft by voice ("contact tower", "contact approach") did nothing at all.
- Fixed a bug where saying "radar contact" or "identified" to an aircraft that had just checked in didn't take it under control or stop its check-in call, forcing you to use the button or Tab.
- Fixed a bug where two aircraft a safe 1000 feet apart in altitude could still trigger a wake-turbulence separation loss.
- Fixed a bug where raising the render scale pushed the side command panel off the top and bottom of the screen and changed its width.

---

## v0.27.3 — Phraseology reference, approach fixes, and display options

### New

- **Phraseology reference card.** A **?** button on the main menu and pause menu opens a searchable list of the controller commands, grouped by type, with the parts you fill in highlighted and their valid ranges shown. Alternate ways of saying each command are tucked behind a toggle.
- **In-game quick-reference cheat-sheet.** A small, draggable panel that shows the relevant commands for whatever you have selected — arrival vs departure — so you always have the phraseology handy. It can be shown or hidden in Settings (and defaults to shown on the easier difficulties).
- **Simpler voice recognition confidence.** One setting (Strict / Normal / Relaxed / Lenient) now applies to every command consistently — lower it if your accent or setup is getting missed.
- Bug reports sent from the game now include more detail, so we can track down issues faster.

### Fixes

- Fixed a bug where an aircraft held its assigned speed all the way to touchdown instead of slowing down to land — it now reduces to its approach speed on short final (hold 180 to about 6 miles, or 160 to about 4).
- Fixed a bug where the instructed speed disappeared from an aircraft's label once it slowed for landing.
- Fixed a bug where the heading arrow keys couldn't wind an aircraft the long way around a full 360 — you can now keep tapping ‹ or › past the reciprocal, while normal nudges still keep the shortest turn.
- Fixed a bug where ultrawide (21:9) and super-ultrawide (32:9) window resolutions weren't available for windowed play (fullscreen already renders at your screen's native resolution on any monitor).
- Fixed a bug where an aircraft established on the localizer but held **above** the glideslope triggered a false wake-separation loss.
- Fixed a bug where a go-around kept drawing its missed-approach line back to a fix it had already flown over.
- Fixed a bug where showing an aircraft's route (RTE) while it was being vectored or holding hid the SPL VOR and other navaids.
- Fixed a bug where "contact tower" or "contact Amsterdam" read back the wrong frequency instead of the one the aircraft was actually handed to.
- Fixed a bug where giving a departing aircraft a landing runway didn't bring back its approach fix.
- Fixed a bug where, with Voice Auto Label Update off, you had to nudge a value away and back before pressing EXE would update the data tag — one press now syncs it.
- Fixed a bug where rebinding a game-speed key back to 1–5 reverted to F1–F5 after restarting.

---

## v0.27.2 — Administrative label mode, new settings, and a big batch of fixes

### New

- **Administrative label mode.** With **Voice Auto Label Update** turned off, the data tag, LID and BID now behave like your own notepad: they show only what *you* enter through the LID/BID — voice clearances no longer change them. You keep the label in sync yourself, the way a real controller does. Departures show FL60 / no heading / no speed on check-in until you enter your instructions.
- **New setting — Infringement Look-Ahead.** Choose how far ahead the airspace infringement warning projects an aircraft's staged track.
- **New setting — Heading Preview Length.** Choose how long the dashed heading-preview line is.
- **Voice: "and" and "stand by".** You can now use "and" as a filler ("…and reduce speed…"), and say "stand by" to an aircraft.
- **Game-speed shortcuts are now F1–F5** by default (still rebindable).
- **Better bug reports.** Reports now include the last 10 instructions you gave the selected aircraft, and suggestions/praise now go to their own feedback channel. The feedback form also asks you to pick a type before sending.
- **Departure list** now has a column header row (RWY · EOBT · CALLSIGN · TYPE · SID · HDG).
- The heading list now **centres on the aircraft's current heading** when you select it, so the heading you want is right there.
- Settings sliders redrawn so the bar is actually visible and clearer to read.

### Fixes

- Fixed a bug where two aircraft both established on the **same runway's ILS** could trigger a false separation-loss alert.
- Fixed a bug where a go-around aircraft **didn't tell you why** it was going around.
- Fixed a bug where a go-around aircraft **wouldn't climb** when instructed after a runway conflict.
- Fixed a bug where a go-around aircraft's label showed the **wrong cleared altitude** (now shows the missed-approach level).
- Fixed a bug where a go-around aircraft left an **endless dashed line** and hid the SPL VOR after you gave it a new heading.
- Fixed a bug where an aircraft **asked to climb again after you'd handed it off** to the next sector.
- Fixed a bug where a **departure was offered an arrival approach fix** it shouldn't have.
- Fixed a bug where **"Contact" was shown as "Kontact"** on screen.
- Fixed a bug where an aircraft landing on **36R was given the wrong tower frequency**.
- Fixed a bug where a spoken **"direct [fix]" was understood but not carried out**.
- Fixed a bug where **greyed-out (too-high) altitudes could still be selected** with the keyboard.
- Fixed a bug where the **per-aircraft runway dropdown could get stuck open** when switching aircraft.
- Fixed a bug where typing on an aircraft you **hadn't taken control of** would stage a heading.

---

## v0.27.1 — Playtest hotfix

Fixes from the first round of playtest reports — thank you for sending them in.

- Fixed a bug where achievements you had earned did not show as unlocked on the medals wall.
- Fixed a bug where aircraft would go around for no clear reason even when spacing was more than enough, sometimes going around repeatedly.
- Fixed a bug where departing aircraft often did not make their initial check-in call.
- Fixed a bug where an aircraft would sometimes check in a second time even after you had already given it an instruction.
- Fixed a bug where an aircraft that went around would not climb when instructed, got stuck, never called Approach back, and showed a flashing label.
- Fixed a bug where the Spijkerboor (SPY) departure was read aloud letter-by-letter instead of by name.
- Fixed a bug where an inactive runway's localizer line stayed on the scope when "active runways only" was turned on.
- Fixed a bug where the game did not fill ultrawide (21:9) monitors, leaving a black bar down the side.
- Fixed a bug where voice-recognition setup errors were confusing — they now explain the cause and how to fix it, with a new setup guide in Settings → Audio.

---

## v0.27.0 — Playtest is ready: join the community

The playtest now has its own Discord, and claiming your **Playtester** role takes one click.

### Community

- **Get your Playtester role in one click.** From the main menu, hit **PLAYTESTER DISCORD**, authorize in your browser, and you're in the server with the Playtester role — no codes to copy or paste. It's tied to your Steam playtest access, so it just works.
- **You're in control of your data.** Once you're in, the button becomes **Disconnect Discord** (also under Settings → About / Legal) — one click removes your role and deletes the link between your accounts, whenever you like.

### Feedback

- **Better bug reports.** The in-game feedback tool (press **F8**) now includes more technical detail with each report so we can fix things faster. You still choose what to send, and nothing leaves your machine until you press Send.

### Stability

- **Crash-safe saves.** A crash or power loss while saving can no longer corrupt your saved game.
- **More reliable startup.** The game now degrades gracefully instead of failing to launch if any bundled data is off.

Thanks for helping test — see you in the Discord!

---

## v0.26.13 — Voice, labels & smarter traffic

### Voice

- **Speed readbacks say the right thing.** Minimum-clean and final-approach-speed clearances now read back "reducing" or "increasing" to match the actual change.
- **"Speed 220 or less" just works.** You no longer have to say "maintain" first — "speed 220 or less" (and "or greater") is understood on its own.
- **Labels update the instant you're understood.** Heading, direct-to and approach clearances now update the aircraft tag as soon as the command is understood, the same way speed and level already did — no waiting for the readback.
- **Two label fixes.** The heading tag no longer shows a wrong value when auto label update is off, and an aircraft no longer makes its initial call after you've already instructed it.

### Traffic & radar

- **Shorter turns on re-direct.** Sending an aircraft direct to a fix and then direct to another fix mid-turn now takes the shorter way round.
- **Both departure runways at once.** With two departure runways in use, departures are now spread across both in parallel instead of one runway working through its queue before the other starts.
- **Better pre-loaded traffic.** Arrivals that are already in the air when you start now sit on the correct straight-in track and at a realistic, already-descending level instead of holding high.
- **Cleaner airspace labels.** Removed a duplicate altitude label sitting next to the control zone.

### Feedback

- **More useful bug reports.** In-game feedback now includes your hardware, display and recent log details — no personal data — to help pin down issues faster.

---

## v0.26.12 — Smoother arrivals, sharper voice & radar

### Traffic & simulation

- **Arrivals fly a proper speed profile.** Inbound aircraft now hold 250 kt on the way in and only start slowing once they are close to final, with a "reducing speed" call — instead of quietly bleeding speed off far out.
- **General-aviation departures fly SIDs.** GA departures now follow a real departure route like everyone else, and their departure runway shows in the aircraft info panel.

### Voice

- **Your GA runways are understood.** Saying a general-aviation landing runway — like "runway two two" or 36R — is now recognised.
- **Handoff without a frequency works.** "Contact Amsterdam" or "Contact Tower", with no frequency, now transfers the aircraft.
- **Red means "understood, but can't".** When the game understood your instruction but couldn't carry it out, the voice readout turns red — so it's clear it wasn't a mis-hear.

### Radar

- **No more phantom airspace warnings.** The infringement warning now only triggers on the altitude limits actually shown on the label, so an aircraft above a hidden ceiling no longer sets it off.
- **Traffic you don't own looks like it.** Aircraft that aren't on your frequency yet stay greyed out until they check in (in automatic under-control mode).

### Fixes

- You can now type number-row digits in the in-game feedback box.

---

## v0.26.11 — New difficulty tier & clearer settings

### Difficulty & settings

- **New Professional difficulty.** There are now four presets — **Beginner, Enthusiast, Professional, Expert** — with Professional sitting between Enthusiast and Expert. It's offered on first launch and can be changed any time in Settings. Every preset's mix of realistic and assisted options was re-tuned.
- **Clearer setting names.** Several gameplay options were renamed to say what they actually do: **Transfer Frequency**, **Under Control (UCO)**, **Auto Speed Change in Label**, **Airspace Restriction Labels**, **Airspace Infringement Warning**, and **Short Term Conflict Alert**. Your existing choices carry over.
- **Speed in the label: Live or Instructed.** Auto Speed Change now works across the whole flight. **Live** shows the aircraft's actual speed (rounded to 10 kt) as it speeds up or slows down; **Instructed** shows only the last speed you gave — "or greater" as +1, "or less" as −1 — with the real change happening quietly in the background.

### Airspace

- **Sharper airspace warnings.** The infringement warning now flags a staged vector that would drop an aircraft below a stepped TMA's floor (or above its ceiling), as well as one that would send it into a neighbouring sector — and it no longer false-flags a descent that levels exactly at a floor.
- **Restriction labels follow the pressure.** Airspace floors set to the transition level now track the current QNH.

### Aircraft & voice

- **"Speed X or greater" speeds back up.** An "or greater" instruction is now a floor, not a fixed target — the aircraft accelerates back to its normal speed instead of getting stuck at whatever it was doing when you said it.
- **Acknowledged aircraft stop re-calling.** Once you've dealt with an aircraft, it won't keep repeating its check-in; a fresh call after a go-around still comes through.
- **Voice speed qualifiers work.** Saying "two two zero or greater" is now understood as a floor instead of a fixed 220, and frequencies spoken without "decimal" (e.g. "one two one five") are recognised.

### Feedback

- **Optional Discord name.** The in-game feedback form now has a remembered Discord field — enter your user ID to be pinged when we reply. Reports also attach the full data of the aircraft you have selected.

### Traffic

- Added **Air Serbia** to the airline list.

---

## v0.26.10 — Manual label updates (INST/ADMIN)

### Voice & labels

- **New INST / ADMIN switch on the command panel**, shown when Voice Auto Label Update is off. After you instruct an aircraft by voice, flip to **ADMIN** and press EXE to bring its data tag in line with what you said — without re-sending the instruction or triggering a second readback. **INST** keeps the normal behaviour (issues the clearance and gets a readback). It sits beside the BID/LID/TID buttons, colour-coded amber for INST and blue for ADMIN.

### Holding

- **Correct holding leg timing down the stack.** A holding aircraft's inbound leg now shortens from 90 to 60 seconds as it descends through FL140, for both your holds and published holds.

---

## v0.26.9 — Landings, approach speeds & spacing

### Approach & landing

- **Aircraft land reliably again on high-pressure days.** ILS traffic no longer hangs a few hundred feet above the runway — it descends through the touchdown gate and lands.
- **Firmer, realistic approach speeds.** Arrivals hold their speed until they need to slow, then reduce cleanly, instead of drifting slow from far out.
- **"180 knots or greater" releases on final** so the aircraft can slow to final-approach speed for landing.

### Descent & spacing

- **Straight-in arrivals hold higher until they reach you.** Preloaded straight-in traffic stays at FL070 until it enters your airspace instead of descending too low in the neighbouring sector.
- **Published crossing restrictions stay put.** A restriction at or above the transition level is treated as a flight level, so it no longer drifts when the pressure changes.
- **Cleaner approach spacing.** The spacing arcs rank downwind traffic more honestly.

### Control & sectors

- **Strays count sooner.** An aircraft that wanders into a neighbouring sector counts as a violation even before you've accepted it.
- **"Contact tower" respects range.** A voice handoff of an arrival to Tower is only accepted once it's within localizer range, matching the REL button.

### Radio

- **QNH still read back.** With QNH not required, pilots still read back the pressure setting on their first descent through the transition altitude.

### Radar & weather

- **Uniform airspace borders.** Overlapping boundaries of the same type draw at a single opacity instead of stacking darker.
- **New default airspace opacities** on first launch (CTR, TMA, CTA, MILL, coastline).
- **Wind stays within limits.** The random-weather button keeps the wind clear of every active runway's crosswind and tailwind limits.

---

## v0.26.8 — Snappier readbacks & control polish

### Radio

- **Pilots reply the instant you instruct them.** Readbacks and responses to your commands — handovers ("contact …, good day"), "confirm climb/descend?", "unable / request QNH", "unable, maximum speed …" — now come straight back instead of waiting a couple of seconds behind other radio traffic.
- **Pausing silences the frequency.** No pilot call plays while the game is paused; the interrupted transmission picks up again when you resume.
- **Clearer direct-to phrasing.** Sending an aircraft direct to a fix now reads back as "direct {fix}". The "resume own navigation, proceed direct {fix}" phrasing is reserved for when an aircraft you'd vectored rejoins a point on its own route.

### Controls

- **One-tap FL130 climb on the advanced panel too.** The first level-up on a departure jumps straight to FL130 on the LID as well as the BID.
- **Direct-to shows its route on the BID.** Picking a fix in the BID DIRECT column now draws the track on the radar — straight to the fix, plus the rest of the SID when the fix is on the aircraft's route.
- **Steppers keep a steady speed when fast-forwarding.** The traffic-per-hour selector and the heading/level/speed steppers no longer race when you speed the sim up.

### Radar

- **Semi-active runway buttons no longer flicker.** They blink cleanly and at a steady rate regardless of sim speed.
- **Route (RTE) labels are readable.** Showing an aircraft's route no longer double-prints its waypoint names over the map labels.
- **Departure list shows the plan for aircraft on a heading.** A vectored departure now lists its heading plus its exit point as a short code (e.g. `H123 BRG`).

### Flying

- **Approach won't ask for QNH twice.** An aircraft already down on the local pressure setting (or already descending onto it) won't be asked for QNH again on the next low-altitude clearance.
- **Straighter preset arrivals.** The preset ARTIP-to-TIDVO arrival (runway 27) now tracks a clean straight line in.
- **Schiphol control-zone ceiling.** The approach sectors of the Schiphol CTR are now capped at 3000 ft (adjusting the change from the previous update).

---

## v0.26.7 — All-caps display & quicker departure climbs

### Look & feel

- **The whole interface is now in capitals** — a cleaner, more authentic ATC-display look across menus, the command panel, and every radar data tag.

### Controls

- **One-tap standard departure climb.** On the basic input panel (BID), the first time you raise a departure's level it jumps straight to **FL130** — the standard initial climb — instead of creeping up one step at a time. From there the arrows fine-tune to any other level as usual. (A departure already above FL130 just steps normally.)
- **The UCO/REL button tells you what it does** — **UCO** when you can take an aircraft under control (or take back one you handed off), **REL** once you're controlling it in realistic (hidden-frequency) mode, or the **handoff frequency** in arcade mode.

### Airspace

- **Approach can now use the full height of the Schiphol control zone** — the old 3000 ft cap in the approach sectors has been removed.

### Radio

- **Dutch pronunciation of "contact."** The voice command now also understands "kontakt" / "kontact" when you tell a pilot to contact another frequency.

---

## v0.26.6 — "Under Control" rename & input fixes

### Taking and handing off aircraft

- **"TOC" is now "UCO" (Under Control).** The transfer-of-control button and setting have been renamed throughout — same mechanic, clearer name. Your existing settings carry over automatically.
- **The UCO/REL button tells you what it does.** It shows **UCO** when you can take an aircraft under control, **REL** once you're controlling it in realistic (hidden-frequency) mode, or the **handoff frequency** in arcade mode — so the button always matches the action available.

### Controls

- **Direct-to now works from the basic input panel (BID).** You can send an aircraft direct to a waypoint from the BID, not just the advanced panel.
- **Cleaner BID speed selection.** Final approach speed is now the slowest option you can pick (no unrealistic sub-approach speeds), the top of the range is capped at the aircraft's maximum speed just like the advanced panel, and the selector wraps around from the slowest stop back to the fastest.

### Radio

- **Reduce to final approach or minimum clean speed by voice.** You can now say "reduce to final approach speed" or "reduce to minimum clean speed" and the pilot slows to that aircraft's correct speed.

---

## v0.26.5 — Correct handoff frequencies, natural readbacks & keyboard polish

### Radio & readbacks

- **Give the right frequency or nothing happens.** When you tell a pilot to contact another frequency, they only act on it if it's the frequency they'd actually be handed to next — the tower for their runway, or the area sector for their departure. Read the wrong one and the instruction is simply not understood, so you can't accidentally hand an aircraft to the wrong controller.
- **Pilots read fixes back more naturally.** Waypoints are read the way real crews say them — pronounceable names as words ("SUGOL"), three-letter beacons spelled out or occasionally by their spoken name ("sierra papa lima" / "schiphol"), and RNAV points as "alfa mike zero nine four".

### Flying & guidance

- **Published altitude restrictions respect the current QNH.** "At or above / at or below" limits on SIDs and STARs are now read against the actual pressure setting, so they hold aircraft at the correct height on high- or low-pressure days.
- **Turns you set stay the way you set them.** Nudging a heading with the keyboard keeps the turn going the direction you started — a small correction the other way no longer flips the aircraft into a full turn the wrong way round. The dashed preview line always shows the direction the aircraft will really fly.
- **Proximity warnings match the preview.** The warning for vectoring an aircraft toward a neighbouring sector now follows the exact same predicted path as the dashed heading line, so what you see is what's checked.

### Radar & controls

- **ILS approaches read APP on the tag.** A full ILS clearance now shows **APP** from the moment it's armed, instead of switching from "ILS" to "APP" only once established. Localizer-only approaches still read **LOC**.
- **Heading, level and speed lists follow the keyboard.** Stepping a value with the keyboard now scrolls its column to keep your selection in view, so you can always see what you're setting.
- **Wake category shows RECAT** on the aircraft tag when RECAT wake separation is turned on.

---

## v0.26.4 — Realistic approach speeds & smarter sector handoffs

### Approach speeds

- **Arrivals now fly a real deceleration profile on their own.** With no speed instruction given, an arrival smoothly slows toward 1.3× its final approach speed by 10 miles out, to 160 knots by 6 miles, then down to final approach speed inside 4 miles — instead of just holding whatever speed it last had.
- **Speed restrictions expire on their own.** Give an aircraft a hard speed restriction below FL100 and it won't fly it forever — the pilot will resume the normal approach speed once established close enough to the localizer, announcing "reducing speed" when they do.
- **New NORMAL button.** Clear a speed restriction and hand control back to the aircraft's own profile from either the button panel or the keyboard speed selector — previously only possible by voice.

### Sector handoffs

- **More accurate jurisdiction handoffs.** Which sector an aircraft belongs to is now worked out from its actual entry or exit fix, fixing edge cases where the wrong sector could be shown as receiving an aircraft.
- **Airspace proximity warnings are a little more forgiving** near a neighbouring sector's boundary, and now also catch aircraft cruising toward one without any altitude change staged.

### Accessibility & quality of life

- **Colourblind mode now covers the main menu too**, not just the live radar screen.
- **New microphone test** in the settings menu so you can check your input level before jumping into a session.
- **Fixed a voice-recognition bug** where a correctly spoken QNH readout could be silently ignored instead of accepted.

---

## v0.26.3 — Colourblind support, cleaner airspace warnings & fixes

### Accessibility

- **Colourblind display modes.** New deuteranopia / protanopia / tritanopia options recolour the airspace boundaries (TMA, CTR, CTA, FIR) with a colourblind-safe palette so the lines stay easy to tell apart from the background and each other. Off by default — pick your mode in **Settings → Radar**.

### Turns, speeds & the data tag

- **Aircraft turn the way you tell them.** Choosing left or right next to a heading now actually flies that direction for the turn onto the new heading, instead of always taking the shortest way round.
- **FAS and MCS in the BID.** The button-input speed selector now steps into final-approach speed and minimum-clean speed, sitting in the list at their real speed values.
- **Vectored departures keep their exit fix.** A departure flying a heading off a vector SID still shows its sector-exit point, so you can send it direct.
- **Voice labels update as you speak.** With auto label update on, the data tag shows your new clearance the moment the game understands it — the aircraft still complies after the pilot reads back. With it off, the tag now holds heading, level **and** speed until you set them yourself.

### Airspace warnings

- **No more false warnings over neighbouring airspace.** Legally overflying Rotterdam's TMA at 3,000 ft, or holding near SUGOL up at FL70, no longer flashes a warning — the game now checks the full 3D shape of each sector (floor and ceiling included) before flagging anything.

### Fixes

- **Arrivals land reliably at any QNH** — a low-pressure bug that could leave an aircraft hanging just above the runway is fixed.
- The 18C/36C centreline stays on **18C by default** and only flips to 36C when 36C is landing.
- Spacing crossbars and range read-outs now update in step with the radar sweep, so they no longer jitter.

---

## v0.26.1 — Frequency discipline & sharper separation

A radio-realism update: only one aircraft talks at a time, and separation now behaves the way the rulebook says it should.

### One frequency, one voice

The shared radio channel now behaves like a real party-line.

- **Pilots wait for a clear frequency.** An aircraft won't check in until the frequency has been silent for a couple of seconds, so calls no longer step on each other.
- **Key over someone and you'll hear it.** Transmit while a pilot is already talking and you get the classic **blocked heterodyne squeal** — the two transmissions garble, just like the real thing.
- **You have to answer them first.** An aircraft can't be given instructions until it has checked in with you — the transfer button, keyboard commands and voice all wait until it's on frequency. Selecting an aircraft also gives you a few seconds' grace before it re-calls.
- **Steadier re-calls.** An aircraft you haven't answered now re-calls on a consistent ten-second rhythm instead of drifting out to long, uneven gaps.

### Separation that matches the book

- **On the line is separated.** Two aircraft sitting *exactly* on the required distance now count as separated — you only lose separation once the gap drops **below** the minimum, not the instant you touch it.
- **Departures behave in the climb.** A departure following another now stays on its ground-based wake spacing until it climbs through **2,000 ft**, rather than switching the moment its wheels leave the ground.
- **Inbound spacing respects wake.** Traffic feeding in at ARTIP, SUGOL and RIVER is now spaced by whichever is greater — the standard gap or the wake requirement for that specific pairing — so a light aircraft behind a heavy is never fed in too tight.

### Voice recognition

- More reliable speech recognition on Windows, including a fix for **non-English Windows** installs where the command grammar could fail to load.

---

## v0.26.0 — General aviation, smarter runways & realistic descents

Schiphol gets general-aviation traffic with its own runway logic, a new "semi-active" runway state, and more natural descents. This release also rolls in everything since v0.25.0.

### New in this release

**General aviation comes to Schiphol**

Business jets and light aircraft — Gulfstreams, Citations, Learjets, the Pilatus PC-12 and PC-24, the Dornier 328 and more — now fly to their own runways and behave differently from the airliners.

- **Arrivals** ignore the usual entry-point logic and head for **RWY 22**. When 22 isn't available — because it's out on the wind, or another runway it conflicts with is in use — they're sent to **RWY 36R** instead.
- **The 36R break-off.** About **three in four** GA arrivals sent to 36R fly the ILS down to around 300 ft, then leave the glideslope, cross the 36R threshold at ~150 ft and make a **visual right turn to land on the short RWY 04**. Their label stays on 36R the whole way, and if one goes around it flies the correct missed approach for wherever it was — 36R before the break-off, 04 after.
- **Go-arounds re-plan.** If a GA arrival goes around, the game immediately works out its new landing runway and updates the label straight away — it might not be the runway it was on before.
- **Departures** have their own runway order: **RWY 04** by default (with a lower initial climb of 3,000 ft), falling back to 09, 22 or 18L depending on what's in use. A RWY 04 departure waits for GA break-off traffic on final and for a spell after a 06/36R go-around; a RWY 22 departure waits for arrivals on short final; and when RWY 24 is the departure runway, 22 and 24 are sequenced together so they don't go head-to-head.

**Semi-active runways**

Runways can now be in a half-way state instead of just on or off.

- Deselect a runway that still has aircraft committed to it and it becomes **semi-active**: it shows as a **dashed-green box** in the runway menu, its localizer and locator stay on the scope, and its traffic keeps landing — but nothing new is sent to it. Once the last aircraft is down it blinks three times and goes dark. A note in the runway menu tells you which runway is winding down.
- The same state powers GA traffic on runways you haven't selected: a GA arrival or departure lights its runway up (blinking in), and a GA **departure** runway appears about **ten minutes ahead** — the moment the flight shows in the departure list — then switches off once it has departed and climbed through 2,000 ft.

**More realistic descents**

With the Realistic descent setting, vectored arrivals below FL100 no longer use one fixed rate. Each aircraft picks a comfortable descent rate and plans **where to level off** on the way to 2,000 ft — gentler descents level off earlier, steeper ones carry on further out — so the flow down final looks a lot more natural. (Arcade descents are unchanged.)

**Start with traffic already flowing**

A fresh game can **pre-load the airspace** with a few minutes of traffic already in progress, so you drop straight into a live picture with aircraft on the arrival and departure flows instead of waiting for an empty scope to fill.

### Also since v0.25.0

- **Radio phraseology overhaul.** Pilots now read back with varied wording instead of one fixed phrase every time, and a "descend" instruction that would actually be a climb (or the other way round) gets a **"confirm climb/descend?"** query back instead of being flown the wrong way. Read-backs also match the aircraft's state and your QNH setting.
- **First-run welcome & help.** New players are greeted and pick **Arcade or Realistic** to start, which sets sensible defaults; you can change it later. Every setting now has a small **"?"** explaining what it does, including what each side of the Arcade/Realistic toggles changes.
- **Ground view (early).** A first look at an airport-surface (ground radar) view with taxiing aircraft, its own zoom and pan.
- **Levels of separation.** Aircraft that are procedurally separated — departures on diverging SIDs, parallel missed approaches, a missed approach against the parallel localizer, and similar — no longer trip **false separation losses**.
- **RWY 24 departure hold.** A RWY 24 departure now waits on the ground while an 18C arrival is on short final, and for two minutes after one goes around.
- **New commands.** Cancel a hold to rejoin the route, and resume normal (own) speed to hand speed back to the procedure.
- **Fixes.** Wake-separation alerts no longer fire for aircraft still on the ground; handed-off labels keep their side; approach-while-vectoring re-arms correctly; every landing counts even when the aircraft taxis clear; and the New Game setup steppers support press-and-hold.

---

## v0.25.0 — Fresh menus, your career, real-world traffic & tailwind go-arounds

A big one. As well as this release's new features, it brings together everything since v0.24.0 — a full menu redesign, a controller career with ranks and medals, and a real-world traffic mode.

### New in this release

- **Wind that works both ways.** Aircraft now watch the tailwind as well as the crosswind. A strong tailwind can push a landing aircraft into a go-around, and pilots will call up when they can't land in it. Departures wait on the ground while their runway has too much tailwind, and the runway picker turns red or orange when a runway is out of — or close to — its wind limits.
- **Go-arounds tell you why.** When an aircraft goes around it now says the reason — runway not clear, wake turbulence, too much crosswind or too much tailwind — and repeats it when it checks back in on the missed approach. Give one a heading and it drops the missed-approach procedure to become a normal arrival again; the missed-approach path shows in yellow the first time you select it.
- **See your departures coming.** A new departure list on the radar shows the upcoming departures for each active runway — when they'll appear, their callsign, type and SID — so you can plan the outbound flow before they even spawn.
- **Rebind your controls.** Every keyboard shortcut is now listed under Settings → Controls, and you can rebind the ones that matter — hold, approach, expedite, push-to-talk, the radar toggles, sim speed and more. Click a key, press a new one; if it clashes with another shortcut they simply swap.
- **Know what every setting does.** A small "?" next to each setting explains what it does — and for the arcade/realistic options, what each side changes.
- **Set your own track-line length.** A new slider sets how far ahead the predicted track lines project by default (still adjustable live with the [ and ] keys).
- **Name your saves.** Saving now lets you type a name for the slot and warns you before overwriting an existing save.
- **Sharper separation and scoring.** Departures climbing away on diverging routes are handled more realistically, a brief re-loss right after you've fixed a conflict isn't double-counted, and every landing now counts — even when the aircraft taxis clear of the runway.
- **Coming soon.** New Game now shows a **Live ADS-B** traffic mode as a preview of what's next.

### Also rolled in since v0.24.0

- **A fresh look across the menus.** Settings, New Game and Career have been rebuilt around a clean category sidebar with a consistent teal/green palette and restyled scrollbars throughout. Settings groups everything under Video, Radar, Gameplay, Audio, Accessibility, Controls and Career; New Game and Career now share the same tidy layout.
- **Your controller career.** Earn ranks from Trainee all the way up to Area Director as you handle more traffic, with an XP bar toward the next tier. A medals wall tracks your achievements (and keeps them even offline), a Recent Sessions log lists your last 20 sessions, and lifetime stats now include your longest clean streak and total miles controlled.
- **Real-world traffic.** A new Real World mode plays a real daily schedule at the airport — pick a start time and how busy you want it, and aircraft appear on their real schedule with real callsigns and tail numbers. Custom traffic now pulls real callsigns and registrations too, and you'll see an aircraft's registration in its info panel.
- **Clearer approach spacing.** The final-approach spacing markers now appear earlier — as soon as an aircraft is lining up on the ILS — and sit steadily on the localizer. The aircraft info panel also shows the arrival runway and the arrival's entry/exit fixes.

---

## v0.24.0 — Scores, stats & your controller career

Track how you're doing — in the moment, at the end of each session, and across your whole career.

- **A clearer score panel.** The in-game score now splits into Traffic (arrivals, departures, transits, total, and time played) and Performance (separation losses, wake losses, sector violations, and your total score), colour-coded so you can read it at a glance.
- **End-of-session report card.** Pause the game and open Statistics for a letter grade from S to F with a one-line verdict, your safety percentage, most common incident, and how this session stacks up against your personal best.
- **Your controller career.** A new Career screen on the main menu keeps lifetime stats — total aircraft handled, hours controlling, safety record, best and busiest sessions, favourite runways, and the aircraft types, airlines and destinations you see most. It fills in as you play.
- **Smoother go-arounds.** Aircraft that go around now do the right thing when you give them a new heading, route or approach: the heading shows on the tag, they re-intercept the localizer for another try, and the missed-approach line clears once you take over.
- **Pilots take a beat.** After you press EXE, aircraft now wait a second or two before starting the manoeuvre — like a real crew reading back, then acting. Your readback and the label update instantly.
- **More weather variety.** Starting a new game now rolls the QNH along with the wind, instead of always sitting at 1013.
- **Fixes & polish.** Better voice handling of altitudes above the transition level, the runways you pick in setup now show correctly in-game, paused games no longer nag you with "do you read?", and an all-round tidier control panel.

---

## v0.23.0 — Wake separation, expedite climb/descent & in-game feedback

Two new tools for managing your traffic, plus a quick way to tell us what you think — right from the radar.

- **Expedite climb and descent.** Tell an aircraft to climb or descend at its maximum rate when you need it to get a move on. Press **E** (or long-press the level on the control panel); the pilot reads back "expedite climb/descent." Re-selecting a level normally cancels the expedite.
- **Wake turbulence separation.** Aircraft now need wake spacing behind heavier traffic. A red trail and **WAKE** marker show when a follower is too close, final-approach spacing shows a marker for where the next arrival should sit, and an aircraft that catches wake on final will go around ("going around, due to wake"). Lined-up departures also wait the required time behind the one ahead. Losing wake separation costs points on the new score panel, and you can turn the whole system on or off in Settings (ICAO or RECAT rules).
- **Send feedback from inside the game.** Press **F8** (or use the pause menu) to send a bug report, suggestion or note without leaving the game. Pick a category, add a title and message, and optionally include a screenshot of your current radar — it goes straight to the team. Nothing personal is sent.
- **Consistent dropdown menus.** All the dropdown menus across the game now share the same clean look.

---

## v0.22.0 — Simplified control panel (BID)

A new, beginner-friendly way to direct aircraft, sitting right next to the detailed panel you already know.

- **New "Basic" input panel.** Switch between the full list panel and a cleaner, simpler one using the panel buttons above the command bar — handy when the detailed layout feels like a lot, especially while you're learning.
- **Direct-to a fix in one click.** A single DIRECT list shows the arrival's route fixes, navaids and approach fixes together; click one to send the aircraft straight there. Press and hold a holding fix to put it in the hold.
- **Simple heading, level and speed steppers.** Nudge heading, altitude and speed up or down with arrow buttons, or clear a setting with its red ✕. The aircraft's current target shows in amber and your pending change in blue, just like the detailed panel.
- **Nothing else changes.** Same EXE confirmation, same keyboard shortcuts, same flying — only the way you enter instructions is simpler. Switch back to the full panel any time.

---

## v0.21.0 — Ground movement & taxi control

Aircraft now live on the airport surface — watch them land, taxi in, and taxi them yourself.

- **New ground (airport) map.** Press **G** to switch to a top-down view of Schiphol's taxiways, aprons, stands and runways. Pan and zoom to follow the action on the surface.
- **See the whole arrival.** Aircraft appear on the ground map as they come down final approach — touchdown, rollout, and turning off the runway onto a taxiway are all shown.
- **Aircraft taxi in instead of vanishing.** After landing, an aircraft clears the runway, taxis past the holding point and waits for your instructions instead of disappearing.
- **Give taxi clearances.** Click an aircraft on the ground and clear it to a stand, choosing exactly which taxiways it should follow — including routes that double back over the same taxiway.
- **Runway crossings.** When a taxi route crosses a runway, the aircraft holds short and asks to cross; you issue the crossing clearance and it continues.

---

## v0.20.4 — Landings, crosswind and go-arounds

The biggest update yet to how arrivals behave at the runway.

- **Aircraft now land and roll out.** Arrivals no longer vanish at the threshold — they flare, touch down, brake, pick a runway exit and taxi clear. A landed aircraft shows as a grey blip until it leaves the runway.
- **Realistic spacing guidance.** A "do-not-cross" marker is drawn on final showing where the next arrival should sit so it lands only once the runway ahead has vacated.
- **Crosswind matters.** Strong crosswinds can block a runway in the picker, suppress departures, and make pilots call "unable to land due to crosswind." Out-of-limits approaches go around.
- **Go-arounds and missed approaches.** Arrivals can go around (crosswind or insufficient spacing) and fly the runway's published missed-approach procedure, then come back for another try.
- **Voice and pilot speech now work in the Windows build.** Push-to-talk voice control and neural pilot read-backs are correctly included in the packaged Windows version.

---

## v0.19.0 — New game setup overhaul

- **Pick a traffic intensity** — Quiet, Normal, Busy or Overload — with realistic arrival/departure rates.
- **Automatic runway combinations** matched to the traffic load, so a new game is realistic out of the box (you can still adjust runways yourself).
- **Weather tied to the runways in use**, with Calm / Normal / Gusty conditions rolled at start.
- **Pilots wait while you work an aircraft** — incoming check-ins hold off when you're busy with someone else, instead of talking over you.

---

## v0.18.0 — Updated navigation data & smarter traffic

- **Latest navigation cycle (AIRAC 2606)** — updated runway 24 departures and waypoints.
- **Smarter arrival runway allocation** that matches real published logic, including sensible behaviour when runways are at capacity.
- **Departures fly their full routes** in more runway combinations.
- **Shorter radio pauses** between transmissions for a snappier frequency.

---

## v0.17.0 — Audio mix, subtitles & achievements

- **Volume sliders** for Master, Voice and Effects so you can balance pilot speech against radio noise.
- **Pilot subtitles** — optional on-screen captions of every pilot transmission (Accessibility setting).
- **Steam achievements** — 8 to earn, from your first landing to 1000 cumulative landings.

---

## v0.16.0 — Autosave & Steam Cloud

- **Automatic saves** running alongside your three manual slots, so a crash or accidental exit won't lose your session. Load it from the Continue screen.
- **Steam Cloud support** — saves and settings sync across machines.
- Improved dev/staging tools for setting up scenarios.

---

## v0.15.0 — Setup presets & spoken handoffs

- **Traffic and weather presets** in the setup menu for quick configuration.
- **Spoken sector handoffs** — pilots read back the real sector name ("amsterdam", "tower") on a frequency change.
- Voice recognition cleanly reports as Windows-only on other platforms instead of failing silently.

---

## v0.14.0 — Rock-steady altitudes

- **Rebuilt altitude/pressure model** so an aircraft's flight level stays constant when you change the QNH, exactly like the real world — only the values that should move, move.
- **Readback speed slider** to set how fast pilots talk, with a live preview.
- **"Expect runway" voice command.**

---

## v0.13.0 — Direct-to by voice

- **"[Callsign] direct [waypoint]"** voice command to send an aircraft straight to any fix.
- **More reliable voice recognition** with per-command grammar, plus a Dutch-accent alias for "zero".
- Several command-panel and save/load fixes.

---

## v0.12.0 — Discord Rich Presence

- **Show your live session on Discord** — your profile displays "Controlling EHAM" with on-frequency, landed and departed counts and a session timer.
- **New radar-scope logo.**

---

## v0.11.0 — Frequencies & control handoffs

- **ATC frequency and jurisdiction system** — realistic handoffs to Tower (arrivals) and Area Control (departures), with named ACC sectors owning specific departure exits.
- **Rebuilt TOC/REL handoff button** showing the target frequency.

---

## v0.10.0 — Full QNH altitude model

- **Complete pressure/altitude rebuild** — aircraft hold their correct levels as you change the altimeter setting, and updates appear in step with the radar sweep rather than instantly jumping.

---

## v0.9.0 — Steam groundwork

- **Steam integration foundation.** The game continues to run normally outside of Steam.

---

## v0.8.0 — Approach & ILS polish

- **Live localizer preview** — pressing APP or LOC draws the blue centerline immediately, before you execute the clearance.
- **Direct-to an intermediate fix then intercept the ILS** in one go.
- Clearer labels for handed-off aircraft, including aircraft type.

---

## v0.7.0 — Pre-game setup screen

- **New Game configuration** — choose arrival/departure runways, traffic rates, wind and QNH before you start; remembered between sessions.
- **Aircraft respect their maximum speed** — instruct one too fast and the pilot replies "unable, maximum speed X knots."
- Cleaner first-contact behaviour.

---

## v0.6.0 — Heading turn preview

- **Turn preview** — staging a heading draws a dashed arc showing exactly where the aircraft will roll out (optional; off by default in realistic mode).
- **Reworked status bar** that shows only what you're actively clearing.

---

## v0.5.0 — Holds, turn directions & speed constraints

- **Stage a holding clearance** with a live preview of the racetrack and the correct ICAO entry (direct, teardrop or parallel).
- **Left / Right turn buttons** to force a turn direction on the first turn.
- **"Or less" / "or greater" speed constraints.**
- **Reciprocal runway protection** — selecting a runway blocks its opposite end in the picker so you can't activate a head-on pair by accident.

---

## v0.4.0 — Smarter conflict detection

- **Better separation logic** — aircraft established on different ILS approaches no longer trigger false alerts, and diverging departures are handled to the ICAO rule.
- **Smoother arrival deceleration** modelling flaps and speedbrakes.
- Radar and pilot-readback polish.

---

## v0.3.0 — Command panel staging

- **Consistent staging** — final-approach speed, minimum clean speed, runway assignment and approach cancellation all stage in blue and confirm on EXE, just like heading/level/speed.
- Major dev-menu refresh.

---

## v0.2.0 — Real traffic & systems update

A large update that turned the prototype into a proper simulation.

- **Schedule-driven traffic** — real callsigns, aircraft types and airport pairs from an offline database.
- **Full physics chain** — indicated → true → ground speed with an altitude-varying wind model (Normal / Strong / Jet upper winds), automatic night mode from the real time of day, and an automatic 250 kt speed cap below FL100.
- **Approach and direct-to clearances** staged alongside heading/level/speed and sent together on EXE.
- **Full keyboard navigation** for the command panel.
- **3-slot save/load** with autosave on exit.
- **Settings menu** and an animated main menu with a live radar background.
- **Windows voice recognition** — offline, grammar-based, no cloud required.

---

## v0.1.0 — First playable build

The first playable version of MultiScope ATC, controlling **EHAM (Amsterdam Schiphol)**.

- Full EHAM navigation database — runways, waypoints, airspace, SIDs, STARs and procedures.
- Realistic aircraft physics (heading, altitude, speed, wind).
- ATC command system — vectors, altitude, speed, ILS, holds, direct-to and hand-offs.
- ILS approaches with localizer capture and glideslope, and holding patterns with all three ICAO entry types.
- Conflict detection (STCA) with ICAO separation minima and predictive advisories.
- 49 aircraft types, each with its own performance.
- Offline Windows voice control with realistic pilot read-backs and a full radio audio chain (squelch clicks, static hiss).
- Plan-view radar scope with data tags, speed vectors, ILS lines, airspace boundaries and conflict symbology.
