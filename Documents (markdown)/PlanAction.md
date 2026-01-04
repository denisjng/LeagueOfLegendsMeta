Problématique générale

Comment maximiser les chances de victoire dans une partie de League of Legends en s’appuyant sur l’analyse des données de jeu et des modèles de machine learning, via les choix stratégiques (champions, items, draft) et les performances individuelles ?

Objectif global : transformer les données brutes en recommandations actionnables (picks, bans, builds, priorités d’objectifs) et en outils prédictifs.

---

# Plan d’action détaillé (step by step)

## Phase 0 — Préparation et qualité des données
- Rassembler et contrôler : MatchTbl, MatchStatsTbl, TeamMatchTbl, SummonerMatchTbl, ChampionTbl, ItemTbl, RankTbl.
- Nettoyer : IDs champions/items anormaux, runes/sorts à 0 → NaN, filtrer patches hors périmètre.
- Échantillonner : séparer ARAM vs CLASSIC, classé vs non classé (RankFk > 0), patchs récents vs historiques.
- Définir jeux : train/val/test par chronologie de patch pour limiter le leakage temporel.

## I. Compréhension de la méta actuelle (ce qui gagne)

### 1. Efficacité des champions
- Questions : quels champions gagnent le plus par rank, lane, durée ?
- Data Scientist (DS) — étapes
	- Calculer winrate par champion, contrôlé par lane et rank.
	- Détecter sur/sous-performance : champions dont le winrate est > moyenne + écart-type par lane/rank.
- Expert ML — étapes
	- Encodage champion (one-hot ou target encoding contrôlé par CV stratifiée par patch).
	- Modèle de base : régression logistique pour obtention des odds ratios par champion.
	- Modèle non linéaire : Gradient Boosting/Random Forest pour interactions champion × rank.
	- Importance globale (SHAP) pour quantifier l’effet réel du choix du champion sur la probabilité de victoire.

### 2. Impact des items
- Questions : quels items sont associés aux victoires, et dans quels contextes (lane, patch, rank) ?
- DS — étapes
	- Fréquences items (slots 1-6) en victoire/défaite, par lane.
	- Identifier items « core » (présents dans ≥X% des builds gagnants d’un champion) vs « situationnels ».
	- Comparer builds gagnants/perdants via tests proportionnels (chi2) et lift (winrate build / winrate moyen champion).
- ML — étapes
	- Représenter les items : multi-hot sur 6 slots ou top-k items les plus fréquents.
	- Entraîner un modèle de victoire avec items seuls puis items + champion pour mesurer le gain.
	- Extraire l’importance des items (permutation importance ou SHAP) pour prioriser les recommandations.

## II. Optimisation du champion (jouer au mieux)

### 3. Builds optimaux
- Questions : existe-t-il des builds d’items optimaux par champion et contexte ?
- DS — étapes
	- Regrouper les builds complets (items 1-6) par champion ; filtrer les builds avec support d’occurrence suffisant.
	- Clustering de builds (Jaccard / Hamming) pour trouver des archétypes gagnants.
	- Analyser winrate des archétypes par durée de partie (early/late scaling) et par patch.
- ML — étapes
	- Modèle séquentiel simple (ordre des items) vs modèle set-based (ensemble non ordonné).
	- Recommander top-N builds par champion en maximisant la probabilité de victoire prédite.
	- Simuler scénarios (sans item X, avec item Y) pour mesurer l’impact marginal sur la win prob.

### 4. Performances mécaniques (skill vs pick)
- Questions : la victoire dépend-elle plus du champion ou des performances du joueur ?
- DS — étapes
	- Comparer métriques moyennes (Gold, CS, DmgDealt, DmgTaken, KDA) gagnants vs perdants, contrôlé par champion et lane.
	- Corrélations et tests de différence (t-test/Mann-Whitney) par rôle.
	- Elasticité : variation de winrate quand CS/Gold augmentent d’un écart-type.
- ML — étapes
	- Modèles avec et sans variables mécaniques pour mesurer le delta d’AUC/log-loss.
	- SHAP pour classer l’importance relative : champion vs métriques de performance.
	- Conclusion : si les features mécaniques dominent, focus coaching; sinon, focus draft/build.

## III. Draft et contre-stratégies (gagner avant la partie)

### 5. Counter-picks
- Questions : quels matchups réduisent fortement le winrate d’un champion donné ?
- DS — étapes
	- Calculer winrate champion A vs champion B par lane et par patch (fenêtre glissante).
	- Filtrer sur support minimal (>= N matchups) pour fiabilité.
	- Construire tables de counters et marges de winrate (différence vs winrate moyen du champion).
- ML — étapes
	- Modèle d’interactions champion_allié vs champion_adverse (embedding ou one-hot croisé).
	- Importance des paires via SHAP interaction values.
	- Générer une liste priorisée des pires matchups pour chaque champion.

### 6. Champions à bannir
- Questions : quelles menaces bannir contre un champion/équipe spécifique ?
- DS — étapes
	- Croiser fréquence × winrate des champions adverses par lane et patch.
	- Identifier champions à fort impact négatif sur le winrate de notre pick (marge de perte attendue).
- ML — étapes
	- Simuler la proba de victoire avec et sans la présence d’un champion adverse (ablation).
	- Classer les bans par gain attendu de winrate.

## IV. Stratégies globales de victoire

### 7. Facteurs clés de victoire
- Questions : quels facteurs pèsent le plus sur la win prob ?
- DS — étapes
	- Analyse multivariée : champion, items, gold, cs, dégâts, durée.
	- Standardiser et mesurer les contributions (coefficients normalisés, VIF pour multicolinéarité).
- ML — étapes
	- Régression logistique calibrée + modèle arbre (XGBoost) pour robustesse.
	- Feature importance (gain) et SHAP pour ranking final des facteurs.

### 8. Prédiction de l’issue d’une partie
- Questions : peut-on prédire l’issue à partir des choix et stats ?
- DS — étapes
	- Préparer features (champion, items, runes, lane, gold, cs, kda, objectifs) et cible Win.
	- Split temporel train/val/test ; métriques : AUC, log-loss, Brier score, calibration.
	- Courbes de calibration et lift chart pour interpréter.
- ML — étapes
	- Modèles candidats : LogReg, RandomForest, XGBoost/LightGBM.
	- Optimisation d’hyperparamètres (CV par patch ou K-fold stratifié).
	- Export d’un score de victoire + intervalle de confiance; préparation d’une API/outil d’aide à la décision.

---

## Phase d’exécution proposée
1) Nettoyage + EDA rapide (Phase 0). Livrable : data dictionnaire + qualité.
2) I. Méta actuelle : rapports winrates champions/items par lane/rank/durée + visuels.
3) II. Optimisation champion : recommandations de builds + analyse skill vs pick.
4) III. Draft : tables de counters et priorisation des bans.
5) IV. Modèles globaux : importance des facteurs + modèle de prédiction calibré.

## Livrables attendus
- Notebook(s) ou scripts pour reproduction complète.
- Dashboards/visuals synthétiques : heatmaps matchups, top builds, feature importance.
- Tableau de recommandations : picks/bans par lane, builds optimaux par champion.
- Modèle exportable (pkl/onxx) ou API minimale pour prédiction en temps réel ou en draft.