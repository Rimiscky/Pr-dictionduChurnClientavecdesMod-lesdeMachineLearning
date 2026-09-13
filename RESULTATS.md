# Résultats du projet Churn

Résultats obtenus à partir du dataset Kaggle **Telco Customer Churn** (`WA_Fn-UseC_-Telco-Customer-Churn.csv`).

## Qualité des données

- 7 043 clients
- 21 colonnes
- 0 doublon exact
- 11 valeurs non numériques / manquantes dans `TotalCharges` après conversion
- Taux de churn : 26,54 %

Les valeurs manquantes numériques sont imputées par la médiane dans le pipeline. Les variables catégorielles sont imputées par la modalité la plus fréquente puis encodées avec One-Hot Encoding.

## Protocole

- cible : `Churn` (`Yes` = 1, `No` = 0)
- suppression de `customerID`
- split train/test : 80/20
- split stratifié sur la cible
- `random_state=42`
- standardisation des variables numériques
- pondération des classes (`class_weight="balanced"`)

## Comparaison des modèles

| Modèle | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Régression logistique | 0,738 | 0,504 | 0,783 | 0,614 | **0,841** |
| Random Forest | **0,776** | **0,565** | 0,690 | **0,621** | 0,838 |
| Arbre de décision | 0,740 | 0,507 | **0,794** | 0,619 | 0,834 |

## Matrices de confusion

### Régression logistique

- TN : 747
- FP : 288
- FN : 81
- TP : 293

### Arbre de décision

- TN : 746
- FP : 289
- FN : 77
- TP : 297

### Random Forest

- TN : 836
- FP : 199
- FN : 116
- TP : 258

## Interprétation

Le **Random Forest** obtient la meilleure accuracy et la meilleure précision. Il génère donc moins de faux positifs que les deux autres modèles.

La **régression logistique** obtient toutefois le meilleur ROC-AUC (0,841) et un recall élevé (0,783). Pour un cas métier où manquer un client sur le point de résilier coûte cher, ce compromis est particulièrement intéressant : le modèle détecte environ 78 % des churners du jeu de test.

L'**arbre de décision** obtient le meilleur recall (0,794), mais au prix d'un grand nombre de faux positifs. Il est plus simple à expliquer mais moins performant globalement.

## Variables les plus influentes dans la régression logistique

Parmi les coefficients de plus forte amplitude :

- `tenure` : effet fortement protecteur — les clients plus anciens churnent moins
- contrat `Two year` : effet protecteur important
- contrat `Month-to-month` : effet associé à davantage de churn
- `InternetService = Fiber optic` : associé à davantage de churn dans ce modèle
- `MonthlyCharges` et `TotalCharges` jouent également un rôle notable

Ces coefficients décrivent des associations apprises par le modèle, pas des relations causales.

## Conclusion

Aucun modèle n'est "meilleur" sur toutes les métriques. Le choix dépend du coût métier des erreurs :

- priorité à la détection des clients à risque → régression logistique ou arbre de décision
- priorité à la réduction des faux positifs → Random Forest
- compromis global / capacité de classement → régression logistique avec le meilleur ROC-AUC

Une étape suivante pertinente serait d'optimiser le seuil de classification selon le coût réel d'un faux négatif et d'un faux positif, puis de faire une validation croisée et une recherche d'hyperparamètres.
