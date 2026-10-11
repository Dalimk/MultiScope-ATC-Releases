# MultiScope ATC

A radar simulator made by two student air traffic controllers, built with **Godot 4** and **C#**.

Work arrivals and departures at **EHAM (Amsterdam Schiphol)** using your voice and a radar scope, the way a real controller does. Pick the sector you want to work and AI controllers run the rest.

**[Wishlist on Steam](https://store.steampowered.com/app/4832540/MultiScope_Air_Traffic_Control/)** · **[Join the Discord](https://discord.gg/BDvP6ZuY5m)**

> This repository is the public home for **release notes and announcements**. The game's source code is closed and lives in a private repository. See [LICENSE](LICENSE).

---

## Releases

- **[Latest release](https://github.com/Dalimk/MultiScope-ATC-Releases/releases/latest)**
- **[All releases](https://github.com/Dalimk/MultiScope-ATC-Releases/releases)**
- **[Full changelog](CHANGELOG.md)**
- **[Roadmap](ROADMAP.md)**: what's coming next

Every new version is published here as a GitHub Release, so you can watch this repo (Watch → Custom → Releases) to be notified.

---

## Features

### Voice and radio

- **Voice control with our own speech model.** Trained on ATC phraseology, it understands callsigns, flight levels, QNH, ILS clearances and waypoint names, copes with accents and responds in about a fifth of a second. It runs on your graphics card or your processor, entirely on your own PC (Windows, no internet required). Talk naturally, and say "correction" to fix a mistake.
- **ICAO and FAA phraseology**, including rates, crossing restrictions, holds, direct-to and hand-offs.
- **Pilots with accents.** 102 voices across 23 accent groups, matched to each airline's home country, on a frequency that sounds like real VHF. One aircraft talks at a time, and keying over someone gets you a blocked transmission.

### Control

- **Multi-sector with AI controllers.** Take over any approach sector, or hand it back, at any time. The AI approach controller sequences the arrivals, keeps every aircraft separated and shows you what it is working around.
- **COSYS coordination.** Coordinate with neighbouring sectors: early handovers, transfer levels, departure releases, go-around informs and airspace delegation.
- **Conflict detection.** Real-time STCA, wake turbulence separation (ICAO or RECAT) and airspace infringement warnings.

### Simulation

- **Realistic aircraft performance.** 56 aircraft types, each with its own climb, descent and speed figures, and an altitude-varying wind model.
- **Accurate altitudes.** Full QNH / pressure-altitude model; flight levels hold correctly as the altimeter setting changes.
- **ILS approaches, landings and go-arounds.** Aircraft flare, roll out and vacate the runway. Crosswind, tailwind and tight spacing trigger realistic go-arounds and missed approaches.
- **Holding patterns** with all three ICAO entry types and wind-compensated timing.
- **SID/STAR route following** with fly-by turn anticipation and speed and altitude restrictions.
- **Schedule-driven traffic.** Real callsigns, types, registrations and routes, including a Real World mode that plays a real daily schedule, plus general aviation traffic with its own runway logic.

### Getting started and progress

- **Four difficulty presets**, from Beginner to Expert, or fully custom.
- **Phraseology trainer** that drills radio calls by voice and grades them with the same recogniser the game uses.
- **Controller career** with ranks, medals, session grades and lifetime stats.
- **Steam achievements and Steam Cloud saves**, plus manual save slots and autosave.

### Accessibility and extras

- Colourblind display modes, pilot subtitles, audio mix sliders, rebindable controls and Discord Rich Presence.

---

## Status

In active development and preparing for Steam Early Access. Early Access launches with EHAM (Amsterdam Schiphol).

---

## License

MultiScope ATC is **all rights reserved**. The source code, assets, and
documentation are the property of Dalí Klaassen and Pim van Gool and may not be
reproduced, distributed, or modified without prior written permission. See [LICENSE](LICENSE).
