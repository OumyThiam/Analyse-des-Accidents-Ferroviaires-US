# Analyse des Accidents Ferroviaires US : Dashboard Power BI & Machine Learning Dataiku

Ce projet est né d'une question simple. Comment 47 ans d'accidents ferroviaires aux États-Unis peuvent-ils raconter une histoire utile aux décideurs ?
L’enjeu consistait à transformer un jeu de données brut issu de la Federal Railroad Administration (FRA). Ce jeu contient environ 38 000 lignes et de nombreuses colonnes hétérogènes. L’objectif était d’en faire un véritable outil de diagnostic stratégique.
La solution mise en place repose sur deux niveaux complémentaires. Un dashboard Power BI permet le pilotage opérationnel et l’exploration des données. En parallèle, un pipeline de machine learning sous Dataiku est utilisé pour la prédiction et l’anticipation des risques.
L’objectif du projet est double. Il s’agit d’abord de comprendre les dynamiques temporelles et géographiques des accidents ferroviaires. Ensuite, l’analyse va plus loin en cherchant à prédire automatiquement le type d’accident et le moment d’occurrence à partir des caractéristiques disponibles.

---


## Partie 1 — Dashboard Power BI

#### 1. Nettoyage Python 
J’ai commencé par préparer et fiabiliser les données afin de garantir la qualité des analyses.
- **Filtrage des colonnes vides** : suppression de toutes les colonnes
  dont le taux de valeurs manquantes dépasse 5% du jeu de données.
- **Suppression des doublons** et des lignes sans `Report Year`,
  garantissant l'intégrité de l'axe temporel.
- **Nettoyage des caractères parasites** : élimination des caractères
  de contrôle (`\x0b`) présents dans certains champs texte bruts.
- **Correction des colonnes monétaires** : les champs
  `Equipment Damage Cost`, `Track Damage Cost` et `Total Damage Cost`
  contenaient des virgules comme séparateurs de milliers, empêchant
  toute conversion numérique. Ces virgules ont été supprimées avant
  conversion en `float`.
- **Typage strict des colonnes numériques** : conversion en `Int64`
  (compatible valeurs manquantes) pour 12 colonnes clés `Report Year`,
  `Train Speed`, `Temperature`, `Track Class`, `Gross Tonnage`, etc.

#### 2. Mesures DAX et Modélisation
## Indicateurs clés
Après le nettoyage des données, j’ai construit plusieurs mesures dans Power BI afin d’identifier les indicateurs clés et faciliter l’analyse.
### Nombre total d’accidents
Cette mesure représente le nombre total d’accidents enregistrés dans le jeu de données sur la période étudiée.  
Elle permet d’avoir une vision globale de l’ampleur des incidents ferroviaires et sert de référence pour l’ensemble de l’analyse.

### Type d’accident dominant
Cette mesure identifie le type d’accident le plus fréquent.  
Elle met en évidence la cause principale des incidents, ce qui permet de mieux comprendre les risques majeurs et d’orienter les actions de prévention.

### État le plus touché
Cette mesure permet de déterminer l’État ayant enregistré le plus grand nombre d’accidents.  
Elle met en lumière les zones géographiques les plus exposées et facilite l’identification des régions prioritaires en matière de sécurité.

### Station la plus touchée
Cette mesure met en évidence la station qui concentre le plus grand nombre d’accidents.  
Elle permet d’identifier un point critique du réseau ferroviaire nécessitant une attention particulière et une analyse approfondie.

### Moment le plus fréquent
Cette mesure détermine le moment de la journée durant lequel les accidents surviennent le plus fréquemment.  
Elle permet d’analyser l’influence des conditions temporelles, comme la visibilité ou l’intensité du trafic, sur la survenue des incidents.


#### 3. Design et Navigation
J’ai porté une attention particulière à la conception du dashboard afin de garantir une lecture claire et une navigation intuitive.
- **Thème sombre cohérent** : palette vert menthe sur fond anthracite, conçue pour maximiser la lisibilité des KPI et la hiérarchie visuelle.
- **3 pages thématiques** avec navigation fluide : Vue générale, Analyse géographique & économique, Conditions de visibilité.
- **Filtres croisés dynamiques** : sélecteur de période (curseur 1975–2022), filtre par État (dropdown), segmentation par moment de la journée (boutons).
- **Carte choroplèthe interactive** pour une lecture géographique immédiate de la densité des accidents par État.

---

### Aperçu visuel

#### Vue d'ensemble : Pilotage de la sinistralité

J’ai conçu cette page comme une vue globale couvrant l’ensemble de la période 1975–2022.  
Elle regroupe les principaux indicateurs, une courbe d’évolution annuelle et un graphique en barres permettant d’analyser les types d’accidents.

La courbe temporelle joue un rôle central dans l’analyse.  
Elle met en évidence une phase relativement stable avant 1990, probablement liée à des limites dans les données disponibles.  
Un pic très marqué apparaît ensuite entre 1999 et 2001, avec près de 3 800 accidents par an.  
À partir de 2002, une baisse significative s’amorce, puis la situation se stabilise autour de 1 500 accidents par an après 2010.  
Cette stabilisation soulève une question importante sur les freins actuels à une réduction supplémentaire du nombre d’accidents.

Le graphique des types d’accidents met en lumière une tendance structurante.  
Le déraillement représente à lui seul plus de 55 % des cas, ce qui le place largement devant les autres catégories.  
Ce poids très important oriente naturellement l’analyse vers les enjeux de maintenance des voies et de surveillance des infrastructures.

Les filtres interactifs permettent d’explorer les données de manière dynamique.  
Ils offrent la possibilité de sélectionner une période, un État ou un moment de la journée afin d’observer immédiatement l’impact sur l’ensemble des indicateurs.

<img width="1878" height="1302" alt="Capture d’écran_7-5-2026_11313_" src="https://github.com/user-attachments/assets/00da96b3-2929-4c61-ae2f-73ef813e08b7" />

---

#### Analyse géographique et économique
Cette page a pour objectif de cartographier la sinistralité et d’évaluer son impact financier à l’échelle des États.
J’y combine plusieurs angles d’analyse afin de proposer une lecture complète et complémentaire des données.

La carte choroplèthe permet d’obtenir une vision géographique immédiate.  
On'y observe une concentration plus marquée des accidents dans les États de la ceinture centrale comme l’Illinois, le Texas ou le Nebraska.  
Cette répartition met en évidence l’importance des grands axes de transport de marchandises dans la distribution du risque.

Le graphique des coûts par État met en lumière des écarts significatifs.  
Le Texas se distingue nettement avec un niveau de dommages particulièrement élevé, proche de 419 millions de dollars.  
Ce montant est presque deux fois supérieur à celui observé en Californie.  
Cette situation s’explique par la combinaison d’un réseau ferroviaire dense, d’un trafic industriel soutenu et d’accidents à fort impact économique.

Le tableau de détail complète cette analyse en offrant une vision plus fine.  
Il permet d’examiner chaque État individuellement en croisant le nombre d’accidents avec le coût total des dommages.  
Sa structure facilite l’exploration approfondie grâce aux possibilités de tri et de filtrage.

L’analyse est enrichie par la prise en compte du moment de la journée.  
J’observe que la soirée concentre systématiquement le plus grand nombre d’accidents.  
Cette tendance reste stable, quel que soit l’État ou la période étudiée.

<img width="1875" height="1420" alt="Capture d’écran_7-5-2026_11349_" src="https://github.com/user-attachments/assets/4ef3a744-24f7-480d-8ece-aac876d593df" />

-----

#### Conditions de visibilité et facteurs de risque
Cette information est affichée en complément du graphique des types d’accidents, lors du survol d’une catégorie spécifique comme *Side Collision*.  
Elle permet d’apporter un éclairage supplémentaire sur les conditions de visibilité associées à ce type d’incident.
La visualisation ci-dessous montre la répartition des accidents selon la visibilité.

<img width="1160" height="461" alt="image" src="https://github.com/user-attachments/assets/d8f08c74-6cd1-42d1-a6f5-3d9f07dcdb76" />

On observe que, même pour les collisions latérales, la majorité des accidents survient en journée (`Day`).  
Les périodes de faible visibilité comme la nuit (`Dark`), le crépuscule (`Dusk`) ou l’aube (`Dawn`) restent moins représentées.

Ce résultat peut sembler contre-intuitif, car une visibilité réduite est souvent perçue comme un facteur de risque.  
Cependant, il s’explique principalement par le niveau d’activité.  
Le trafic ferroviaire étant plus important en journée, les accidents y sont mécaniquement plus nombreux, quel que soit le type d’incident étudié.

Cette information permet donc de compléter la lecture du graphique principal.  
Elle montre que la fréquence d’un type d’accident ne dépend pas uniquement des conditions de visibilité, mais aussi du niveau d’exposition au risque.

Enfin, cette observation reste cohérente avec une tendance globale.  
Le déraillement demeure le type d’accident dominant, indépendamment des conditions de visibilité, ce qui confirme son caractère structurel.


---

## Partie 2 — Pipeline Machine Learning (Dataiku)

### Ce que j'ai réalisé

Là où le dashboard Power BI répond à la question *"que s'est-il passé ?"*, le pipeline Dataiku répond à *"que pourrait-il se passer ?"*. J'ai construit un flow de bout en bout allant de la donnée brute à deux modèles de classification entraînés et évaluables.

#### Pipeline Dataiku (Vue du flow)

<img width="1275" height="397" alt="Capture d’écran 2026-05-09 120017" src="https://github.com/user-attachments/assets/4dc6146a-2a61-46ec-9a49-45c07e85d2dc" />


#### 1. Nettoyage Python (`df_clean`)

Le premier script Python prend en charge le nettoyage profond des données brutes :

- Suppression des colonnes à taux de valeurs manquantes trop élevé.
- Standardisation des formats texte (`State Name`, `Accident Type`) pour garantir la cohérence des encodages en aval.
- Traitement des outliers sur les variables numériques de coût (`Track Damage Cost`, `Equipment Damage Cost`).
- Filtrage des enregistrements incohérents (dates hors plage, coordonnées invalides).

#### 2. Préparation visuelle (`df_clean_prepared`)

L'étape **Prepare** de Dataiku a permis d'appliquer des transformations sans code directement sur l'interface :

- Encodage des variables catégorielles (`Visibility`, `TimeOfDay`, `State Name`).
- Normalisation des variables numériques pour les modèles sensibles à l'échelle.
- Création de nouvelles colonnes calculées (décennie, tranche horaire simplifiée).

#### 3. Feature Engineering Python (`time_class`)

Un second script Python construit les features les plus discriminantes pour les deux tâches de classification :

- Extraction de variables temporelles enrichies depuis `Report Year`.
- Construction d'un indicateur de densité d'accidents par État et par période.
- Recodage de la variable cible `Accident Type` en classes consolidées pour réduire le déséquilibre de classes (le déraillement représentant 55% des cas, un traitement spécifique était nécessaire).

#### 4. Modèles de prédiction (multiclass)

J’ai développé deux modèles de classification multiclasse afin d’anticiper les accidents ferroviaires à partir des caractéristiques disponibles dans les données.
L’objectif est double. Il s’agit d’identifier le type d’accident probable et d’analyser les facteurs qui influencent ces événements.

---

##### Analyse des prédictions

<img width="735" height="687" alt="prediction-cause" src="https://github.com/user-attachments/assets/bf09f105-5011-4e18-a9ad-3728ba9d5706" />

Ce tableau présente un extrait des prédictions du modèle.  
Chaque ligne compare la classe réelle (`actual_class_id`) à la classe prédite (`predicted_class_id`) et indique si la prédiction est correcte.

On observe que la majorité des prédictions sont correctes, notamment pour des classes dominantes comme *Derailment*.  
Cependant, certaines erreurs apparaissent, par exemple lorsque un cas de collision est mal classé, ce qui montre les limites du modèle face à des situations proches.

---

##### Performance du modèle

<img width="1322" height="428" alt="Capture d’écran 2026-05-07 121548" src="https://github.com/user-attachments/assets/628a0880-517d-45ea-9b09-243d3c2beefb" />


Les performances globales du modèle sont élevées.  
L’accuracy atteint 0.947, indiquant que le modèle prédit correctement la grande majorité des cas.

Le score F1 et le ROC AUC montrent que le modèle est capable de bien distinguer les différentes classes, même si les performances sont moins homogènes sur les classes rares.

---

##### Importance des variables

<img width="1336" height="743" alt="Capture d’écran 2026-05-07 121441" src="https://github.com/user-attachments/assets/56c6d271-ae63-4932-b255-92cb2ebc9084" />

L’analyse des variables importantes montre que certaines caractéristiques jouent un rôle clé dans la prédiction.

Le code du type d’accident et les causes associées sont les variables les plus influentes.  
D’autres variables comme la vitesse du train, l’année ou le type de voie ont un impact plus modéré.

Ces résultats permettent de mieux comprendre les facteurs qui influencent les accidents.

---

##### Optimisation du modèle

<img width="1472" height="452" alt="Capture d’écran 2026-05-07 121711" src="https://github.com/user-attachments/assets/d7cb8e50-0c90-4224-b6d8-6e40713f96f4" />


Une optimisation des hyperparamètres a été réalisée pour améliorer les performances du modèle.

Le graphique montre que la performance augmente progressivement jusqu’à atteindre un équilibre optimal autour d’une certaine valeur.  
Au-delà, les gains deviennent limités alors que le temps de calcul augmente.

---

##### Interprétation globale

Les résultats montrent que le modèle est performant et capable de capturer les tendances principales des accidents ferroviaires.

Toutefois, le déséquilibre des classes reste un défi important.  
Les classes les plus fréquentes sont mieux prédites, tandis que les classes rares restent plus difficiles à identifier.

Ce modèle constitue une première étape vers une approche prédictive permettant d’anticiper les risques et d’améliorer la prise de décision.


---

## Stack technique

| Outil | Usage |
|-------|-------|
| Power BI Desktop | Modélisation, DAX, visualisations interactives |
| Power Query (M) | Nettoyage, transformation, structuration |
| DAX | Mesures calculées, KPI, filtres dynamiques |
| Dataiku DSS | Orchestration du pipeline ML, AutoML |
| Python (Dataiku) | Nettoyage avancé, feature engineering |
| Scikit-learn (via Dataiku) | Modèles de classification multiclasse |
| CSV (FRA) | Source de données brutes |

---

## Données

- **Source** : Federal Railroad Administration (FRA) — données publiques américaines
- **Période** : Décembre 1975 → Octobre 2022
- **Volume** : 38 312 accidents enregistrés
- **Colonnes clés** : `Report Year`, `State Name`, `Accident Type`, `TimeOfDay`, `Visibility`, `Track Damage Cost`

---

## Auteur

**Oumy THIAM**  
