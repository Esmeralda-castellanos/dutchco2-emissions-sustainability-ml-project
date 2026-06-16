# Dutch CO₂ Emissions – Sustainability ML Project

## Project Title
Dutch Road-Traffic CO₂ Emissions: A Reproducible Sustainability Regression Project

## Project Goal

This project uses machine learning and official Dutch statistics to model how
annual CO₂ emissions from road traffic evolve over time and how they are
influenced by traffic activity and vehicle fleet characteristics.

The main objective is to build a **reproducible regression pipeline** that:

- Predicts annual CO₂ emissions from road traffic in the Netherlands.
- Quantifies how changes in traffic activity and fleet efficiency relate to
  changes in emissions.
- Provides transparent, policy-relevant insights for transport decarbonisation.

## Problem Statement

**Core question**

> How do changes in road traffic activity and vehicle fleet characteristics
> influence annual CO₂ emissions from road traffic in the Netherlands?

**Target variable (Y)**

- Annual CO₂ emissions from road traffic on Dutch territory  
  (e.g. million kg CO₂ per year, total motor vehicles).

**Feature variables (X)**

Initial feature set (subject to refinement as the project evolves):

- Fleet emission factor for CO₂ (grams/km) for the road-traffic vehicle fleet.
- Indicators of the vehicle mix, such as the share of emissions from
  passenger cars vs heavy vehicles.
- (Planned extension) Traffic activity metrics like vehicle-kilometres
  travelled per year from transport statistics.

**Contextual constraints**

- Geographic scope: Netherlands, road traffic on Dutch territory.
- Time frame: approximately 1990–2024 (depending on data availability per
  variable).
- Sector focus: Road traffic emissions as part of the national greenhouse
  gas inventory.

## Hypothesis

The working hypothesis for this project is:

- **H1:** Increases in road-traffic activity (e.g. vehicle-kilometres
  travelled) are associated with higher CO₂ emissions, all else equal.
- **H2:** Reductions in fleet emission factors (grams CO₂ per km), driven by
  cleaner vehicles and technology improvements, are associated with lower
  CO₂ emissions, partially offsetting traffic growth.
- **H3:** Changes in the vehicle mix (e.g. a higher share of heavy-duty
  vehicles) have a measurable effect on national road-traffic CO₂ emissions.

The regression model is used to test these relationships and to explore
scenarios such as “What if traffic grows but the fleet becomes more efficient?”

## Data Sources

The project relies on official Dutch and open data sources to ensure
credibility and policy relevance. Key datasets include:

1. **Target dataset – CO₂ emissions**

   - CBS StatLine: *Emissions to air on Dutch territory; road traffic*  
     (table for air pollution emissions and fleet emission factors by
     vehicle category, yearly).
   - Used to construct the target variable:
     - Annual CO₂ emissions from road traffic (total motor vehicles).

2. **Feature datasets – fleet and activity (planned)**

   - From the same road-traffic emissions table:
     - Fleet emission factor CO₂ (grams/km) by year and vehicle category.
     - CO₂ emissions by vehicle category (to derive vehicle mix indicators).
   - From additional CBS transport tables (to be added):
     - Vehicle-kilometres travelled per year by vehicle type.
     - Additional indicators of the road-traffic activity level.

Each dataset used in the project will be documented in the
`data/README_data.md` file (to be created) with:

- Source and URL.
- Time coverage.
- Key variables and units.
- Any preprocessing steps applied.

> **Note:** Raw data files are stored in `data/raw/` and are not manually
> edited. Processed, cleaned versions used for modeling are stored in
> `data/processed/`.

## Project Structure

This repository follows a lightweight, reproducible structure:

```text
dutchco2-emissions-sustainability-ml-project/
├── README.md                  # Project overview (this file)
├── data/
│   ├── raw/                   # Untouched, original data files
│   └── processed/             # Cleaned or transformed datasets
├── notebooks/
│   ├── 01-exploration.ipynb   # Exploratory data analysis (EDA)
│   └── 02-modeling.ipynb      # Regression model building & evaluation
├── src/
│   ├── __init__.py
│   └── utils.py               # Helper functions (loading, preprocessing, plotting)
├── outputs/
│   ├── figures/               # Plots and visuals
│   └── reports/               # Final summaries, markdown or HTML reports
├── config/
│   └── config.yml             # Project configuration (paths, target, features, params)
├── requirements.txt           # Project dependencies
└── .gitignore                 # Files/folders to exclude from version control
```

### Config file

A minimal `config/config.yml` might look like:

```yaml
target: co2_emissions_road_total
features:
  - fleet_emission_factor_co2
  - share_co2_passenger_cars
  - share_co2_heavy_vehicles
time_range:
  start_year: 1990
  end_year: 2024
model:
  type: linear_regression
  test_size: 0.2
  random_state: 42
```

This keeps hyperparameters and variable names out of the code and makes it
easy to rerun or extend experiments.

## Reproducibility

Reproducibility is a core design goal of this project. Concretely:

- **Version control:** The repository is tracked with Git. Each major change
  (data cleaning, feature engineering, model tuning) is committed with a
  clear message.
- **Environment tracking:** All Python dependencies are listed in
  `requirements.txt`. Anyone can recreate the same environment using:

  ```bash
  pip install -r requirements.txt
  ```

- **Scriptable workflow:** Data loading, cleaning, feature engineering, and
  model training are implemented in notebooks and reusable functions in
  `src/`, with minimal manual clicking.
- **Clear documentation:** This README and the data documentation explain
  what the project does, where the data comes from, and how to reproduce
  the results step-by-step.

## How to Run the Project

1. **Clone the repository**

   ```bash
   git clone https://github.com/<your-username>/dutchco2-emissions-sustainability-ml-project.git
   cd dutchco2-emissions-sustainability-ml-project
   ```

2. **Create and activate a virtual environment (recommended)**

   Using `venv`:

   ```bash
   python -m venv .venv
   source .venv/bin/activate   # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Add data**

   - Place downloaded CBS StatLine files in `data/raw/`.
   - Run the EDA notebook to generate processed versions in
     `data/processed/`.

5. **Run the notebooks**

   - `notebooks/01-exploration.ipynb`  
     Understand the data, check distributions, and explore trends.
   - `notebooks/02-modeling.ipynb`  
     Build and evaluate the regression model(s), including residual analysis
     and scenario exploration.

6. **View outputs**

   - Figures are saved to `outputs/figures/`.
   - Model summaries and key findings can be saved as markdown or HTML
     reports in `outputs/reports/`.

## Tools and Technologies

- **Language:** Python
- **Environment:** VS Code (recommended editor)
- **Core libraries (planned):**
  - `pandas` for data manipulation
  - `numpy` for numerical operations
  - `matplotlib` / `seaborn` for visualization
  - `scikit-learn` for regression modeling and evaluation

All required packages and versions will be listed in `requirements.txt`.

## Ethical and Sustainability Considerations

- The project uses **official national statistics**, which supports
  transparency and public accountability.
- All results and code are kept reproducible, so others can verify the
  analysis and adapt it to related sustainability questions.
- Interpretations and policy-relevant scenarios are presented cautiously,
  with clear communication of assumptions and limitations.

## Contact

Project lead: *Esmeralda C.*    

If you have feedback or suggestions, feel free to open an issue or submit a
pull request.
