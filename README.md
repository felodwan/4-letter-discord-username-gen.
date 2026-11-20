Statuses: **AVAILABLE** (confirmed by you or a documented API) / **TAKEN** / **UNKNOWN** (not verified,
or a check failed) / **INVALID** (breaks the format rules).

## Discord format rules enforced

2-32 characters in general (this app: exactly 4), only lowercase `a-z`, `0-9`, `_` and `.`, and no two
consecutive periods.

## Features

* **Generator**: letters / numbers+letters / letters+underscore / letters+period / custom characters;
  10, 50, 100, 500 or 1000 candidates; starting characters; excluded characters; duplicate prevention
  (also skips names already in Results); "avoid obvious patterns" (aaaa, abab, abcd...); OS-level
  randomness; save the generated list to TXT.
* **Custom pattern**: choose allowed characters per position, e.g. `a-z`, `0-9`, `_.`, `q`
  (ranges and literals). Presets include `q9vx`-style, `sl_z`-style and `q.z9`-style patterns.
* **Results**: four categories with counts, search, per-row Copy, copy selected, copy all available,
  export available to TXT, export all to CSV, delete selected, clear.
* **Settings** (saved automatically): dark/light theme, default count, duplicate prevention,
  export location, safe request settings (delay >= 1 s, timeout, max requests per run), version.

Data is stored in `%APPDATA%\4L Hunter` (`settings.json`, `results.db`). Exports go to the export
location (default `Documents\4L Hunter Exports`) with timestamped file names.

## Build the .exe (Windows 10/11)

1. Install Python 3.11+ from python.org (tick **Add python.exe to PATH**).
2. Double-click **`build.bat`** (or run it from a terminal in this folder).
   It checks Python, creates `.venv`, installs `requirements.txt` and PyInstaller, then runs:
   `PyInstaller --onefile --windowed --name "4L Hunter" --icon assets\icon.ico ...`
3. Your standalone program is **`dist\4L Hunter.exe`** - no Python needed to run it.

## Run from source

```
py -3 -m venv .venv
.venv\Scripts\pip install -r requirements.txt
.venv\Scripts\python main.py
```

## Tests

```
.venv\Scripts\python -m unittest discover -s tests -v
```
(GUI tests run headless; on Linux/macOS set `QT_QPA_PLATFORM=offscreen`.)
`tests/test_core.py` covers generator modes, patterns, duplicates, checker error handling (against a
local mock server, not Discord), database, exports and settings. `tests/test_ui.py` drives the real
GUI pages.

## Project layout

```
main.py        entry point            generator.py  generation + validation
checker.py     availability checker   database.py   SQLite results store
settings.py    persisted settings     exporter.py   TXT/CSV export
theme.py       dark/light styles      ui_main.py / ui_pages.py   GUI
build.bat      Windows build          assets/icon.ico, tools/make_icon.py
```

## Troubleshooting

* *"Python 3.11 or newer was not found"* - install it and re-run; open a new terminal after installing.
* *Antivirus flags the exe* - common with unsigned PyInstaller one-file builds; build locally and
  add an exclusion for your own file if needed.
* *First start is slow* - one-file executables unpack to a temp folder on launch.

