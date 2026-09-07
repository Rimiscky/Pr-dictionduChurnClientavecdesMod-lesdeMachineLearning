# Prédiction du Churn Client

Projet #3 — Introduction au Machine Learning (L'École Multimédia).

## Contexte

On se met dans la peau d'un data scientist d'une entreprise de télécommunications.
Objectif : prédire si un client va résilier son abonnement (le **churn**), à partir
de ses données de compte, de ses services souscrits et de son profil démographique.

C'est un problème de **classification binaire** : la cible `Churn` vaut soit `Yes`,
soit `No`. On compare plusieurs modèles (régression logistique, arbre de décision,
Random Forest) pour trouver celui qui prédit le mieux.

## Structure du projet

```
.
├── data/
│   └── raw/                          # Le CSV brut (non versionné, voir plus bas)
├── notebooks/
│   ├── 01_exploration_preparation.ipynb   # Analyse + nettoyage des données
│   └── 02_modelisation_evaluation.ipynb   # Entraînement + comparaison des modèles
├── requirements.txt
└── README.md
```

Pas plus compliqué que ça : deux notebooks, qui correspondent exactement aux deux
documents demandés dans le rendu final du brief (préparation des données d'un côté,
modélisation + validation de l'autre).

## Installation

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Récupération des données

Le dataset est "Telco Customer Churn" (Kaggle). Il n'est **pas** versionné sur Git
(règle de base : on ne commit jamais des données, seulement le code qui les traite).

1. Télécharger `WA_Fn-UseC_-Telco-Customer-Churn.csv` depuis Kaggle
   (dataset `blastchar/telco-customer-churn`).
2. Le placer dans `data/raw/`.

## Étapes du projet (checklist)

- [ ] Collecte et préparation des données (qualité, valeurs manquantes, encodage,
      normalisation)
- [ ] Analyse exploratoire (EDA) et sélection des caractéristiques
- [ ] Régression logistique (modèle de référence) + évaluation (matrice de confusion,
      ROC, AUC)
- [ ] Arbre de décision + interprétation des règles
- [ ] Random Forest + importance des variables + optimisation des hyperparamètres
- [ ] Comparaison des modèles (précision, rappel, F1, AUC)
- [ ] Validation sur jeu de test + analyse de la capacité de généralisation
- [ ] Visualisations et documentation finale

## Conventions

Les commits suivent [Conventional Commits](https://www.conventionalcommits.org)
(`feat:`, `fix:`, `docs:`, `chore:`, ...).
