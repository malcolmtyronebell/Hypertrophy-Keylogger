# 12-Week Hypertrophy Logger
 
A single-file, mobile-first workout logger for a 12-week BBB-inspired hypertrophy program (PPL×2 split, 6 days/week). Built for iPhone. No account, no cloud sync, no dependencies — open the HTML, log your sets, done.
 
Derived from a structured Excel program (12 weekly tabs × 6 training days × 7 exercises per day = 504 logged sessions' worth of structure) and flattened into a single 67 KB HTML file that runs anywhere a browser does.
 
## Why this exists
 
The program lived in a spreadsheet. Spreadsheets are painful to log into on a phone between sets — cell selection is finicky, keyboards cover the cell you're editing, and scrolling a grid one-handed in a gym is a bad time. Commercial logging apps (Strong, Hevy, FitNotes) are excellent but import their own opinions about programs, require accounts, and lock your data inside their ecosystem. This is a tradeoff: less polished than a native app, but the program stays yours, the data stays on your device, and the whole thing is one file you can read, audit, or fork.
 
## Features
 
- **Complete program embedded.** All 12 weeks, 6 days per week, 7 exercises per day, with prescribed sets, rep targets, RPE, and rest periods lifted directly from the source spreadsheet.
- **Mobile-first UI.** Designed for iPhone 12 Pro Max (tested) and scales down to smaller iPhones. Respects safe-area insets, supports Add to Home Screen for fullscreen launch.
- **Per-set logging.** Weight and reps per set, with auto-save on every keystroke.
- **Per-exercise notes.** One notes field per exercise — log form cues, pain points, failure points.
- **Progress bar.** Fills as you complete sets for the current day. Exercises turn green with a ✓ when all sets are logged.
- **Phase metadata visible.** Current week's rep target, rest, RPE, and volume index are always shown at the top.
- **Local-only storage.** Uses `localStorage` — no servers, no accounts, no telemetry. Data lives in the browser on the device you logged it on.
- **JSON export/import.** One-tap export of your entire log as JSON for manual backup or device migration.
- **Offline after first load.** Once cached by Safari, works without internet.
## Stack
 
Intentionally minimal:
 
- Vanilla HTML, CSS, JavaScript — no framework, no build step, no npm
- `localStorage` for persistence
- No external scripts, fonts, or CDN assets (renders identically offline)
- ~67 KB total, including embedded program data
## Program structure
 
The embedded program follows a BBB Level 1 ramp / Super-Growth framework compressed to 12 weeks:
 
| Weeks  | Phase                         | Rep Target | Intensity   | Volume Index |
|--------|-------------------------------|------------|-------------|--------------|
| 1–3    | Ramp 1 — Hyperacceleration    | 12–15      | RPE 7–9     | 70 → 100%    |
| 4–6    | Super-Growth Phase 1          | 6–10       | RPE 8–9     | 60–65%       |
| 7–9    | Ramp 2 — Hyperacceleration    | 6–10       | RPE 8–9     | 80 → 110%    |
| 10–12  | Super-Growth Phase 2 / PR     | 3–7        | RPE 8–10    | 45–55%       |
 
**Weekly split:** Push A / Pull A / Legs A (quad) / Push B / Pull B / Legs B (ham-glute) / Rest.
 
## Install
 
### Recommended: host on a static-hosting service, add to Home Screen
 
iOS Safari won't run JavaScript in HTML files opened directly from the Files app (sandboxed preview). The cleanest workaround is to host the file at a real URL.
 
1. Go to https://app.netlify.com/drop
2. Drag `hypertrophy_logger.html` onto the drop zone
3. Copy the URL Netlify generates
4. (Optional) Claim the site to make the URL permanent and rename it to something memorable
5. On your iPhone, open the URL in Safari
6. Tap Share → Add to Home Screen
After that, tapping the home-screen icon launches the logger fullscreen. Works offline once cached.
 
GitHub Pages works equally well if you'd rather serve it from this repo.
 
### Alternative: desktop browser
 
Open `hypertrophy_logger.html` directly in any modern desktop browser. Everything works identically; data stays in that browser's localStorage.
 
## Usage
 
- **Week / Day pickers** at the top switch context. The app remembers which week and day you were last on between sessions.
- **Per-set inputs** accept decimals (e.g. `47.5` for DB weight).
- **Clear Day** erases logged sets for the current day only — other days are untouched.
- **Export** downloads your entire log as JSON. Save it to iCloud Drive or email it to yourself to back up.
- **Import** pastes JSON back in to restore a previous export (replaces all current data).
## Data model
 
Log data is a single object keyed by `week + day`:
 
```json
{
  "w1d0": {
    "0": {
      "sets": [
        { "w": "185", "r": "12" },
        { "w": "185", "r": "11" }
      ],
      "notes": "Form broke down on set 4, drop next week"
    }
  }
}
```
 
Exercises are referenced by index into the day's exercise array, not by name — so renaming an exercise in the program data would orphan historical logs for that slot. Not a concern for fixed 12-week programs; worth knowing if you fork this.
 
## Limitations (by design)
 
- **No cross-device sync.** Data is `localStorage`-scoped. Log on iPhone → not visible on iPad. Use Export/Import to move data manually. Native iCloud sync requires a signed iOS app and Apple Developer account ($99/yr), which defeats the single-file philosophy.
- **Clearing Safari website data wipes the log.** Export periodically if you care.
- **No rest timer, plate calculator, or history view.** Intentional. This is a logger, not a coach. Use a separate timer app (iOS has a good one built in).
- **Program is hardcoded.** Changing exercises, sets, or rep targets requires editing the HTML directly.
## Customization
 
All program data lives in a single JSON object at the top of the `<script>` block in `hypertrophy_logger.html`:
 
```javascript
const PROGRAM = { "1": { "title": "...", "meta": "...", "days": [...] }, ... };
```
 
To adapt for a different program:
 
1. Replace the `PROGRAM` object with your own data in the same shape
2. Update week count in `initSelectors()` (currently loops `w=1; w<=12`)
3. That's it — everything else is program-agnostic
## Source
 
Program framework adapted from *Big Beyond Belief* (Leo Costa Jr. / Dr. Russ Horine), Level 1 Ramp / Super-Growth structure, compressed to 12 weeks and modernized with a PPL×2 split and contemporary compound-lift emphasis.
 
## License
 
Personal use. Do whatever you want with the code; don't expect support.# Hypertrophy-Keylogger
