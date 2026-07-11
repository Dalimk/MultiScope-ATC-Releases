# Changelog

Player-facing release notes for **MultiScope ATC**. Newest first.

This is the curated, plain-English changelog — it highlights what changes for you as a controller. Minor internal and tooling work between these versions is not listed.

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
