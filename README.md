# Analyse des Accidents Ferroviaires US : Dashboard Power BI

Ce projet est né d'une question simple : **comment 47 ans d'accidents ferroviaires aux États-Unis peuvent-ils raconter une histoire utile aux décideurs ?** L'enjeu était de transformer un jeu de données brutes de la Federal Railroad Administration (FRA 38 000 lignes), des dizaines de colonnes hétérogènes — en un outil de diagnostic stratégique clair, interactif et exploitable.

L'objectif est double : **comprendre les dynamiques temporelles et géographiques de la sinistralité ferroviaire**, et **identifier les leviers prioritaires de prévention** à travers l'analyse croisée des types d'accidents, des conditions d'occurrence et des coûts de dommages.

---

## Ce que j'ai réalisé

### 1. Nettoyage et Structuration (Power Query)

- **Audit de données** : identification des valeurs manquantes, des colonnes redondantes et des incohérences de saisie sur les champs `State Name`, `Visibility` et `TimeOfDay`.
- **Normalisation** : structuration du modèle autour d'une table de faits centrale (accidents) reliée à des dimensions claires (États, Types d'accidents, Conditions de visibilité, Périodes).
- **Traitement des anomalies temporelles** : la série pré-1990 affichait une flatline révélatrice d'une non-numérisation des archives historiques. Ce biais a été documenté et intégré dans les annotations du rapport.
- **Renommage des colonnes** pour un vocabulaire métier francisé et lisible directement dans les visuels.

### 2. Mesures DAX et Modélisation

- **Mesures de volume** : total accidents, fréquence par État, distribution par moment de la journée.
- **Mesures financières** : somme des coûts de dommages (`Track Damage Cost`), coût moyen par accident, agrégats par État et par période.
- **Mesures filtrées** : calculs isolés par condition de visibilité (`Visibility = "Dark"`, `"Day"`, etc.) et par type d'accident (`Accident Type = "Derailment"`) pour des analyses segmentées sans dupliquer les visuels.
- **Classements dynamiques** : TOP 10 des États par coût de dommages, recalculé automatiquement selon les filtres actifs.

### 3. Design et Navigation

- **Thème sombre cohérent** : palette vert menthe sur fond anthracite, conçue pour maximiser la lisibilité des KPI et la hiérarchie visuelle.
- **3 pages thématiques** avec navigation fluide : Vue générale, Analyse géographique & économique, Conditions de visibilité.
- **Filtres croisés dynamiques** : sélecteur de période (curseur 1975–2022), filtre par État (dropdown), segmentation par moment de la journée (boutons).
- **Carte choroplèthe interactive** pour une lecture géographique immédiate de la densité des accidents par État.

---

## Aperçu visuel

### Vue d'ensemble : Pilotage de la sinistralité

Cette page offre un tableau de bord macro sur la totalité de la période 1975–2022. Elle centralise quatre indicateurs clés — total accidents (38K), type dominant (Derailment), État le plus touché (Illinois) et moment le plus accidentogène (Soirée) — complétés par une **courbe d'évolution temporelle annuelle** et un **graphique à barres des types d'accidents**.

La courbe temporelle est l'élément narratif central du rapport : elle révèle une flatline pré-1990 (biais d'archivage), un pic dramatique autour de 1999–2001 (~3 800 accidents/an), puis une décrue significative après 2002 qui se stabilise autour de 1 500 accidents/an. Ce plateau post-2010 devient lui-même un sujet d'analyse : qu'est-ce qui empêche de descendre en dessous de ce seuil ?

Le graphique des types d'accidents illustre une réalité saisissante : le **déraillement représente à lui seul plus de 55% des cas**, loin devant toutes les autres catégories. Cette domination écrasante oriente naturellement les recommandations de prévention vers la maintenance des voies et la surveillance des infrastructures.

> *Les filtres dynamiques (période, État, moment de la journée) permettent d'isoler n'importe quel sous-ensemble de données et de faire réagir l'ensemble des visuels en temps réel.*

---

### Focus : Analyse géographique et économique

Cette page a pour objectif de **cartographier la sinistralité et de quantifier son impact financier par État**. Trois angles d'analyse se complètent :

La **carte choroplèthe** offre une lecture géographique immédiate : les États de la ceinture centrale (Illinois, Texas, Nebraska) apparaissent nettement plus foncés, trahissant une concentration du risque sur les grands axes de fret.

Le **graphique des coûts par État** (Top 10) révèle une hiérarchie surprenante : le Texas arrive en tête avec **419 M$ de dommages**, soit presque le double de la Californie (249 M$). Ce résultat s'explique par la conjonction d'un réseau ferroviaire très dense, d'un trafic industriel intense (pétrole, agriculture) et d'accidents d'une gravité particulièrement élevée.

Le **tableau de détail** permet une lecture exhaustive État par État, avec le nombre d'accidents et le coût total des dommages, triable et filtrable pour un usage analytique approfondi.

> *La distribution par moment de la journée complète cette page en montrant que la soirée concentre systématiquement le plus grand volume d'accidents, indépendamment de l'État ou de la période considérée.*

---

### Focus : Conditions de visibilité et facteurs de risque

Cette page explore une dimension moins évidente mais analytiquement riche : **les conditions lumineuses au moment de l'accident**. Le résultat principal est contre-intuitif — la grande majorité des accidents survient de jour (`Day`), bien avant la nuit (`Dark`).

Ce paradoxe apparent s'explique par un **biais d'exposition** : l'essentiel du trafic ferroviaire (fret et voyageurs confondus) opère en journée, ce qui mécaniquement concentre les accidents sur cette plage horaire. La vraie question analytique n'est pas le volume absolu, mais le **taux d'accident ramené à l'exposition au risque** — une mesure que le rapport invite à construire en complément.

Les conditions `Dawn` (aube) et `Dusk` (crépuscule) représentent un volume faible mais un risque relatif potentiellement sous-estimé, lié aux transitions lumineuses difficiles pour les systèmes de signalisation et la perception des conducteurs.

> *Cette page croise naturellement avec le graphique des types d'accidents : le déraillement reste dominant quelle que soit la condition de visibilité, ce qui confirme son caractère structurel plutôt que conjoncturel.*

---

## Stack technique

| Outil | Usage |
|-------|-------|
| Power BI Desktop | Modélisation, DAX, visualisations |
| Power Query (M) | Nettoyage, transformation, structuration |
| DAX | Mesures calculées, KPI, filtres dynamiques |
| CSV (FRA) | Source de données brutes |

---

## Données

- **Source** : Federal Railroad Administration (FRA) — données publiques américaines
- **Période** : Décembre 1975 → Octobre 2022
- **Volume** : 38 312 accidents enregistrés
- **Colonnes clés** : `Report Year`, `State Name`, `Accident Type`, `TimeOfDay`, `Visibility`, `Track Damage Cost`

> ⚠️ Les données pré-1990 présentent une forte sous-représentation liée à la numérisation partielle des archives historiques. Cette limite est documentée dans le rapport et prise en compte dans l'interprétation des tendances longues.

---

## Auteur

**Oumy THIAM**  
