# MultiScope ATC

A realistic Air Traffic Control radar simulation built with **Godot 4** and **C#**.

Control arriving and departing aircraft at **EHAM (Amsterdam Schiphol)** using voice commands and a radar scope — just like a real controller.

> This repository is the public home for **release notes and announcements**. The game's source code is closed and lives in a private repository. See [LICENSE](LICENSE).

---

## Releases

- **[Latest release](https://github.com/Dalimk/MultiScope-ATC-Releases/releases/latest)**
- **[All releases](https://github.com/Dalimk/MultiScope-ATC-Releases/releases)**
- **[Full changelog](CHANGELOG.md)**

Every new version is published here as a GitHub Release, so you can watch this repo (Watch → Custom → Releases) to be notified.

---

## Features

- **Voice control** — Speak headings, altitudes, speeds, ILS, holds, direct-to and hand-offs (offline, Windows, no internet required)
- **Realistic aircraft physics** — Heading, altitude and speed with an altitude-varying wind model
- **Accurate altitudes** — Full QNH / pressure-altitude model; flight levels hold correctly as the altimeter setting changes
- **ILS approaches** — Localizer and glideslope guidance with distance-based speed profiles
- **Landings & runway occupancy** — Aircraft flare, roll out and vacate the runway; spacing guidance shows where the next arrival should sit
- **Crosswind, go-arounds & missed approaches** — Out-of-limits runways and tight spacing trigger realistic go-arounds
- **Holding patterns** — All three ICAO entry types with wind-compensated timing
- **STAR/SID route following** — Fly-by turn anticipation with speed and altitude restrictions
- **Frequency & control handoffs** — Tower and Area Control jurisdictions with realistic transfer of control
- **Conflict detection** — Real-time STCA with advisory, warning and loss-of-separation alerts
- **56 aircraft types** — From CRJ7 to A380, each with a unique performance envelope
- **Schedule-driven traffic** — Real callsigns, types and routes with wake-turbulence separation
- **New-game setup** — Traffic and weather presets with automatic, realistic runway combinations
- **Saves & cloud** — Manual save slots plus autosave, with Steam Cloud sync
- **Extras** — Audio mix sliders, pilot subtitles (accessibility) and Discord Rich Presence

---

## Status

In active development — preparing for a Steam release. Currently supports EHAM (Amsterdam Schiphol) only.

---

## License

MultiScope ATC is **all rights reserved**. The source code, assets, and
documentation are the property of Dalí Klaassen and Pim van Gool and may not be
reproduced, distributed, or modified without prior written permission. See
[LICENSE](LICENSE).
