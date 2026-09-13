# Guide oral — Projet Churn

Ce document sert à expliquer le projet simplement, sans apprendre des phrases trop techniques.

## 1. Présenter le problème

> Mon objectif est de prédire quels clients risquent de résilier leur abonnement. La variable cible s'appelle `Churn` et elle contient deux valeurs : Yes ou No.

À retenir : c'est une **classification binaire** parce qu'il y a deux classes possibles.

## 2. Présenter les données

> Le dataset contient 7 043 clients et 21 colonnes. Il y a des informations sur les contrats, les services utilisés, l'ancienneté et les montants facturés.

> Environ 26,5 % des clients ont churné.

À retenir : les classes ne sont pas équilibrées, donc l'Accuracy seule ne suffit pas.

## 3. Expliquer le nettoyage

> J'ai supprimé `customerID` parce que c'est seulement un identifiant. J'ai aussi converti `TotalCharges` en numérique. Quelques cellules étaient vides, donc elles sont devenues des valeurs manquantes. Je les ai remplacées par la médiane.

> Les variables texte ont été transformées en colonnes numériques avec le One-Hot Encoding.

À retenir : un modèle a besoin de données numériques propres.

## 4. Expliquer le train/test

> J'ai séparé les données en deux parties : 80 % pour entraîner le modèle et 20 % pour le tester.

> Le jeu de test n'est pas utilisé pendant l'entraînement. Il sert à vérifier si le modèle fonctionne sur des données qu'il n'a jamais vues.

## 5. Expliquer les trois modèles

### Régression logistique

> C'est mon modèle principal. Il estime la probabilité qu'un client churn. Il est simple et facile à interpréter.

### Arbre de décision

> Il fonctionne comme une suite de questions : par exemple, quel type de contrat ? quelle ancienneté ? puis il arrive à une décision.

### Random Forest

> C'est un ensemble de plusieurs arbres de décision. Chaque arbre donne son avis et la forêt combine leurs décisions.

## 6. Expliquer les métriques

### Accuracy

> C'est la proportion totale de bonnes prédictions.

### Precision

> Parmi les clients que le modèle annonce comme churners, combien churnent réellement ?

### Recall

> Parmi tous les clients qui ont vraiment churné, combien le modèle a réussi à détecter ?

Pour ce projet, le Recall est important parce qu'on veut éviter de rater trop de clients à risque.

### F1-score

> C'est un compromis entre Precision et Recall.

### ROC-AUC

> C'est une mesure globale de la capacité du modèle à différencier les churners des non-churners. Plus le score est proche de 1, mieux c'est.

Tu n'as pas besoin d'expliquer la formule mathématique.

## 7. Expliquer l'optimisation

> J'ai utilisé GridSearchCV pour tester quelques valeurs d'hyperparamètres automatiquement.

> Je n'ai pas testé énormément de combinaisons, car le but du projet est surtout de comprendre le processus.

### Validation croisée

> Au lieu de tester un réglage sur un seul découpage, la validation croisée fait plusieurs découpages du jeu d'entraînement et calcule une moyenne.

À retenir : cela rend le choix du réglage plus fiable.

## 8. Résultats à retenir

### Régression logistique

- Accuracy : environ 74 %
- Recall : environ 78 %
- ROC-AUC : environ 0,84

### Random Forest

- Accuracy : environ 76 %
- Recall : environ 78 %
- ROC-AUC : environ 0,84

> La Random Forest est légèrement meilleure sur le ROC-AUC, mais la différence est très faible.

## 9. Pourquoi choisir la régression logistique ?

> J'ai choisi la régression logistique comme modèle principal parce que ses performances sont proches de la Random Forest, mais elle est beaucoup plus simple à interpréter et à expliquer.

> Pour moi, dans ce projet, le meilleur modèle n'est pas seulement celui qui a le score le plus élevé. Il faut aussi pouvoir comprendre ce qu'il fait.

## 10. Variables importantes

Tu peux citer simplement :

- l'ancienneté (`tenure`) ;
- le type de contrat ;
- les contrats mensuels ;
- les contrats de deux ans ;
- la fibre optique ;
- les frais mensuels ;
- les frais totaux.

Ne dis pas que ces variables "causent" le churn. Dis qu'elles sont **associées** au churn dans les données.

## 11. Matrice de confusion

Si on te montre une matrice :

- vrai positif : le modèle prédit churn et le client churn vraiment ;
- faux positif : le modèle prédit churn mais le client reste ;
- faux négatif : le modèle prédit qu'il reste mais le client churn ;
- vrai négatif : le modèle prédit qu'il reste et il reste vraiment.

Pour l'entreprise, le faux négatif peut être coûteux car elle ne détecte pas un client qui va partir.

## 12. Limites du projet

> Le modèle apprend à partir de données historiques. Il peut détecter un risque mais il ne sait pas expliquer la vraie raison personnelle du départ d'un client.

> Il faudrait aussi vérifier régulièrement que les comportements des clients ne changent pas dans le temps.

## 13. Conclusion orale courte

> J'ai nettoyé et analysé les données, puis comparé trois modèles de classification. La Random Forest donne légèrement le meilleur ROC-AUC, mais j'ai retenu la régression logistique car elle offre des performances très proches tout en étant plus simple à expliquer. Le modèle pourrait servir à identifier les clients à risque afin que l'entreprise puisse agir avant leur départ.

## 14. Questions probables du jury

**Pourquoi supprimer customerID ?**

Parce que c'est un identifiant unique, pas une caractéristique du comportement du client.

**Pourquoi utiliser la médiane ?**

Parce qu'elle est moins sensible aux valeurs extrêmes que la moyenne.

**Pourquoi pas seulement l'Accuracy ?**

Parce que seulement environ 26,5 % des clients churnent. Un modèle peut avoir une bonne Accuracy tout en détectant mal les churners.

**Pourquoi la régression logistique ?**

Parce qu'elle est simple, interprétable et presque aussi performante que la Random Forest ici.

**Qu'est-ce qu'un hyperparamètre ?**

C'est un réglage choisi avant l'entraînement, par exemple la profondeur maximale d'un arbre.

**Qu'est-ce que le sur-apprentissage ?**

C'est quand un modèle apprend trop précisément les données d'entraînement et devient moins bon sur de nouvelles données.
