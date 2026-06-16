# Dutch CO₂ Emissions – Sustainability ML Project

## Project Title

**Dutch Road‑Traffic CO₂ Emissions: A Reproducible Sustainability Regression Project**

## Project Goal

This project uses machine learning and official Dutch statistics to explore how annual CO₂ emissions from road traffic evolve over time and how they are influenced by vehicle fleet characteristics. The current project milestone focuses on building a reproducible regression workflow from raw data loading to initial model evaluation.

The main objectives are to:

- Predict annual CO₂ emissions from Dutch road traffic using a transparent regression pipeline.
- Test whether average fleet CO₂ emission intensity and time are informative predictors of emissions.
- Document the full workflow clearly so the analysis can be reproduced, reviewed, and extended in later milestones.

## Problem Statement

### Core question

> How do changes in road traffic activity and vehicle fleet characteristics influence annual CO₂ emissions from road traffic in the Netherlands?

For the current version of the project, the practical modelling question became:

> Can annual Dutch road‑traffic CO₂ emissions be predicted using year and average fleet CO₂ emission intensity from the CBS road‑traffic emissions dataset?

### Target variable (Y)

- `co2_emissions_mln_kg`: annual CO₂ emissions from road traffic, derived from the CBS variable `CarbonDioxideCO2_1`.

### Feature variables (X)

The current implemented model uses:

- `co2_fleet_gram_per_km`: average fleet CO₂ emission factor, derived from `CarbonDioxideCO2_16`.
- `year`: numeric year extracted from the `Periods` column.

#### Planned future feature extensions include

- Vehicle activity measures such as vehicle‑kilometres travelled.
- Decoded transport‑category information from `MeansOfTransport`.
- Additional vehicle mix indicators derived from category‑level emissions.

### Contextual constraints

- **Geographic scope:** Netherlands, road traffic on Dutch territory.  
- **Time frame:** 1990–2024 in the source table, although fleet emission factor values are only available for a later subset of years.  
- **Sector focus:** Road‑traffic emissions as part of a sustainability and climate‑focused transport analysis.

## Hypothesis

The working hypothesis for this project is:

- **H1:** Higher road‑traffic activity is associated with higher annual CO₂ emissions.
- **H2:** Lower average fleet CO₂ emission factors are associated with lower annual CO₂ emissions.
- **H3:** Changes in vehicle composition and transport activity help explain national road‑traffic emissions better than a minimal model based only on year and average fleet intensity.

At this milestone, the implemented model only tests a simplified part of this hypothesis because the final regression uses `year` and `co2_fleet_gram_per_km` as predictors.

## Data Source

The project currently uses one main official dataset:

- **CBS StatLine – Emissions to air on Dutch territory; road traffic**  
  - Contains annual road‑traffic emissions by pollutant and transport category.  
  - Also includes fleet emission factors in grams per kilometre for a second block of variables.  
  - Source file used in the project: `road_traffic_emissions_cbs.csv`.

### Variables used in the notebook

- `Periods` → converted to `year`  
- `CarbonDioxideCO2_1` → renamed to `co2_emissions_mln_kg`  
- `CarbonDioxideCO2_16` → renamed to `co2_fleet_gram_per_km`  
- `MeansOfTransport` → retained for inspection, but not yet properly decoded or used in the final model  

## What Was Done

### Data loading and inspection

The raw CBS file was loaded from the `data-raw/` folder. During the workflow, it was discovered that the CSV used a semicolon separator rather than a comma separator. Once the file was read with `sep=";"`, the dataset loaded correctly with 245 rows and 33 columns.

### Data preparation

The following preparation steps were completed:

- Checked for exact duplicate rows and found none.
- Converted the `Periods` field into a numeric `year` variable.
- Selected the key modelling columns and renamed them to clearer names.
- Identified that `MeansOfTransport` contains coded IDs rather than human‑readable labels.
- Converted `co2_emissions_mln_kg` and `co2_fleet_gram_per_km` to numeric format.
- Replaced placeholder values such as `.` with missing values through numeric coercion.
- Dropped rows with missing values in the target or selected features.

After numeric cleaning, the final modelling dataframe contained **147 rows and 4 columns**.

## Train–test split

The dataset was split using a random train–test split with `test_size=0.2` and `random_state=42`.

- Training rows: **117**  
- Test rows: **30**

This was a standard random split, not a temporal, stratified, or grouped split.

## Model training

A baseline **Linear Regression** model from scikit‑learn was trained using:

- **Features:** `co2_fleet_gram_per_km`, `year`  
- **Target:** `co2_emissions_mln_kg`  

The Python environment initially did not include scikit‑learn, so the package was installed during the project setup before the model was run successfully.

## Model Results

The linear regression model returned the following coefficients:

- Intercept: **256885.83**  
- Coefficient for `co2_fleet_gram_per_km`: **−5.78**  
- Coefficient for `year`: **−122.74**

### Performance metrics

- Train RMSE: **9524.59**  
- Test RMSE: **10256.99**  
- Train R²: **0.039**  
- Test R²: **0.002**

These results show that the current model has very weak predictive power. The R² values are close to zero, which means the model explains almost none of the variation in annual road‑traffic CO₂ emissions.

## Interpretation

The negative coefficient for `year` is plausible because emissions may decline over time as regulations, technology, and efficiency improve. However, the negative coefficient for `co2_fleet_gram_per_km` is counterintuitive because higher grams of CO₂ per kilometre would normally be expected to increase total emissions.

This suggests that the current model is under‑specified and does not capture the full emissions system well. Total road‑traffic emissions are likely influenced by additional factors such as traffic volume, mobility demand, and the composition of different vehicle classes, which were not yet properly incorporated into the model.

## Key Findings

- The technical pipeline is functional and reproducible from raw file loading to model evaluation.
- Real‑world public datasets require careful inspection before modelling.
- Small data issues had a major effect on the workflow, including file naming problems, a separator mismatch, coded category labels, and non‑numeric placeholders.
- The current predictor set is too limited to explain annual CO₂ emissions well.
- Weak model performance can still be informative because it shows what information is missing from the current problem setup.

## Challenges Encountered

Several practical challenges were encountered during the project:

- The original file path failed because the raw file had an incorrect `.csv.csv` extension.
- The dataset first loaded as a single column because the wrong delimiter was used.
- `MeansOfTransport` contained coded identifiers instead of expected descriptive labels.
- `co2_fleet_gram_per_km` contained non‑numeric placeholder values such as `.`.
- The Python environment initially lacked scikit‑learn.

Each of these issues was resolved through step‑by‑step debugging, which became an important part of the project journey.

## Project Structure

The current repository structure is organized as follows:

```text
dutchco2-emissions-sustainability-ml-project/
├── config/
├── data-processed/
├── data-raw/
├── notebooks/
│   └── Esmeralda_regression_capstone.ipynb
├── outputs-figures/
├── outputs-reports/
├── src/
├── dutchco2-emissions-sustainability-ml-project.docx
└── README.md
```

This structure separates raw data, processed data, notebooks, outputs, and supporting files in a way that supports reproducibility.

## Reproducibility

Reproducibility is a core goal of this project. The workflow is designed so that another user can:

1. Open the notebook.  
2. Load the raw CBS file from the `data-raw/` folder.  
3. Re‑run the cleaning steps.  
4. Recreate the modelling dataframe.  
5. Reproduce the train–test split and baseline regression results.  

The notebook documents not only the final model, but also the troubleshooting steps that were required to make the pipeline work correctly.

## How to Run the Project

Clone the repository:

```bash
git clone https://github.com/Esmeralda-castellanos/dutchco2-emissions-sustainability-ml-project.git
cd dutchco2-emissions-sustainability-ml-project
```

Create and activate a virtual environment:

```bash
python -m venv .venv
```

On Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Place the CBS raw CSV file in the `data-raw/` folder.

Open and run:

```text
notebooks/Esmeralda_regression_capstone.ipynb
```

Review cleaned outputs in `data-processed/` and any generated reports or figures in the output folders.

## Tools and Technologies

- **Language:** Python  
- **Environment:** VS Code with Jupyter notebook support  

Libraries used so far:

- `pandas`  
- `numpy`  
- `scikit-learn`  
- `os` (standard library)  

Future extensions may add visualization libraries such as `matplotlib` or `seaborn`.

## Ethical and Sustainability Considerations

- The project uses official national statistics rather than personal data.
- The modelling goal is linked to a sustainability question with public relevance.
- Results are interpreted cautiously because a weak model should not be over‑claimed.
- The project emphasizes transparency, reproducibility, and honest reporting of limitations.

## Current Limitations

- The model currently uses only two predictors.
- The `MeansOfTransport` codes have not yet been decoded into interpretable transport categories.
- No traffic activity dataset has yet been integrated.
- The current split is random rather than temporal, which is not ideal for a time‑related prediction problem.
- The baseline model is useful as a first benchmark, but not yet strong enough for meaningful predictive insight.

## Next Steps

Planned improvements for the next phase of the project include:

- Add traffic activity variables such as vehicle‑kilometres travelled.
- Decode and use `MeansOfTransport` more effectively.
- Engineer vehicle mix features.
- Compare baseline linear regression with stronger regression models.
- Explore time‑aware validation strategies such as temporal splitting.
- Add visual analysis and diagnostic plots.

## Contact

Project lead: **Esmeralda C.**

GitHub repository:  
[Esmeralda-castellanos/dutchco2-emissions-sustainability-ml-project](https://github.com/Esmeralda-castellanos/dutchco2-emissions-sustainability-ml-project)
