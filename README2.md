1. Objectif du projet

Ce projet a pour objectif de construire un pipeline complet de nettoyage et de préparation de données, applicable à des datasets réels et bruités.

L’idée principale est de passer d’un dataset brut à un dataset exploitable pour un modèle de machine learning, en respectant les bonnes pratiques :

audit des données
traitement des valeurs manquantes
gestion des types incohérents
encodage des variables catégorielles
détection des outliers
prévention de la fuite de données
standardisation des variables
construction d’un pipeline réutilisable
2. Datasets utilisés

Deux datasets ont été utilisés :

Telco Customer Churn

Dataset principal utilisé pour construire et valider toutes les étapes du pipeline.

Variables mixtes (numériques + catégorielles)
Valeurs manquantes cachées (ex : TotalCharges)
Forte dépendance entre certaines variables
FIFA 21 Raw Dataset

Dataset de test “crash test” utilisé pour vérifier la robustesse du pipeline.

Données textuelles complexes (€, M, K, unités mixtes)
Colonnes très bruitées
Forte cardinalité sur certaines variables
Données non structurées
3. Pipeline de traitement

Le pipeline est structuré en plusieurs phases logiques :

Phase 0 — Chargement des données

Importation du dataset et première inspection (shape, types, aperçu).

Phase 1 – Audit qualité
Choix réalisés

Un audit du dataset a été effectué afin d'identifier les types de données, les valeurs manquantes et la répartition de la cible Churn.

Résultats
7043 lignes et 21 colonnes.
Aucun NaN détecté initialement.
Répartition de la cible :
Non : 73,5 %
Oui : 26,5 %
Interprétation

La cible est déséquilibrée. Une simple accuracy pourrait être trompeuse, car un modèle prédisant toujours "Non" obtiendrait déjà environ 73 % de bonnes réponses.

Phase 2 – Nettoyage de TotalCharges
Choix réalisés

La colonne TotalCharges était stockée au format texte. La conversion a été réalisée avec pd.to_numeric(errors="coerce").

Les valeurs non convertibles ont été transformées en NaN puis remplacées par la médiane.

Justification

L'imputation par la médiane permet de conserver les observations tout en limitant l'influence des valeurs extrêmes.

Résultats
11 valeurs manquantes cachées détectées.
Type converti en float64.
Aucun NaN restant après imputation.
Phase 3 – Encodage des variables catégorielles
Choix réalisés
Variables binaires : encodage 0/1.
Variables nominales : One-Hot Encoding.
customerID supprimé.
Justification

customerID est un identifiant unique et n'apporte aucune information prédictive.

Le One-Hot Encoding évite d'introduire un ordre artificiel entre les catégories.

Résultats
Passage de 21 à environ 40 colonnes.
Dataset entièrement numérique.
Phase 4 – Outliers
Choix réalisés

Détection avec la méthode IQR sur :

tenure
MonthlyCharges
TotalCharges
Résultats

Aucun outlier significatif n'a été détecté.

Justification

Les valeurs élevées observées semblent correspondre à de vrais clients et non à des erreurs de saisie. Elles ont donc été conservées.

Phase 5 – Multicolinéarité
Choix réalisés

Analyse des corrélations et calcul des VIF.

Résultats

Une forte relation a été observée entre :

tenure
MonthlyCharges
TotalCharges
Interprétation

TotalCharges est naturellement liée aux deux autres variables puisque le montant total dépend de l'ancienneté et du coût mensuel.

Phase 6 – Variables les plus prédictives
Choix réalisés

Deux méthodes ont été comparées :

corrélation avec la cible ;
importance Random Forest.
Résultats

Les variables les plus influentes sont :

Contract ;
tenure ;
MonthlyCharges.
Interprétation métier

Les clients ayant un contrat mensuel et une faible ancienneté ont davantage tendance à résilier.

Phase 7 – Prévention de la fuite de données
Choix réalisés

Le StandardScaler a été ajusté uniquement sur les données d'entraînement.

Résultats

Les performances obtenues avec et sans fuite sont proches.

Interprétation

Le dataset étant volumineux, les statistiques du train et du test sont similaires. Malgré cela, l'apprentissage sur le train uniquement reste la bonne pratique.

Phase 8 – DataCleaner réutilisable
Choix réalisés

Création d'une classe DataCleaner basée sur le principe fit/transform.

Résultats
Aucun NaN après transformation.
Colonnes train/test parfaitement alignées.
Catégories inconnues correctement ignorées.
Intérêt

Cette approche permet de réutiliser le nettoyage sur d'autres datasets sans réécrire le code.

Phase 9 – Crash Test FIFA 21
Choix réalisés

Le pipeline a été appliqué à un dataset totalement différent du Telco.

Difficultés rencontrées
Montants contenant des symboles monétaires.
Colonnes textuelles à forte cardinalité.
Formats hétérogènes.
Résultats
Dataset nettoyé sans NaN.
Colonnes alignées.
Pipeline fonctionnel sans modification majeure.
Conclusion

Le DataCleaner est suffisamment générique pour traiter des données très différentes.

Phase 10 – Bilan global
Résultats
Dataset final propre.
Pipeline stable.
Aucun problème de fuite détecté.
PCA réalisée pour explorer la structure des données.
