# Résultats du projet Churn

Résultats obtenus avec le dataset Kaggle **Telco Customer Churn**.

## Qualité des données

- 7 043 clients
- 21 colonnes
- 0 doublon exact
- 11 valeurs invalides / manquantes dans `TotalCharges` après conversion
- taux de churn : 26,54 %

Les valeurs numériques manquantes sont remplacées par la médiane dans le pipeline. Les catégories manquantes sont remplacées par la valeur la plus fréquente puis encodées avec One-Hot Encoding.

## Protocole

- cible : `Churn` (`Yes = 1`, `No = 0`)
- suppression de `customerID`
- split train/test : 80/20
- split stratifié
- `random_state = 42`
- standardisation des variables numériques
- `class_weight = "balanced"`
- validation croisée à 5 plis pour la recherche d'hyperparamètres

## Résultats optimisés

| Modèle | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Régression logistique | 0,740 | 0,506 | 0,783 | 0,615 | 0,841 |
| Arbre de décision | 0,755 | 0,527 | 0,759 | 0,622 | 0,832 |
| Random Forest | 0,757 | 0,528 | 0,778 | **0,629** | **0,843** |

## Hyperparamètres retenus

### Régression logistique

- `C = 10`
- ROC-AUC moyen en validation croisée : environ 0,845

### Arbre de décision

- `max_depth = 5`
- `min_samples_split = 2`
- ROC-AUC moyen en validation croisée : environ 0,821

### Random Forest

- `max_depth = 8`
- `min_samples_leaf = 5`
- `n_estimators = 200`
- ROC-AUC moyen en validation croisée : environ 0,848

## Matrices de confusion sur le jeu de test

### Régression logistique

- vrais négatifs : 749
- faux positifs : 286
- faux négatifs : 81
- vrais positifs : 293

### Arbre de décision

- vrais négatifs : 780
- faux positifs : 255
- faux négatifs : 90
- vrais positifs : 284

### Random Forest

- vrais négatifs : 775
- faux positifs : 260
- faux négatifs : 83
- vrais positifs : 291

## Interprétation simple

La **Random Forest** obtient le meilleur ROC-AUC, mais son avantage reste faible par rapport à la régression logistique.

La **régression logistique** détecte environ 78 % des clients qui churnent. Elle est aussi plus facile à expliquer, ce qui en fait un bon choix pour ce projet étudiant.

L'**arbre de décision** est visuel et facile à comprendre, mais ses performances sont un peu moins bonnes.

## Modèle retenu pour la présentation

Le modèle principal retenu est la **régression logistique**.

Ce choix n'est pas basé uniquement sur la meilleure valeur numérique. Il tient aussi compte de la simplicité, de l'interprétation et de la capacité à expliquer clairement le fonctionnement du modèle.

La Random Forest est conservée comme modèle de comparaison pour montrer qu'un modèle plus complexe n'apporte qu'un gain limité dans ce cas.

## Variables importantes

Parmi les variables les plus utiles pour distinguer churners et non-churners :

- `tenure`
- `Contract`
- `Month-to-month`
- `Two year`
- `InternetService = Fiber optic`
- `MonthlyCharges`
- `TotalCharges`

Ces variables sont associées au churn dans les données, mais cela ne prouve pas qu'elles causent directement le churn.

## Conclusion

Le modèle peut aider l'entreprise à identifier les clients présentant un risque de départ plus élevé.

Dans un cas réel, les clients avec un score élevé pourraient être contactés en priorité pour comprendre leur insatisfaction ou leur proposer une offre de fidélisation.
