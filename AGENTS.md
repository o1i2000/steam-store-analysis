# Agent Instructions — Steam Store Analysis

## Project Objective
Analyze Steam store game data to advise on game category, price, and release date for optimal post-launch sales. Key business questions:
- Trends between game price and game category
- Which categories sell best (and whether external factors dominate)
- Gaps in the market
- Effect of release date on post-launch and long-term sales

## Data Sources
| File | Contents |
|------|----------|
| `games.csv` | Main game metadata (title, release date, price, type) |
| `genres.csv` | Genre assignments per game |
| `tags.csv` | Community tags per game |
| `reviews.csv` | Steam ratings and review counts |
| `categories.csv` | Steam categories (Single-player, Controller support, etc.) |
| `descriptions.csv` | Full and summary text descriptions |
| `steamspy_insights.csv` | Estimated sales, playtime, etc. (if present) |

## Data Analysis Workflow

### 1. Understand the Data First
- Always check shape, dtypes, missing values, and summary statistics before any transformation
- Visualize distributions of key numeric variables (price, review counts, etc.)
- Check for parsing issues (notice `price_overview` is a malformed JSON-like string in `games.csv`)
- Run `df.info()` and `df.describe()` on every new dataframe loaded

### 2. Clean Systematically
- Handle missing data explicitly — document why rows are dropped or filled
- Parse and normalize columns with embedded structures (e.g., the price JSON fragment that got split across columns in `games.csv`)
- Standardise date columns to `datetime` — note the `release_date` column uses `DD/MM/YYYY` format
- Remove or flag duplicate app_ids
- Fix mixed-type column warnings on import (use `dtype` or `low_memory=False`)
- Drop unnamed/nan columns after verifying they carry no information

### 3. Explore & Validate
- Build univariate distributions first, then bivariate/multivariate
- Use visualisations (seaborn/matplotlib/plotly) to complement summary stats — never rely on numbers alone
- Check for outliers and decide whether to cap, transform, or segment
- Validate assumptions: are price distributions normal? Are review counts correlated with sales?
- When merging datasets (games + genres + reviews + tags), always verify join keys match and row counts make sense

### 4. Answer Business Questions with Evidence
- Frame every analysis around the business questions
- Use statistical summaries and visualisations; avoid claiming causation without appropriate methods
- Segment by meaningful groups (free vs paid, genre clusters, price tiers, release windows)
- Document effect sizes, not just p-values
- Always note data limitations (e.g., SteamSpy estimates vs actual sales, selection bias of Steam catalogue)

### 5. Code Standards
- Use `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly` — these are the project's established libraries
- Prefer `%matplotlib inline` in notebooks; use `plt.tight_layout()` to avoid clipping
- Use `path.join()` for file paths, never hardcode separators
- Write functions for repeated logic (parsing price, cleaning dates, etc.)
- Label all plot axes and add titles — every chart should be self-explanatory

### 6. Reproducibility
- Never overwrite raw CSVs — keep original data read-only and save cleaned versions with a suffix (e.g., `games_cleaned.csv`)
- Set random seeds when sampling
- Order notebook cells logically: imports → load → clean → explore → model → conclusions
- If a cell depends on earlier cells, keep them in execution order

### 7. Verification Before Declaring Done
- Rerun the entire notebook from top to bottom
- Check that no cell raises an unhandled exception
- Verify merged data has expected row counts
- Confirm that conclusions directly reference the analysis shown
