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

- **Voice control** — Issue clearances by speaking: headings, altitudes, speeds, ILS, holds, and hand-offs (offline, no internet required)
- **Realistic aircraft physics** — Heading, altitude, speed, and wind simulation at 100 Hz
- **ILS approaches** — Full localizer and glideslope guidance with distance-based speed profiles
- **Holding patterns** — All three ICAO entry types with wind-compensated timing
- **STAR/SID route following** — Fly-by turn anticipation with speed and altitude restrictions
- **Conflict detection** — Real-time STCA with advisory, warning, and loss-of-separation alerts
- **49 aircraft types** — From CRJ7 to A380, each with a unique performance envelope
- **Live traffic** — Dynamic spawn from real procedures with wake turbulence separation

---

## Status

Early development — pre-release. Currently supports EHAM only.

---

## License

MultiScope ATC is **all rights reserved**. The source code, assets, and
documentation are the property of Dalí Klaassen and Pim van Gool and may not be
reproduced, distributed, or modified without prior written permission. See
[LICENSE](LICENSE).
