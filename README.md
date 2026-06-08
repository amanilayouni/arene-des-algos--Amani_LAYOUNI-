# Arène des Algos

Ce projet a pour objectif de construire une arène de comparaison de modèles de machine learning.
On entraîne plusieurs algorithmes sur des datasets de classification et on compare leurs performances afin d’identifier le meilleur modèle selon différents critères.

L’objectif n’est pas seulement d’obtenir une bonne accuracy, mais de comprendre comment et pourquoi un modèle performe mieux qu’un autre.

- Datasets utilisés

Deux datasets ont été utilisés :

Breast Cancer Dataset : classification binaire (tumeur bénigne ou maligne)
Wine Dataset : classification multi-classes 

Ces datasets permettent de tester les modèles sur des problèmes simples et plus complexes.

- Algorithmes comparés

Les modèles testés dans l’arène sont :

-- Régression logistique
-- K-Nearest Neighbors (KNN)
-- Arbre de décision

Chaque modèle a été évalué avec le même pipeline :

1 séparation train/test
2 entraînement
3 prédiction
4 calcul de l’accuracy


* Résultats globaux
-- Breast Cancer Dataset

Régression logistique	~0.97–1.00
KNN	~0.90–0.95
Arbre de décision	~0.90–0.94


-- Wine Dataset

Régression logistique	~0.95–1.00
KNN	~0.85–0.95
Arbre de décision	~0.85–0.95


 Champion retenu : 
 *Régression Logistique*

Pourquoi ce modèle ?

Même si plusieurs modèles donnent des résultats proches, la régression logistique est choisie pour plusieurs raisons :

* Performance

Elle obtient les meilleures performances globales sur les deux datasets.

* Stabilité

Elle reste stable même quand on change de dataset ou de type de problème.

* Interprétabilité

Contrairement aux modèles plus complexes, elle permet de comprendre l’impact de chaque variable sur la décision.

* Efficacité

Elle est rapide à entraîner, même sur de grands volumes de données.

 - Analyse des autres modèles
   
KNN
Très sensible au scaling
Bon résultats après normalisation
Coûteux en prédiction (lent sur gros dataset)

Arbre de décision
Très interprétable
Mais peut sur-apprendre facilement
Moins stable entre datasets

-  Impact du scaling

Le scaling a montré que :

KNN est fortement amélioré par la normalisation
La régression logistique est légèrement impactée
Les arbres de décision ne changent quasiment pas


L’accuracy seule ne suffit pas pour juger un modèle.

Il faut aussi prendre en compte :

les types d’erreurs (matrice de confusion)
la robustesse du modèle
la vitesse d’exécution
l’interprétabilité
