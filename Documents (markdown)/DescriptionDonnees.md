# Documentation Détaillée du Dataset League of Legends Meta

## Vue d'ensemble

Ce dataset contient des données complètes sur les matchs de League of Legends, incluant les informations sur les champions, les objets, les statistiques des joueurs, les équipes et les classements. Le dataset est organisé en 8 fichiers CSV interconnectés permettant une analyse approfondie de la méta-game (stratégies dominantes) du jeu.

---

## 📋 Index des Fichiers

1. [ChampionTbl.csv](#championtblcsv)
2. [ItemTbl.csv](#itemtblcsv)
3. [RankTbl.csv](#ranktblcsv)
4. [MatchTbl.csv](#matchtblcsv)
5. [SummonerMatchTbl.csv](#summonermmatchtblcsv)
6. [MatchStatsTbl.csv](#matchstatstblcsv)
7. [TeamMatchTbl.csv](#teammatchtblcsv)

---

## ChampionTbl.csv

### Description Générale
Table de référence contenant la liste de **tous les champions jouables** dans League of Legends. Cette table sert de table de lookup pour identifier les champions par leurs identifiants numériques uniques.

### Taille du Fichier
- **Nombre de lignes** : 175 champions
- **Nombre de colonnes** : 2

### Structure Détaillée des Colonnes

| Colonne | Type | Description | Exemple | Particularités |
|---------|------|-------------|---------|-----------------|
| **ChampionId** | Numérique (Entier) | Identifiant unique du champion dans le système Riot Games | `0`, `1`, `2`, ... | L'ID `0` représente "No Champion" (aucun champion sélectionné). C'est la clé primaire du tableau. |
| **ChampionName** | Texte (String) | Nom du champion tel qu'affiché en jeu | `Annie`, `Olaf`, `Galio`, `TwistedFate` | Noms sans espaces avec capitalisation particulière (CamelCase pour les noms composés). Utilisé à titre informatif. |
| **ChampionLane** | Texte (String) | Rôle ou voie principale du champion | `MID`, `JUNGLE`, `TOP`, `BOTTOM`, `SUPPORT`, `NONE` | Indique la lane principale du champion. `NONE` pour "No Champion". |

### Observations Clés
- Le champion avec ID `0` ("No Champion") est un enregistrement spécial utilisé pour les matchs ARAM ou pour indiquer l'absence de champion
- Les IDs ne sont **pas consécutifs** (on trouve `0`, `1`, `2`, `3`, ..., `50`, `51`, mais `46`, `47`, `49` manquent) car le jeu a supprimé ou changé certains champions au fil des années
- Total d'environ **175 champions** jouables (le jeu ajoute régulièrement de nouveaux champions)
- Les noms suivent une convention de nommage spécifique (pas d'espaces, noms composés en CamelCase)

### Utilité dans l'Analyse
- **Table de référence** pour tous les autres fichiers utilisant les IDs de champions
- **Jointure obligatoire** pour obtenir les noms lisibles à partir des IDs numériques
- Permet d'analyser **les préférences des champions** par rôle, par rang, ou par patch

---

## ItemTbl.csv

### Description Générale
Table de référence contenant **tous les objets (items) du jeu**. Les objets sont les équipements que les joueurs achètent pour augmenter les statistiques de leurs champions. Ce dataset inclut les objets de base, composants et items finaux.

### Taille du Fichier
- **Nombre de lignes** : 637 objets
- **Nombre de colonnes** : 2

### Structure Détaillée des Colonnes

| Colonne | Type | Description | Exemple | Particularités |
|---------|------|-------------|---------|-----------------|
| **ItemID** | Numérique (Entier) | Identifiant unique de l'objet dans le système Riot Games | `1001`, `1006`, `1011`, ... | C'est la clé primaire. Les IDs commencent à 1000+. |
| **ItemName** | Texte (String) | Nom de l'objet tel qu'affiché en jeu | `Boots`, `Faerie Charm`, `Ruby Crystal`, `Giant's Belt` | Noms lisibles en anglais, avec capitalisation appropriée. Certains noms contiennent des caractères spéciaux. |

### Observations Clés
- **637 objets** uniques en total
- Les IDs ne sont **pas consécutifs** (1001, 1004, 1006, 1011, 1018...)
- Inclut:
  - **Objets de base** : Boots, Ruby Crystal (composants simples)
  - **Objets finaux** : Items complètement construits (ex: BF Sword)
  - **Objets spéciaux** : Certains objets spécifiques aux modes de jeu (ex: Turret Plating avec ID 1515)
  - **Objets invalides/obsolètes** : Certains IDs renvoient à des objets qui ne sont plus en jeu ou à des valeurs corrompues
- Certains noms semblent anormaux (ex: `Mosstomper Seedling`, `Gustwalker Hatchling`) indiquant des objets exotiques ou temporaires
- Des IDs élevés (1500-1600) qui semblent être des modifications ou des objets spéciaux

### Utilité dans l'Analyse
- **Table de référence** pour les objets équipés par les joueurs (dans MatchStatsTbl)
- Permet d'identifier **les builds (combinaisons d'objets) les plus populaires**
- Analyse des **préférences d'objets par champion ou par rôle**
- Détection des **tendances d'items** à travers les patches

---

## RankTbl.csv

### Description Générale
Table de référence contenant les **rangs de classement (divisions)** du système de classement compétitif de League of Legends. Chaque joueur est assigné à un rang basé sur ses victoires/défaites.

### Taille du Fichier
- **Nombre de lignes** : 11 rangs
- **Nombre de colonnes** : 2

### Structure Détaillée des Colonnes

| Colonne | Type | Description | Exemple | Particularités |
|---------|------|-------------|---------|-----------------|
| **RankId** | Numérique (Entier) | Identifiant unique du rang | `0`, `1`, `2`, ..., `10` | C'est la clé primaire. Valeurs de 0 à 10 (11 rangs au total). Ordonnés du plus bas au plus haut. |
| **RankName** | Texte (String) | Nom du rang affiché | `Unranked`, `Iron`, `Bronze`, `Silver`, `Gold`, `Platinum`, `Emerald`, `Diamond`, `Master`, `Grandmaster`, `Challenger` | Noms en anglais. Le rang `Unranked` (0) représente les joueurs sans classement. |

### Hiérarchie des Rangs (du plus bas au plus haut)
```
0: Unranked (aucun classement)
1: Iron (plus faible)
2: Bronze
3: Silver
4: Gold
5: Platinum
6: Emerald (intermédiaire)
7: Diamond (avancé)
8: Master (très avancé)
9: Grandmaster (élite)
10: Challenger (le plus haut)
```

### Observations Clés
- **11 rangs au total** (incluant Unranked)
- **Nouveaux rangs** : Emerald (RankId = 6) a été ajouté récemment dans League of Legends
- Les rangs **Unranked** (0) incluent les matchs en mode non-classé (ARAM, modes spéciaux)
- Chaque rang (excepté Master, Grandmaster et Challenger) est subdivisé en **4 sous-divisions (tiers)** : IV, III, II, I (mais ces sous-divisions ne sont pas stockées dans cette table)

### Utilité dans l'Analyse
- **Table de référence** pour filtrer les matchs par niveau de compétition
- **Analyse comparative** : voir comment la méta change selon le rang
- **Filtrage** des matchs classés vs non-classés
- **Identification des matchs professionnels** vs matchs amateurs

---

## MatchTbl.csv

### Description Générale
Table **centrale** contenant **tous les matchs du dataset**. Chaque ligne représente un match unique avec ses métadonnées générales (patch, type de queue, durée).

### Taille du Fichier
- **Nombre de lignes** : 110,479 matchs
- **Nombre de colonnes** : 5

### Structure Détaillée des Colonnes

| Colonne | Type | Description | Exemple | Particularités |
|---------|------|-------------|---------|-----------------|
| **MatchId** | Texte (String) | Identifiant unique du match | `EUN1_3707659547`, `EUW1_7565751492` | C'est la clé primaire. Format: `{RegionCode}_{NuméroUniqueMatch}`. Régions: EUW1, EUN1, etc. |
| **Patch** | Texte (String) | Version du patch du jeu au moment du match | `14.23.636.9832`, `15.13.693.4876` | Format: `{Major}.{Minor}.{Build}.{Revision}`. Patch 14 = Season 14, Patch 15 = Season 15, etc. |
| **QueueType** | Texte (String) | Type de file d'attente / mode de jeu | `ARAM`, `CLASSIC` | **ARAM** = mode Aléatoire Tous Magiciens (map réduite, tous les champions au hasard). **CLASSIC** = Invocateur's Rift (mode classique 5v5). |
| **RankFk** | Numérique (Entier) | Clé étrangère vers le rang (RankTbl) | `0`, `6` | Identifie le niveau de rang du match. `0` = Unranked (matchs ARAM ou non-classés). Rejoint avec RankTbl.RankId. |
| **GameDuration** | Numérique (Entier) | Durée totale du match en **secondes** | `1173`, `1986`, `920` | Représente le temps de jeu pur. Pour convertir en minutes: diviser par 60. Exemple: 1173 sec ≈ 19.5 minutes. |

### Observations Clés
- **110,479 matchs uniques** dans le dataset
- **Deux régions principales** : 
  - **EUN1** = Europe Nordic & East
  - **EUW1** = Europe West
- **Deux modes principaux** :
  - **ARAM** : Mode amusant et aléatoire (uniquement Unranked)
  - **CLASSIC** : Mode classique 5v5 (peut être classé ou non)
- **Patches variés** : données couvrent plusieurs versions du jeu (patch 14.23 à patch 15.13 dans les exemples)
- **Durée des matchs** : généralement entre **15 minutes (~900 sec)** et **40 minutes (~2400 sec)**
  - Matches rapides (victoire écrasante) : < 20 minutes
  - Matches standards : 25-35 minutes
  - Matches longues (très compétitives) : 35+ minutes

### Relations avec Autres Tables
- **Clé primaire** pour joindre avec SummonerMatchTbl et TeamMatchTbl
- Jointure avec **RankTbl** via RankId pour obtenir le nom du rang
- Jointure avec **MatchStatsTbl** via SummonerMatchTbl

### Utilité dans l'Analyse
- **Base de tous les analyses** sur les matchs
- **Calcul de statistiques** : taux de victoire par patch, durée moyenne par mode
- **Analyse temporelle** : évolution de la méta entre patches
- **Filtrage** : sélectionner matchs classés, matchs ARAM, matchs rapides, etc.

---

## SummonerMatchTbl.csv

### Description Générale
Table **d'association joueur-match** qui établit le **lien entre les invocateurs (joueurs) et les champions qu'ils ont joués dans chaque match**. Chaque ligne représente un joueur jouant un champion spécifique dans un match donné.

### Taille du Fichier
- **Nombre de lignes** : 223,154 enregistrements
- **Nombre de colonnes** : 4

### Structure Détaillée des Colonnes

| Colonne | Type | Description | Exemple | Particularités |
|---------|------|-------------|---------|-----------------|
| **SummonerMatchId** | Numérique (Entier) | Identifiant unique de l'enregistrement | `1`, `2`, `3`, ..., `223154` | C'est la clé primaire. Index séquentiel incrémenté. |
| **SummonerFk** | Numérique (Entier) | Clé étrangère vers l'invocateur (joueur) | `1` | ID unique du joueur. Identifie quel joueur a participé. Remarque: cette table ne contient pas le détail des joueurs (juste l'ID). |
| **MatchFk** | Texte (String) | Clé étrangère vers le match (MatchTbl) | `EUW1_7565751492`, `EUN1_3707659547` | Référence le MatchId unique. Rejoint avec MatchTbl.MatchId. |
| **ChampionFk** | Numérique (Entier) | Clé étrangère vers le champion (ChampionTbl) | `902`, `16`, `103`, `800`, `127` | ID du champion joué par ce joueur dans ce match. Rejoint avec ChampionTbl.ChampionId. |

### Observations Clés
- **223,154 enregistrements** = 223,154 "joueurs dans un match"
- **Ratio de ~2 enregistrements par match** : 223,154 / 110,479 ≈ 2.02
  - Cela signifie que chaque match a en moyenne 2 enregistrements (match 5v5 = 10 joueurs, mais il y en a seulement ~2 par match dans cette table)
  - **Indication** : cette table ne contient que des **joueurs sélectionnés ou filtrés** (peut-être seulement le summoner principal et son équipe, ou données manquantes pour certains joueurs)
- **IDs de champions spéciaux** : certain IDs semblent anormaux (ex: `223157`, `223089`, `447108`) ce qui pourrait indiquer:
  - Des données corrompues
  - Des champions supprimés ou des versions spéciales
  - Des valeurs d'erreur ou placeholder

### Relations avec Autres Tables
- **Clé étrangère vers MatchTbl** : joindre via MatchFk = MatchTbl.MatchId
- **Clé étrangère vers ChampionTbl** : joindre via ChampionFk = ChampionTbl.ChampionId
- **Lié à MatchStatsTbl** via SummonerMatchId

### Utilité dans l'Analyse
- **Identification des champions joués** dans chaque match
- **Analyse des champions** : quelle équipe (Blue/Red) joue quel champion
- **Jointure essentielle** pour accéder aux statistiques détaillées via MatchStatsTbl
- **Calcul des statistiques par champion** : taux de victoire, pick rate, ban rate
- **Analyse des compositions de team** : quels champions sont souvent jouées ensemble

---

## MatchStatsTbl.csv

### Description Générale
Table **principale des statistiques de performance individuelle**. Contient des **statistiques détaillées pour chaque joueur dans chaque match**, incluant kills, morts, damages, objets équipés, runes, et bien plus. C'est la table la plus riche en informations.

### Taille du Fichier
- **Nombre de lignes** : 223,154 enregistrements (une ligne par joueur par match)
- **Nombre de colonnes** : 33

### Structure Détaillée des Colonnes

#### Colonnes de Clé et Identification

| Colonne | Type | Description | Exemple |
|---------|------|-------------|---------|
| **MatchStatsId** | Numérique (Entier) | Identifiant unique de la statistique | `1`, `2`, `3`, ... |
| **SummonerMatchFk** | Numérique (Entier) | Clé étrangère vers SummonerMatchTbl | Relie à SummonerMatchTbl.SummonerMatchId |

#### Colonnes de Minions et Économie

| Colonne | Type | Description | Exemple | Plage Typique | Interprétation |
|---------|------|-------------|---------|---------------|----|
| **MinionsKilled** | Numérique (Entier) | Nombre de minions tués | `30`, `29`, `34`, `51`, `0` | 0-300+ | **CS** (Creep Score). Indicateur clé de farming. Minage pauvre: <3/min, Excellent: >8/min |
| **TotalGold** | Numérique (Entier) | Or total accumulé pendant le match | `7058`, `9618`, `9877`, `12374` | 1000-25000+ | Or gagné via minions, tués, et passif. Prédicteur fort de puissance d'équipe. |

#### Colonnes de Dégâts

| Colonne | Type | Description | Exemple | Plage Typique | Interprétation |
|---------|------|-------------|---------|---------------|----|
| **DmgDealt** | Numérique (Entier) | Total de dégâts infligés à l'ennemi | `4765`, `8821`, `6410`, `22206` | 1000-100000+ | Dégâts physiques + magiques + vrais dégâts. Indicateur de contribution au combat. |
| **DmgTaken** | Numérique (Entier) | Total de dégâts encaissés | `12541`, `14534`, `19011`, `14771` | 5000-80000+ | Dégâts absorbés (avant armure). Élevé = rôle tank ou engagement important. |
| **TurretDmgDealt** | Numérique (Entier) | Dégâts infligés aux tourelles ennemies | `0`, `1`, `3`, `2` | 0-50+ | Capacité à détruire la structure ennemie. Souvent faible pour les rôles supports. |

#### Colonnes de Performance

| Colonne | Type | Description | Exemple | Plage Typique | Interprétation |
|---------|------|-------------|---------|---------------|----|
| **kills** | Numérique (Entier) | Nombre de champions tués | `0`, `2`, `8`, `13`, `15`, `25` | 0-30+ | Éliminations ennemies. Élevé = joueur agressif/dominant. |
| **deaths** | Numérique (Entier) | Nombre de fois mort | `2`, `5`, `4`, `8`, `10`, `15` | 0-20+ | Décès du joueur. Élevé = manque d'esquive ou surextension. |
| **assists** | Numérique (Entier) | Nombre d'assists (aide à la tuerie) | `12`, `23`, `22`, `35`, `28`, `19` | 0-50+ | Contribution indirecte aux kills. Rôles support = plus d'assists. |

#### Colonnes de Position (Lane)

| Colonne | Type | Description | Exemple | Valeurs Possibles |
|---------|------|-------------|---------|-------------------|
| **Lane** | Texte (String) | Position du joueur sur la carte | `BOTTOM`, `TOP`, `MIDDLE`, `NONE` | **TOP**: Lane supérieur. **MIDDLE**: Jungle/Mid. **BOTTOM**: Lane inférieur (ADC+Support). **NONE**: Autres/Jungle. |

#### Colonnes de Résultat

| Colonne | Type | Description | Exemple | Valeurs | Interprétation |
|---------|------|-------------|---------|--------|---|
| **Win** | Numérique (Binaire) | Résultat du match | `0`, `1` | **0** = Défaite, **1** = Victoire | Résultat de l'équipe du joueur (tous les joueurs de la même équipe ont la même valeur). |

#### Colonnes des Objets

| Colonne | Type | Description | Exemple | Max | Notes |
|---------|------|-------------|---------|-----|-------|
| **item1** à **item6** | Numérique (Entier) | Objets équipés dans 6 slots | `3870`, `2055`, `3107` | 6 items | Références ItemTbl.ItemID. **item6** = consommable ou item spécial. Valeur `0` = slot vide. |

**Exemple d'analyse d'objets** :
```
Joueur avec items: [3870, 2055, 3107, 3171, 6620, 2022]
Pour identifier les noms: joindre avec ItemTbl sur chaque ItemID
```

#### Colonnes de Runes (Keystones)

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **PrimaryKeyStone** | Numérique (Entier) | Rune Keystonethe principale | `8465`, `8214`, `8112`, `8128` | ID unique de la rune. Détermine la stratégie (offensif, défensif, utilitaire). |
| **PrimarySlot1** à **PrimarySlot3** | Numérique (Entier) | Runes additionnelles du chemin principal | `8463`, `8473`, `8453` | Runes mineures accompagnant le Keystone. |
| **SecondarySlot1** à **SecondarySlot2** | Numérique (Entier) | Runes du chemin secondaire | `8345`, `8347` | Runes provenant d'un second chemin de runes. |

#### Colonnes des Sorts d'Invocateur

| Colonne | Type | Description | Exemple | Valeurs Courantes |
|---------|------|-------------|---------|-------------------|
| **SummonerSpell1** | Numérique (Entier) | Premier sort d'invocateur | `4`, `2`, `14`, `21`, `32`, `7` | **4** = Flash (téléportation). **14** = Smite (jungle). **7** = Heal (soin). **21** = Ignite (dégâts + anti-heal). |
| **SummonerSpell2** | Numérique (Entier) | Deuxième sort d'invocateur | `7`, `14`, `21`, `32` | Diffère selon le rôle/stratégie. |

#### Colonnes de Maîtrise et Progression

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **CurrentMasteryPoints** | Numérique (Entier) | Points de maîtrise gagnés dans le match | `902`, `16`, `103`, `127`, `61`, `800` | Points accordés en fin de match basés sur la performance (S+, S, A, etc). Élevé = meilleure performance. |

#### Colonnes de Championnats Tués

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **EnemyChampionFk** | Numérique (Entier) | Champion ennemi principal afronté (?) | `51`, `236`, `498`, `54` | Peut représenter un champion spécifique d'intérêt ou d'opposition. |

#### Colonnes d'Objectifs

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **DragonKills** | Numérique (Entier) | Nombre de dragons tués | `0`, `1`, `2`, `3`, `4` | Objectifs épiques donnant des bonus. 0-4 généralement. |
| **BaronKills** | Numérique (Entier) | Nombre de Barons tués | `0`, `1` | Boss ultime donnant "Empowerment" temporaire. Rarement > 1 par match. |
| **visionScore** | Numérique (Entier) | Score de vision (nombre de wards) | `67`, `88`, `97`, `0`, `55`, `99`, `60` | Représente la contribution aux contrôle de vision. Rôles support: élevé, Roles carry: bas. |

### Observations Clés Générales

1. **Données complètes** : 223,154 enregistrements (10 joueurs par match théoriquement, mais ratio ~2:1 suggère des données filtrées)

2. **Valeurs anormales** : 
   - Certains items ont des IDs très grands (ex: `223157`, `447108`) potentiellement corrompus
   - Certains Champion IDs dans EnemyChampionFk semblent hors limites

3. **Données manquantes** : 
   - Runes pour certains matchs contiennent `0` (absent ou non renseigné)
   - Some minions killed = 0 pour des non-carries (supports)

### Utilité dans l'Analyse
- **Analyse de performance** : wins/loss corrélés à kills, farming, dégâts
- **Analyse des builds** : quels items construisent les joueurs sur quel champion
- **Analyse des runes** : Keystones populaires par matchups
- **Matchup analysis** : performance vs certains champions ennemis
- **Prédiction de victoire** : features ML puissantes (kills, gold, damage)

---

## TeamMatchTbl.csv

### Description Générale
Table contenant les **statistiques au niveau d'équipe pour chaque match**. Chaque match a 2 enregistrements (un pour l'équipe Blue, un pour l'équipe Red), avec les compositions de champions, les kill-scores, les objectifs pris et les résultats.

### Taille du Fichier
- **Nombre de lignes** : 97,885 enregistrements
- **Nombre de colonnes** : 24

### Structure Détaillée des Colonnes

#### Colonnes de Clé et Identification

| Colonne | Type | Description | Exemple |
|---------|------|-------------|---------|
| **TeamID** | Numérique (Entier) | Identifiant unique de l'enregistrement équipe-match | `1`, `2`, `3`, ... |
| **MatchFk** | Texte (String) | Clé étrangère vers le match | `EUW1_7565751492`, `EUW1_7564368646` |

#### Colonnes de Composition des Champions (Équipe Blue)

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **B1Champ** à **B5Champ** | Numérique (Entier) | Champions de l'équipe Blue (positions 1-5) | `897`, `154`, `157`, `51`, `902` | **B1** = Top. **B2** = Jungle. **B3** = Mid. **B4** = ADC. **B5** = Support. Réference ChampionTbl.ChampionId. |

#### Colonnes de Composition des Champions (Équipe Red)

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **R1Champ** à **R5Champ** | Numérique (Entier) | Champions de l'équipe Red (positions 1-5) | `164`, `5`, `25`, `221`, `497` | Même structure que Blue. L'équipe Red est l'équipe "ennemie" pour Blue. |

#### Colonnes des Objectifs - Équipe Blue

| Colonne | Type | Description | Exemple | Plage Typique | Notes |
|---------|------|-------------|---------|---------------|----|
| **BlueBaronKills** | Numérique (Entier) | Nombre de Barons tués par Blue | `0`, `1` | 0-1 | Baron Nashor donne "Empowerment" (buff temporaire puissant). |
| **BlueRiftHeraldKills** | Numérique (Entier) | Nombre de Rift Heralds tués par Blue | `1`, `0`, `1` | 0-2 | Heraut du Gouffre pousse les vagues et détruit tourelles. |
| **BlueDragonKills** | Numérique (Entier) | Nombre de Dragons tués par Blue | `1`, `3`, `2`, `4` | 0-4+ | Dragons donnent des stacks permanents (+stats). |
| **BlueTowerKills** | Numérique (Entier) | Nombre de tourelles détruites par Blue | `3`, `10`, `7`, `2` | 0-20+ | Indica destruction des structures de Base. |
| **BlueKills** | Numérique (Entier) | Total de kills (champions tués) pour Blue | `13`, `39`, `27`, `55`, `42`, `73` | 5-100+ | Somme des kills de tous les joueurs Blue. |

#### Colonnes des Objectifs - Équipe Red

| Colonne | Type | Description | Exemple | Notes |
|---------|------|-------------|---------|-------|
| **RedBaronKills** | Numérique (Entier) | Barons tués par Red | `1`, `0` | 0-1 |
| **RedRiftHeraldKills** | Numérique (Entier) | Rift Heralds tués par Red | `0`, `1`, `2` | 0-2 |
| **RedDragonKills** | Numérique (Entier) | Dragons tués par Red | `3`, `1`, `0`, `4` | 0-4+ |
| **RedTowerKills** | Numérique (Entier) | Tourelles détruites par Red | `8`, `3`, `8`, `4`, `0` | 0-20+ |
| **RedKills** | Numérique (Entier) | Total de kills pour Red | `26`, `33`, `37`, `39`, `0`, `91` | 5-100+ |

#### Colonnes de Résultat - Équipe Red

| Colonne | Type | Description | Exemple | Valeurs | Interprétation |
|---------|------|-------------|---------|---------|---|
| **RedWin** | Numérique (Binaire) | Victoire de l'équipe Red | `1`, `0` | **1** = Victoire, **0** = Défaite | Le team Red a remporté le match. |

#### Colonnes de Résultat - Équipe Blue

| Colonne | Type | Description | Exemple | Valeurs | Interprétation |
|---------|------|-------------|---------|---------|---|
| **BlueWin** | Numérique (Binaire) | Victoire de l'équipe Blue | `0`, `1` | **1** = Victoire, **0** = Défaite | Le team Blue a remporté le match. Contraste avec RedWin (l'un des deux = 1, l'autre = 0). |

### Observations Clés

1. **Symétrie des résultats** : À chaque ligne, exactement l'une des valeurs suivantes est vraie:
   - **BlueWin = 1 ET RedWin = 0** (Blue gagne)
   - **BlueWin = 0 ET RedWin = 1** (Red gagne)

2. **Nombre d'enregistrements** : 97,885 ≈ 110,479 / 1.13
   - Chaque match devrait avoir 2 enregistrements (Blue + Red)
   - Le ratio suggère que **certains matchs ne sont pas complètement représentés**
   - Possible raison: données incomplètes ou filtrées

3. **Compositions de Champions** :
   - Chaque team a 5 champions (positions fixes)
   - **B1-B5 = Top, Jungle, Mid, ADC, Support** (ordre standard)
   - Champions dupliqués = permis (mais rare) si 2 champions identiques en picks différents

4. **Statistiques d'Objectifs** :
   - Dragons: le compte peut aller jusqu'à 4+ car plusieurs dragons sont disponibles par match
   - Baron: rarement > 1 par équipe (une seule occurrence généralement)
   - Rift Herald: peut être pris 2x au début du jeu
   - Tourelles: corrélée à la durée et à la force de l'équipe

### Relations avec Autres Tables

- **Jointure avec MatchTbl** : via MatchFk = MatchTbl.MatchId
- **Jointure avec ChampionTbl** : via B1Champ-B5Champ et R1Champ-R5Champ

### Utilité dans l'Analyse

1. **Analyse au niveau équipe** :
   - Taux de victoire basé sur compositions (quels 5 champions ensemble gagnent)
   - Impact des objectifs sur la victoire (baron = win rate +X%)

2. **Picks/Bans métrique** :
   - Champions les plus pickés (fréquence en B1-B5/R1-R5)
   - Synergies des compositions (compo offensives vs défensives)

3. **Impact des objectifs** :
   - Dragons tués + victoire (plus de dragons = meilleure réussite?)
   - Baron comme "game changer" décisif
   - Importance de détruire les tourelles

4. **Prédiction** :
   - Feature importante: composition, objectifs, kills pour ML
   - Corrélation: BlueKills + BlueDragonKills → BlueWin?

---

## 🔗 Relations et Hiérarchie des Tables

### Diagramme de Relation Entité-Association (Simplifié)

```
ChampionTbl (Champion Master Table)
    ↓
    └─→ MatchStatsTbl.EnemyChampionFk
    └─→ SummonerMatchTbl.ChampionFk
    └─→ TeamMatchTbl.B1Champ-B5Champ, R1Champ-R5Champ

ItemTbl (Item Master Table)
    ↓
    └─→ MatchStatsTbl.item1-item6

RankTbl (Rank Master Table)
    ↓
    └─→ MatchTbl.RankFk

MatchTbl (Match Facts)
    ↓
    ├─→ MatchStatsTbl.SummonerMatchFk → SummonerMatchTbl.SummonerMatchId
    ├─→ TeamMatchTbl.MatchFk
    └─→ SummonerMatchTbl.MatchFk

SummonerMatchTbl (Summoner-Match Association)
    ↓
    └─→ MatchStatsTbl.SummonerMatchFk
```

### Flux de Jointure Typique

```
MatchTbl 
  ├─ JOIN RankTbl ON RankTbl.RankId = MatchTbl.RankFk
  └─ JOIN TeamMatchTbl ON TeamMatchTbl.MatchFk = MatchTbl.MatchId
       ├─ JOIN ChampionTbl ON ChampionTbl.ChampionId = TeamMatchTbl.B1Champ
       ├─ JOIN ChampionTbl ON ChampionTbl.ChampionId = TeamMatchTbl.B2Champ
       └─ ... (pour tous les champions)

MatchTbl 
  └─ JOIN SummonerMatchTbl ON SummonerMatchTbl.MatchFk = MatchTbl.MatchId
       ├─ JOIN ChampionTbl ON ChampionTbl.ChampionId = SummonerMatchTbl.ChampionFk
       └─ JOIN MatchStatsTbl ON MatchStatsTbl.SummonerMatchFk = SummonerMatchTbl.SummonerMatchId
            ├─ JOIN ItemTbl ON ItemTbl.ItemID = MatchStatsTbl.item1
            ├─ JOIN ItemTbl ON ItemTbl.ItemID = MatchStatsTbl.item2
            └─ ... (pour tous les items)
```

---

## 📊 Statistiques Globales du Dataset

| Métrique | Valeur |
|----------|--------|
| **Nombre total de matchs** | 110,479 |
| **Nombre total de joueurs-matchs** | 223,154 |
| **Nombre de champions** | 175 |
| **Nombre d'objets** | 637 |
| **Nombre de rangs** | 11 |
| **Durée moyenne des matchs** | ~25-30 minutes |
| **Matchs ARAM** | ~30-40% (estimation) |
| **Matchs CLASSIC** | ~60-70% (estimation) |
| **Données couvrant** | Patch 14.23 à 15.13 (~1-2 mois de données) |
| **Régions** | EUW1, EUN1 (Europe) |

---

## 📝 Conclusion

Ce dataset est une **ressource riche et multi-facettes** pour l'analyse de League of Legends. Il offre une visibilité complète sur :

- **Les champions** et leur popularité/viabilité
- **Les builds** et stratégies d'équipement
- **Les performances individuelles** via des métriques détaillées
- **Les stratégies d'équipe** via compositions et contrôle d'objectifs
- **L'impact du patch** via numéros de version

Avec une structure bien-organisée et des relations claires entre tables, il est idéal pour des analyses exploratoires, des prédictions ML, et des insights sur la méta-game du moment.

**Données estimées** : ~1-2 mois de matches European (patch 14.23 à 15.13), soit un snapshot représentatif de la méta couvrant la fin de la saison 14 et le début de la saison 15.
