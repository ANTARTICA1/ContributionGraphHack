# GitHub Contribution Graph Pattern Generator

An automated Git commit scripting utility that renders custom pixel art and typographic patterns onto the GitHub contribution activity heatmap by scheduling backdated repository commits across defined coordinate matrices.

## Overview

GitHub calculates user contribution graphs by aggregating commit timestamps matching the primary author email address across the 52-week annual calendar grid (7 rows representing days of the week, Sunday through Saturday). This utility automates the generation of backdated git commits mapped directly from a 2D JSON pixel grid, allowing developers to draw custom banners, logos, and typographic patterns.


## Execution Preview

[![Execution Preview Status](https://img.shields.io/badge/Pattern_Preview-Documentation_in_Progress-2563eb?style=for-the-badge&logo=git&logoColor=white)](https://github.com/ANTARTICA1/ContributionGraphHack)

## Technical Mechanics

### 1. Matrix Coordinate System
The GitHub contribution heatmap is modeled as a 7 × 52 discrete coordinate grid:
- Rows ($r \in [0, 6]$): Days of the week (Sunday = 0, Monday = 1, ..., Saturday = 6).
- Columns ($c \in [0, 51]$): Calendar weeks relative to the first Sunday of the target year.

For a selected year $Y$, the base date $D_0$ is defined as the first Sunday of January. Each coordinate $(r, c)$ corresponds to a target timestamp:

$$\text{Date}(r, c) = D_0 + (c \times 7\text{ days}) + (r \times 1\text{ day})$$

### 2. Git Timestamp Spoofing
Standard Git commits record the current system clock. To assign commits to historical coordinate dates, the script overrides Git environment variables:
- `GIT_AUTHOR_DATE`: Sets the authorship timestamp.
- `GIT_COMMITTER_DATE`: Sets the committer timestamp.

Formatted in ISO 8601 representation (`YYYY-MM-DDTHH:MM:SS`), commits are created via:
```bash
git commit --allow-empty -m "<message>" --date "<ISO-timestamp>"
```

### 3. Contribution Density Control
The contribution graph colors pixels with tiered shades of green based on relative commit frequency. The configuration variable `COMMITS_PER_PIXEL` controls the number of commits generated per active cell, ensuring the desired color saturation level.

## Project Structure

```
ContributionGraphHack/
├── pattern.json     # 2D character matrix defining the graphical pattern
├── script.py        # Core automation script executing dated Git commits
├── info.txt         # State file modified between successive commits
└── README.md        # Technical documentation
```

## Requirements

- Python 3.8 or higher
- Git CLI configured with authenticated credentials to push to the target GitHub repository

## Usage

1. Create a dedicated empty repository on GitHub to hold the pattern commits.
2. Clone your repository locally and place `script.py` and `pattern.json` inside it.
3. Define your target pattern in `pattern.json`:
   - 7 strings representing the 7 days of the week (Sunday to Saturday).
   - Use spaces (`" "`) for blank cells and non-space characters (`"#"`) for active pixels.
4. Execute the generator:
   ```bash
   python script.py
   ```
5. Enter the target calendar year when prompted.
6. The script will automatically generate the commits and push them to your remote repository.

## Notice

This utility is intended for artistic, experimental, and educational demonstrations of Git internals and metadata structures. Generating synthetic contribution histories does not reflect actual code production.

## Author

Anak Agung Ngurah Krisna Artha Wibawa ([ANTARTICA1](https://github.com/ANTARTICA1))

## License

This project is licensed under the MIT License.