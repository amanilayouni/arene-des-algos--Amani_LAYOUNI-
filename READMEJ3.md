Arène des Algo — Projet Machine Learning

1. Présentation générale du projet
Objectif global

Ce projet consiste à construire une arène de comparaison d’algorithmes de Machine Learning sur plusieurs problèmes standards afin d’analyser leurs performances dans différents contextes.

L’objectif n’est pas uniquement d’obtenir de bons scores, mais de comprendre :

Pourquoi un modèle fonctionne mieux qu’un autre
Dans quels cas un algorithme échoue
Quel compromis performance / robustesse / vitesse est optimal
Problèmes étudiés

Le projet couvre 5 grands types de tâches en Machine Learning :

Régression (prédiction de valeurs continues)
Clustering (segmentation non supervisée)
Classification de texte
Classification binaire sur signaux
Benchmark final (compétition entre modèles)
Idée centrale

Chaque phase représente un cas réel de data science, permettant de comparer les algorithmes dans des contextes variés et de construire un leaderboard final (Fight des IA).

 Phase A — Régression : Prix immobiliers en Californie
 Objectif

Prédire le prix médian d’un logement à partir de variables socio-économiques.

Dataset : fetch_california_housing (Scikit-learn)

 Pipeline
 
Étapes réalisées
Chargement du dataset
Séparation train / test
Standardisation des variables
Entraînement de modèles
Modèles testés
Régression Linéaire (baseline)
Random Forest (modèle non linéaire)

 Résultats
Régression Linéaire : performances correctes mais limitées
Random Forest : meilleure performance globale

 Analyse

La régression linéaire repose sur une hypothèse forte : la relation entre variables est linéaire.

Limite : elle ne capture pas les interactions complexes.

La Random Forest est plus performante car elle :

capture les non-linéarités
gère les interactions entre variables
est robuste aux distributions complexes
 Cas limites
 
1. Peu de données (100 lignes)

Le modèle devient instable et perd en généralisation.

 Conclusion : un modèle ML nécessite un volume suffisant de données pour être fiable.

2. Cas adversarial

Exemple : revenu = 0, population élevée

Résultat : prédiction incohérente (négative ou extrême)

 Conclusion : le modèle ne comprend pas les contraintes réelles du monde → nécessité de filtrage et validation des données en production.

 Conclusion Phase A

Le modèle le plus performant est :

 Random Forest

Car il est plus robuste et plus précis que la régression linéaire.

 Phase B — Clustering Airbnb (non supervisé)
 Objectif

Segmenter les annonces Airbnb sans labels (budget, premium, long séjour…).

Dataset : Inside Airbnb (Paris)

 Pipeline
Prétraitement
Sélection des variables numériques :
price
minimum_nights
number_of_reviews
availability_365
Nettoyage des valeurs manquantes (NaN)
Standardisation des données
Modèle
KMeans clustering
Recherche du meilleur k (silhouette score)

 Résultats
k optimal généralement entre 2 et 4
Segments interprétables :
logements économiques
logements intermédiaires
logements premium / longue durée
 Analyse
Importance de la standardisation

Sans standardisation :

la variable price domine totalement
les autres variables deviennent inutiles

 Conclusion : clustering biaisé et non interprétable

 Cas limites
1. Sans standardisation

Les clusters se basent uniquement sur le prix.

 Conclusion : perte totale de sens du clustering

2. Valeur aberrante (outlier 100 000€)

Un seul outlier suffit à :

déformer les centres
modifier les clusters
perturber l’ensemble du modèle

 Conclusion : KMeans est très sensible aux valeurs extrêmes

 Conclusion Phase B

Un clustering fiable nécessite :

données propres
standardisation obligatoire
suppression des outliers


 Phase C — Classification de spam (texte)
 Objectif

Classer des SMS en :

spam
message normal

Dataset : SMS Spam Collection (UCI)

 Pipeline
Vectorisation
TF-IDF (meilleure représentation que bag-of-words classique)
Modèles
Naive Bayes (baseline texte)
Logistic Regression
 Résultats
Naive Bayes : très performant sur données textuelles simples
Logistic Regression : meilleure stabilité globale
 Analyse

TF-IDF améliore la représentation car :

réduit le poids des mots fréquents (“de”, “le”)
augmente le poids des mots discriminants (“urgent”, “gratuit”)
 Cas limites
1. Message vide

Le modèle ne peut pas extraire de features → comportement instable

2. Spam déguisé

Exemple : “ton colis est en attente”

 Le modèle peut être trompé car le vocabulaire ressemble à un message normal

 Conclusion Phase C

Le choix du modèle dépend du besoin :

Naive Bayes → rapide et efficace
Logistic Regression → plus robuste


 Phase D — Classification sonar (mine vs roche)
 Objectif

Détecter si un signal sonar correspond à :

une mine
un rocher

Dataset : Sonar (UCI)

 Pipeline
Standardisation obligatoire
Modèles testés :
Logistic Regression
SVM RBF
Random Forest
 Résultats
SVM RBF : meilleur modèle global
Random Forest : stable mais moins performant
Logistic Regression : baseline correcte
 Analyse

Le SVM est particulièrement adapté car :

petit dataset
beaucoup de features
séparation non linéaire
 Cas limites
Sans standardisation

Le SVM perd fortement en performance.

Signal vide

Le modèle produit une prédiction malgré tout.

 Conclusion : absence de détection des cas anormaux

 Conclusion Phase D

 SVM RBF est le meilleur modèle

Car il est adapté aux données complexes et de petite taille.


 Phase E — Fight des IA (benchmark final)
 Objectif

Comparer plusieurs algorithmes sur un même dataset avec :

même split train/test
même métrique
mêmes conditions expérimentales
 Modèles testés
Logistic Regression
Decision Tree
Random Forest
Gradient Boosting
SVM RBF
 Résultats

Le classement dépend du dataset et de la métrique, mais généralement :

Gradient Boosting → meilleur score
SVM → excellent compromis performance / vitesse
Random Forest → stable
Logistic Regression → baseline
 Analyse

Ce benchmark montre que :

le meilleur modèle dépend du contexte
la performance brute ne suffit pas
le temps d’exécution est un critère important

 Conclusion Phase E

Le meilleur modèle n’est pas seulement celui avec le meilleur score, mais celui qui :

généralise bien
reste stable
est rapide et utilisable en production
 Conclusion générale du projet

Ce projet met en évidence plusieurs principes fondamentaux :

Il n’existe pas de meilleur algorithme universel
Le choix dépend fortement du problème
Le prétraitement est aussi important que le modèle
Les outliers et le scaling peuvent complètement changer les résultats
 Champion final (synthèse globale)
Régression → Random Forest
Clustering → KMeans (avec standardisation)
Texte → Logistic Regression / Naive Bayes
Sonar → SVM RBF
Fight global → Gradient Boosting ou SVM (selon métrique)
