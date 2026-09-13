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

```text
.
├── data/
│   └── raw/                          # Le CSV brut (non versionné)
├── notebooks/
│   ├── 01_exploration_preparation.ipynb
│   └── 02_modelisation_evaluation.ipynb
├── src/
│   └── train.py                      # Pipeline ML reproductible en ligne de commande
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

## Récupération des données

Le dataset utilisé est **Telco Customer Churn** de Kaggle.

1. Télécharger `WA_Fn-UseC_-Telco-Customer-Churn.csv` depuis le dataset `blastchar/telco-customer-churn`.
2. Placer le fichier dans `data/raw/`.

Le CSV brut n'est pas versionné dans Git : le dépôt contient le code permettant de reproduire l'analyse.

## Exécuter la comparaison des modèles

Une fois le dataset placé dans `data/raw/` :

```bash
python src/train.py
```

Le pipeline :

- supprime `customerID`, qui est un identifiant et non une variable explicative ;
- convertit `TotalCharges` en variable numérique et transforme les valeurs invalides en valeurs manquantes ;
- supprime uniquement les doublons strictement identiques ;
- remplace les valeurs numériques manquantes par la médiane ;
- remplace les catégories manquantes par la valeur la plus fréquente ;
- encode les variables catégorielles avec One-Hot Encoding ;
- standardise les variables numériques ;
- réalise un split train/test stratifié 80/20 ;
- compare Régression Logistique, Arbre de Décision et Random Forest.

Pour chaque modèle, le script affiche :

- Accuracy ;
- Precision ;
- Recall ;
- F1-score ;
- ROC-AUC ;
- matrice de confusion.

Le classement final est ordonné par ROC-AUC. Pour le churn, le **Recall de la classe churn** reste particulièrement important : manquer un client réellement susceptible de partir peut coûter plus cher que contacter quelques faux positifs.

## Étapes du projet

- [x] Structure reproductible du projet
- [x] Pipeline de nettoyage de base (`TotalCharges`, doublons, identifiant client)
- [x] Split train/test stratifié
- [x] Préprocessing automatique des variables numériques et catégorielles
- [x] Régression logistique, arbre de décision et Random Forest
- [x] Comparaison Accuracy / Precision / Recall / F1 / ROC-AUC
- [ ] Analyse exploratoire détaillée dans le notebook 01
- [ ] Visualisations du churn et des variables explicatives
- [ ] Courbes ROC et matrice de confusion graphiques dans le notebook 02
- [ ] Optimisation des hyperparamètres avec validation croisée
- [ ] Analyse de l'importance des variables
- [ ] Conclusion métier et recommandation du modèle final

## Principes ML appliqués

Le dataset est déséquilibré : la majorité des clients ne churnent pas. Une Accuracy élevée ne suffit donc pas pour juger un modèle.

Exemple : un modèle qui prédit toujours « pas de churn » pourrait sembler correct en Accuracy tout en étant inutile commercialement. C'est pourquoi le projet compare plusieurs métriques, notamment le Recall, la Precision, le F1-score et le ROC-AUC.

Le préprocessing est inclus dans un `Pipeline` scikit-learn afin qu'il soit appris uniquement sur le jeu d'entraînement. Cela réduit le risque de **data leakage** entre le train et le test.

## Conventions

Les commits suivent Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, ...).
