# Problématique générale

---

> **Comment maximiser ses chances de victoire dans une partie de League of Legends en s'appuyant sur l'analyse des données de jeu et des modèles de machine learning, à travers les choix stratégiques (champions, items, draft) et les performances individuelles en partie ?**

Cette problématique vise à **comprendre**, **quantifier** et **modéliser** les éléments qui influencent la victoire, afin de transformer les données brutes en **recommandations stratégiques exploitables**.

---

## I. Compréhension de la méta actuelle

**_Identifier ce qui gagne le plus dans les données_**

### 1. Analyse de l'efficacité des champions

#### Question de recherche
> Quels champions présentent les meilleurs taux de victoire en fonction du rank, de la lane et de la durée de la partie ?

#### Approche Data Scientist

- **Calcul des taux de victoire par champion**
- **Analyses croisées :**
  - Champion × Rank
  - Champion × Lane
  - Champion × Durée de partie
- **Visualisation et comparaison des distributions**
- **Identification de tendances fortes et de champions surperformants**

**Objectif :**
> Mettre en évidence les champions dominants de la méta et observer les différences de performance entre bas et haut niveau de jeu.

#### Approche Expert IA / Machine Learning

- **Encodage du champion** comme variable explicative
- **Intégration du champion** dans un modèle prédictif
- **Mesure de son poids relatif** dans la prédiction de la victoire

**Objectif :**
> Quantifier l'impact réel du choix du champion sur la probabilité de victoire.

---

### 2. Analyse de l'impact des items

#### Question de recherche
> Quels items sont le plus fréquemment associés aux victoires, et dans quels contextes ?

#### Approche Data Scientist

- **Analyse de fréquence** des items en cas de victoire et de défaite
- **Identification des items « core »** par lane
- **Étude des items situationnels** associés à un taux de victoire élevé
- **Comparaison statistique** entre builds gagnants et perdants

**Objectif :**
> Comprendre quels choix d'items sont corrélés à une augmentation des chances de victoire.

#### Approche Expert IA / Machine Learning

- **Transformation des items** en variables binaires ou catégorielles
- **Intégration des items** comme features du modèle
- **Analyse de l'importance** des items dans la prédiction

**Objectif :**
> Déterminer quels items contribuent le plus à la performance prédictive du modèle.

---

## II. Optimisation du champion

**_Jouer un champion de manière optimale selon les données_**

### 3. Identification des builds optimaux

#### Question de recherche
> Existe-t-il des builds d'items optimaux pour chaque champion selon le contexte de la partie ?

#### Approche Data Scientist

- **Regroupement des builds** par champion
- **Clustering des builds** associés aux victoires
- **Comparaison des builds** gagnants et perdants
- **Analyse du lien** entre build et durée de la partie

**Objectif :**
> Mettre en évidence des builds efficaces et identifier des choix d'items à éviter.

#### Approche Expert IA / Machine Learning

- **Modélisation de la victoire** à partir des builds
- **Apprentissage de combinaisons d'items** performantes
- **Simulation de scénarios** de builds

**Objectif :**
> Apprendre automatiquement les configurations d'items les plus favorables à la victoire.

---

### 4. Importance des performances mécaniques

#### Question de recherche
> La victoire dépend-elle davantage du champion joué ou des performances individuelles du joueur ?

**Variables étudiées :**

| Variable | Description |
|----------|-------------|
| **Gold** | Quantité d'or accumulée |
| **CS** | Creep Score (sbires tués) |
| **Dégâts infligés** | Dommages causés aux ennemis |
| **Dégâts subis** | Dommages reçus |

#### Approche Data Scientist

- **Analyse de corrélation** entre performances et victoire
- **Comparaison des performances moyennes** entre gagnants et perdants
- **Études statistiques** contrôlées par champion

**Objectif :**
> Comprendre le poids des performances individuelles indépendamment du champion.

#### Approche Expert IA / Machine Learning

- **Modèles intégrant** à la fois champion et statistiques de performance
- **Analyse de l'importance relative** des variables
- **Comparaison de modèles** avec et sans variables mécaniques

**Objectif :**
> Mesurer quantitativement si le skill ou le choix du champion est plus déterminant.

---

## III. Draft et contre-stratégies

**_Gagner avant même le début de la partie_**

### 5. Identification des counter-picks

#### Question de recherche
> Quels champions réduisent significativement le taux de victoire d'un champion donné ?

#### Approche Data Scientist

- **Analyse des matchups** fréquents
- **Calcul des winrates** champion contre champion
- **Étude des différences** selon la lane

**Objectif :**
> Construire des tableaux de counters et identifier les matchups défavorables.

#### Approche Expert IA / Machine Learning

- **Modélisation des interactions** entre champions
- **Apprentissage des combinaisons** de champions perdantes
- **Estimation de l'impact** d'un matchup sur la victoire

**Objectif :**
> Automatiser la détection des contre-picks les plus pénalisants.

---

### 6. Détermination des champions à bannir

#### Question de recherche
> Quels champions représentent la plus grande menace face à un champion spécifique ?

#### Approche Data Scientist

**Identification des champions :**
- À **fort taux de victoire**
- **Dominants** sur plusieurs matchups
- **Très présents** dans une lane
- **Analyse combinée** winrate + fréquence

**Objectif :**
> Hiérarchiser les menaces principales pour un champion donné.

#### Approche Expert IA / Machine Learning

- **Simulation de scénarios** avec ou sans certains champions
- **Évaluation de la baisse** de probabilité de victoire
- **Recommandation automatique** de bans

**Objectif :**
> Proposer une stratégie de bannissement basée sur les données.

---

## IV. Stratégies globales de victoire

**_Comprendre ce qui fait réellement gagner une partie_**

### 7. Identification des facteurs clés de victoire

#### Question de recherche
> Quels facteurs influencent le plus la probabilité de victoire ?

**Facteurs étudiés :**

| Facteur | Type |
|---------|------|
| **Champion** | Catégoriel |
| **Items** | Catégoriel/Binaire |
| **Gold** | Numérique |
| **Dégâts** | Numérique |
| **CS** | Numérique |
| **Durée de la partie** | Numérique |

#### Approche Data Scientist

- **Analyse multivariée**
- **Comparaison des distributions**
- **Interprétation des relations** entre variables

**Objectif :**
> Hiérarchiser les éléments déterminants de la victoire.

#### Approche Expert IA / Machine Learning

- **Régression logistique**
- **Random Forest**
- **Analyse de l'importance des variables** (feature importance)

**Objectif :**
> Obtenir un classement quantifié des facteurs de victoire.

---

### 8. Prédiction de l'issue d'une partie

#### Question de recherche
> Peut-on prédire l'issue d'une partie à partir des choix stratégiques et des statistiques de jeu ?

#### Approche Data Scientist

- **Préparation et nettoyage** des données
- **Séparation des jeux** d'entraînement et de test
- **Analyse des performances** du modèle

**Objectif :**
> Évaluer la qualité prédictive et la robustesse des résultats.

#### Approche Expert IA / Machine Learning

- **Entraînement de modèles** prédictifs
- **Évaluation** (accuracy, precision, recall)
- **Interprétation des prédictions**

**Objectif :**
> Construire un modèle capable de transformer les données de jeu en outil d'aide à la décision.

---
