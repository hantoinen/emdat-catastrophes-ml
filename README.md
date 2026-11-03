# Catastrophes et décès : exploration EM-DAT et modèle de régression

Projet réalisé en groupe de trois dans le cadre de la formation Jedha. Ce dépôt contient les notebooks réalisés par Antoine : une exploration des données (EDA) et un modèle de régression (ML).

## Notebooks

1. `01_eda_emdat.ipynb` : valeurs manquantes par type de catastrophe et par année, distribution du nombre de décès, transformation logarithmique, corrélations.
2. `02_modele_emdat.ipynb` : régression Random Forest pour prédire le nombre de décès (échelle logarithmique).

## Données

Les données ne sont pas incluses dans le dépôt.

- **EM-DAT (CRED)** : fichier `public_emdat_project.csv`, à télécharger depuis le site du CRED. Les conditions de redistribution d'EM-DAT s'appliquent : ne pas republier le fichier.
- **Banque Mondiale** : `population_density.csv` (densité de population) et `gdp_per_capita.csv` (PIB par habitant), au format Banque Mondiale (en-tête de 4 lignes).

Placer les trois fichiers à la racine du dépôt, à côté des notebooks. La première cellule de chaque notebook vérifie leur présence.

## Installation

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

## Résultats

Les chiffres ci-dessous sont issus des cellules indiquées de chaque notebook.

**Exploration (notebook 01)**

- `Total Deaths` : médiane de 16 décès, moyenne de 136, maximum de 222 570 (`describe()`).
- Corrélation entre `CPI` et l'année de début : 0,98 (cellule des corrélations).
- Corrélations linéaires du nombre de décès avec les autres variables numériques : entre −0,006 et −0,004.

**Modèle (notebook 02)**

| Mesure | Valeur |
|---|---|
| R² train | 0,524 |
| R² test | 0,251 |
| MAE test (nombre de morts) | 196 |
| Erreur médiane | 9 morts |
| Erreur au 75e percentile | 20 morts |

Sur les 12 655 catastrophes avec un nombre de décès renseigné, `Magnitude` est absente pour 10 053 lignes (cellule `isnull().sum()`).

## Limites

- Les hyperparamètres du modèle sont fixés à la main, sans recherche sur grille ni validation croisée.
- Un seul découpage aléatoire (80/20) est utilisé ; les résultats peuvent varier d'un découpage à l'autre.
- L'écart entre R² train et R² test indique un surapprentissage.
- La MAE (196) est très supérieure à l'erreur médiane (9) : quelques catastrophes très meurtrières dominent l'erreur moyenne.
- `Magnitude` mélange des unités différentes selon le type de catastrophe, et elle est imputée par la médiane pour la plupart des lignes : son importance doit être lue avec prudence.
- Les données sont observationnelles : le modèle associe des variables, il n'établit pas de lien causal.

## Reproduction

1. Télécharger les fichiers décrits dans la section Données.
2. Les placer à la racine du dépôt.
3. Installer les dépendances.
4. Exécuter `01_eda_emdat.ipynb`, puis `02_modele_emdat.ipynb` (« Restart & Run All »).

## Source

- **EM-DAT** : base de données sur les catastrophes du Centre for Research on the Epidemiology of Disasters (CRED), Université catholique de Louvain.
- **Banque Mondiale** : ces jeux de données ont été fournis dans le cadre de la formation Jedha Bootcamp et proviennent des Indicateurs du développement dans le monde (WDI).
  - `population_density.csv` : densité de population (habitants par km²).
  - `gdp_per_capita.csv` : PIB par habitant (USD).
- Métadonnées et séries complètes : [World Bank Open Data](https://data.worldbank.org/).

## Licence

- **Code et analyse** : MIT.
- **Données EM-DAT** : conditions du CRED, voir la section Données.
- **Données de la Banque Mondiale** : Creative Commons Attribution 4.0 (CC BY 4.0). Crédit à « Banque Mondiale » requis.
