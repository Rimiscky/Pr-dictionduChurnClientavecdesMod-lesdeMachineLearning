# Prédiction du Churn Client

Projet #3 — Introduction au Machine Learning — L'École Multimédia.

## Objectif

L'objectif est de prédire si un client d'une entreprise de télécommunications risque de résilier son abonnement (`Churn = Yes`).

Le projet reste volontairement simple et pédagogique : trois modèles classiques sont comparés afin de comprendre leurs différences et de pouvoir expliquer les résultats facilement à l'oral.

- Régression logistique
- Arbre de décision
- Random Forest

## Données

Dataset : **Telco Customer Churn** de Kaggle.

Le fichier contient 7 043 clients et 21 colonnes. Le taux de churn est d'environ 26,5 %.

Le CSV brut n'est pas versionné dans GitHub. Il doit être placé dans :

```text
data/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv
```

## Structure

```text
.
├── data/
│   └── raw/
├── notebooks/
│   ├── 01_exploration_preparation.ipynb
│   └── 02_modelisation_evaluation.ipynb
├── src/
│   └── train.py
├── RESULTATS.md
├── RAPPORT_FINAL.md
├── GUIDE_ORAL.md
├── requirements.txt
└── README.md
```

## Installation

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Sous Windows PowerShell :

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Exécution

```bash
python src/train.py
```

## Préparation des données

Les étapes principales sont :

1. suppression de `customerID`, car c'est seulement un identifiant ;
2. conversion de `TotalCharges` en nombre ;
3. remplacement des valeurs numériques manquantes par la médiane ;
4. encodage des variables catégorielles avec One-Hot Encoding ;
5. standardisation des variables numériques ;
6. séparation des données en 80 % entraînement / 20 % test ;
7. utilisation d'un split stratifié pour garder la même proportion de churn dans les deux jeux.

## Pourquoi plusieurs métriques ?

Le dataset est déséquilibré : environ trois clients sur quatre ne churnent pas.

L'Accuracy seule n'est donc pas suffisante. On regarde aussi :

- **Precision** : parmi les clients prédits comme churners, combien le sont vraiment ?
- **Recall** : parmi les vrais churners, combien sont détectés ?
- **F1-score** : compromis entre Precision et Recall ;
- **ROC-AUC** : capacité globale du modèle à séparer churners et non-churners.

## Résultats après optimisation simple

| Modèle | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Régression logistique | 0,740 | 0,506 | 0,783 | 0,615 | 0,841 |
| Arbre de décision | 0,755 | 0,527 | 0,759 | 0,622 | 0,832 |
| Random Forest | 0,757 | 0,528 | 0,778 | **0,629** | **0,843** |

La Random Forest obtient le meilleur ROC-AUC, mais la différence avec la régression logistique est faible.

Pour le rendu, la **régression logistique est retenue comme modèle principal** car elle est simple à expliquer, détecte environ 78 % des churners et obtient un ROC-AUC proche du meilleur modèle.

La Random Forest est utilisée comme comparaison pour montrer qu'un modèle plus complexe n'apporte ici qu'un petit gain.

## Hyperparamètres testés

Une petite recherche en grille avec validation croisée a été utilisée.

- Régression logistique : `C = 0.1, 1, 10`
- Arbre : `max_depth = 3, 5, 7, None`
- Random Forest : quelques valeurs simples de profondeur et de taille minimale des feuilles

Meilleurs réglages obtenus :

- Régression logistique : `C = 10`
- Arbre de décision : `max_depth = 5`
- Random Forest : `max_depth = 8`, `min_samples_leaf = 5`

## Variables importantes

Les variables qui ressortent le plus sont notamment :

- l'ancienneté du client (`tenure`) ;
- le type de contrat ;
- l'abonnement mensuel `Month-to-month` ;
- le contrat `Two year` ;
- la fibre optique ;
- `MonthlyCharges` ;
- `TotalCharges`.

Ces résultats montrent des associations apprises par le modèle, pas des relations de cause à effet.

## Conclusion simple

Le projet montre qu'il est possible d'identifier une partie importante des clients susceptibles de résilier leur abonnement.

Le modèle choisi pour la présentation est la régression logistique, car il offre un bon compromis entre performance et simplicité d'interprétation.

Dans un contexte réel, l'entreprise pourrait utiliser ce score de risque pour contacter en priorité les clients les plus susceptibles de partir.

## Documents du rendu

- `notebooks/01_exploration_preparation.ipynb` : qualité des données, statistiques, visualisations et nettoyage ;
- `notebooks/02_modelisation_evaluation.ipynb` : modèles, comparaison, optimisation simple et validation ;
- `RAPPORT_FINAL.md` : synthèse du projet ;
- `GUIDE_ORAL.md` : aide pour présenter le projet simplement ;
- `RESULTATS.md` : résultats chiffrés.

## Conventions Git

Les commits suivent Conventional Commits : `feat:`, `fix:`, `docs:`, `chore:`.
