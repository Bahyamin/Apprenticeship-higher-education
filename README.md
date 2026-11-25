# The rise of apprenticeship in French higher education (2020-2026)

Final project of the course **Programming for Data Science** (ESILV, Hugo Alatrista-Salas).

**Team:** Maxence COLLIN (team head), Clément PHUNG, Benjamin ZILBER

We follow the KDD process (selection, pre-processing, transformation, data mining, interpretation) on a real multidimensional dataset to answer one question: how did apprenticeship spread across the programmes that French students choose after the baccalauréat, where is it concentrated, and what comes next?

## Datasets

| Dataset | Source | Licence | Rows | Dimensions |
|---|---|---|---|---|
| Cartographie des formations Parcoursup 2020-2026 (main) | [data.enseignementsup-recherche.gouv.fr](https://data.enseignementsup-recherche.gouv.fr/explore/dataset/fr-esr-cartographie_formations_parcoursup/) | Licence Ouverte 2.0 | 157,497 | time: `annee`; space: `etab_gps`, `region`, `commune`; analysis: type, apprenticeship, public/private, remote teaching... |
| InserJeunes, apprenticeship by training centre (second dataset) | [data.education.gouv.fr](https://data.education.gouv.fr/explore/dataset/fr-en-inserjeunes-cfa/) | Licence Ouverte 2.0 | 11,314 | interrupted contracts, employment and further-study rates per centre |

Both files were downloaded on 25 November 2026 and are stored in `data/` (the main file is gzip-compressed to stay under the GitHub size limit; pandas reads it directly). If a file is missing, the notebook downloads it from the official URL.

## The four queries

| # | Question | Technique | Main result |
|---|---|---|---|
| Q1 | Which programmes and regions offer apprenticeship? | grouping (pandas `groupby`) | 11,251 apprenticeship programmes in 2026, 44% of the offer; BTS = 81% of them |
| Q2 | What goes together with apprenticeship? | Apriori, association rules, Sankey diagram (mlxtend) | {BTS, private without State contract} → apprenticeship: confidence 0.99, lift 2.27 |
| Q3 | How fast does it grow, what comes next? | Themeriver, static state replacement, Prophet forecast with back-test | x2.7 from 2020 to 2025, -1.8% in 2026, plateau forecast |
| Q4 | Where are the hubs? | OPTICS with haversine distance, K-means with silhouette (scikit-learn) | 49 national hubs, Paris 4 times Lyon; 15 dense pockets in the Paris region |
| Extra | What happens to apprentices? | join on the UAI school code with InserJeunes | median 32% of contracts interrupted before the end |

## Repository content

```
Collin_notebook.ipynb    the full analysis (all functions + main), already executed
Collin_dashboard.html    static export of the Dash dashboard (opens offline in any browser)
Collin_slides.pdf        5-minute presentation
requirements.txt         Python libraries
data/                    the two datasets
```

## How to run

```bash
pip install -r requirements.txt
jupyter notebook Collin_notebook.ipynb
```

Run all cells. The `main()` function at the end runs the whole analysis and starts the Dash dashboard at http://127.0.0.1:8050. Set `RUN_DASHBOARD = False` in the settings cell to skip it. The full run takes about 30 seconds on a laptop.

If Prophet cannot be installed on your computer, the notebook still runs: it falls back to a linear trend and says so at the top.
