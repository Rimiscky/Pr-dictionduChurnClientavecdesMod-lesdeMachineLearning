# Rapport final — Prédiction du churn client

## 1. Contexte

L'objectif du projet est de construire un modèle de Machine Learning capable d'identifier les clients d'une entreprise de télécommunications qui risquent de résilier leur abonnement.

Le dataset utilisé est **Telco Customer Churn** de Kaggle. Chaque ligne représente un client et contient des informations sur son contrat, ses services, son ancienneté et ses frais.

La variable à prédire est `Churn` :

- `Yes` : le client a quitté l'entreprise ;
- `No` : le client est resté.

Il s'agit donc d'un problème de **classification binaire**.

## 2. Analyse de la qualité des données

Le dataset contient :

- 7 043 clients ;
- 21 colonnes ;
- aucun doublon exact ;
- 11 valeurs problématiques dans `TotalCharges` après conversion en nombre.

`TotalCharges` était lu comme du texte à cause de quelques cellules vides. Ces valeurs sont transformées en valeurs manquantes puis remplacées par la médiane dans le pipeline.

La colonne `customerID` est supprimée car elle identifie chaque client mais n'apporte pas d'information utile pour prédire le churn.

## 3. Analyse exploratoire

Le churn représente environ **26,5 %** des clients.

Cela signifie que les classes sont déséquilibrées : il y a beaucoup plus de clients qui restent que de clients qui partent.

L'analyse montre notamment que le churn varie selon :

- l'ancienneté du client ;
- le type de contrat ;
- le type de service Internet ;
- les frais mensuels ;
- les frais totaux.

Les contrats mensuels apparaissent davantage associés au churn, tandis que les contrats de deux ans sont davantage associés à la fidélité.

## 4. Préparation des données

Les étapes de préparation sont les suivantes :

1. suppression de `customerID` ;
2. conversion de `TotalCharges` en variable numérique ;
3. remplacement des valeurs numériques manquantes par la médiane ;
4. remplacement des valeurs catégorielles manquantes par la modalité la plus fréquente ;
5. One-Hot Encoding des variables catégorielles ;
6. standardisation des variables numériques ;
7. séparation du dataset en 80 % entraînement et 20 % test ;
8. split stratifié pour conserver la même proportion de churn dans les deux groupes.

Le preprocessing est placé dans un `Pipeline` scikit-learn afin d'éviter de mélanger les informations du jeu de test avec celles du jeu d'entraînement.

## 5. Modèles testés

Trois modèles ont été comparés :

### Régression logistique

C'est le modèle de référence. Il est simple, rapide et facile à interpréter.

### Arbre de décision

Il prend ses décisions sous forme de règles successives. Son fonctionnement est facile à visualiser, mais il peut sur-apprendre s'il devient trop profond.

### Random Forest

Il combine plusieurs arbres de décision. Il est généralement plus robuste, mais il est moins simple à expliquer.

## 6. Optimisation

Une petite recherche en grille avec validation croisée à 5 plis a été utilisée.

L'objectif n'était pas de tester des dizaines de paramètres, mais simplement de vérifier quelques valeurs raisonnables.

Les meilleurs paramètres trouvés sont :

- Régression logistique : `C = 10` ;
- Arbre de décision : `max_depth = 5` ;
- Random Forest : `max_depth = 8` et `min_samples_leaf = 5`.

## 7. Résultats

| Modèle | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Régression logistique | 0,740 | 0,506 | 0,783 | 0,615 | 0,841 |
| Arbre de décision | 0,755 | 0,527 | 0,759 | 0,622 | 0,832 |
| Random Forest | 0,757 | 0,528 | 0,778 | **0,629** | **0,843** |

La Random Forest obtient le meilleur ROC-AUC, mais l'écart avec la régression logistique est très faible.

## 8. Modèle retenu

Le modèle retenu pour la présentation est la **régression logistique**.

Ce choix est volontaire :

- ses performances sont proches de celles de la Random Forest ;
- elle détecte environ 78 % des churners ;
- elle est plus simple à expliquer ;
- ses coefficients permettent de mieux comprendre l'influence des variables.

Pour un projet étudiant, il est préférable de choisir un modèle que l'on comprend bien plutôt qu'un modèle légèrement meilleur mais difficile à défendre à l'oral.

## 9. Validation et généralisation

Le modèle est évalué sur un jeu de test qui n'a pas servi à l'entraînement.

La validation croisée permet aussi de vérifier que les performances ne dépendent pas uniquement d'un seul découpage des données.

Les scores de validation et de test restent proches, ce qui indique qu'il n'y a pas de signe évident de sur-apprentissage important.

## 10. Interprétation métier

Les variables qui ressortent le plus sont notamment :

- `tenure` ;
- le type de contrat ;
- `Month-to-month` ;
- `Two year` ;
- la fibre optique ;
- `MonthlyCharges` ;
- `TotalCharges`.

L'entreprise pourrait utiliser le modèle pour attribuer un score de risque à chaque client et contacter en priorité les clients les plus susceptibles de partir.

## 11. Limites

Le modèle ne dit pas pourquoi un client veut partir. Il détecte seulement des profils ressemblant aux clients qui ont churné dans les données historiques.

Les relations observées ne doivent pas être interprétées comme des relations de cause à effet.

## 12. Conclusion

Le projet montre qu'une approche simple de Machine Learning permet déjà d'identifier une part importante des clients à risque.

La régression logistique constitue ici un bon compromis entre performance, simplicité et interprétation.

La Random Forest montre qu'un modèle plus complexe peut légèrement améliorer certains scores, mais le gain reste faible dans ce projet.
