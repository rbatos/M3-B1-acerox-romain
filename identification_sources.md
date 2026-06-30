# Note d'identification des sources — Acerox Métallurgie

> Document remis à **Sébastien Marchand** (chef de projet industrialisation Acerox). **2-3 pages max.**
> Public : décideur métier non-technique — langage courant, pas de jargon scikit-learn ou SQL.
> Auteur : `Romain` — Date : `30/06/2026`

## 1. Contexte

> 1 paragraphe — qui est Acerox ? quel est leur existant ? quelle est la demande FastIA ?

Acerox est un industriel de la métalurgie (environ 800 salariés) qui produit des pièces techniques depuis 3 sites (Lyon, Roubaix et ?) qui ont chacun une ou pliseurs lignes de production.
Ils ont déjà un modèle de prédiction de défauts qualité, ils veulent l'enrichir avec de nouvelles sources.
C'est la demande émise à FastIA par Sébastien Marchand leur chef de projet industrialisation.

## 2. Demande métier reformulée

> 1 paragraphe — ce que Sébastien veut **vraiment**, distingué de ce qu'il a *dit*. Reformulation en termes de décision métier à améliorer.

Ce que Sébastien a demandé : Il veut une meilleur prédiction de la détection de défaut qualité dans leur chaine de production 24h en amont en observant les signaux envoyés par des logs machines et des capteurs présent le long de la chaine de production, ainsi que des informations récupérées depuis leur ERP.

Ce que je comprends qu'il cherche vraiment : il faut intégrer 3 nouvelles sources de données (détaillées plus bas) à leur modèle existant pour améliorer au minimum de 20% les prédictions de défaut qualité sur les chaines de production pour éviter au maximum l'envoi au client de pièces défectueuses. Il préfère un arrêt temporaire d'une ligne de production pour contrôler la présence d'un potentiel défaut, mais il faut cependant que ça n'arrive pas trop souvent car il perde environ 2H.

## 3. Inventaire des sources

> Une ligne par source que **vous** avez identifiée (fichiers reçus + ce que vous découvrez en explorant les données). Le nom exact du fichier fait partie de l'inventaire à dresser.

| Source | Format | Volume | Fréquence | Qualité observée | Risques RGPD | Pertinence métier |
|---|---|---|---|---|---|---|
| `capteurs_iot.csv` | CSV tabulaire | 51 000 lignes × 7 colonnes (~3,5 Mo) | continue — régulier (~1 mesure/5min), 1 mois échantillonné | Bonne qualité seuls quelques manquements dans "vibration_mms" | Faible (pas d'identifiant nominatif, mais données potentiellement sensibles industriellement) | Très intéressant pour la prédiction de défaut notamment les données vibration_mms, temperature_c, debit_uh qui doivent être des paramètres qui permettent de détecter une potentielle future panne |
| `logs_machines.log` | texte non structuré | 30 000 lignes (~1,9 Mo) | continue (flux machine 1 evènement/min) | Nécessite une transformation en tout cas toutes les lignes ont l'air de respecter le schémas `date`, `site`, `ligne_id`, `type d'événement`, `explication` : donc transformable en csv pour être utilisé plus simplement | Faible (pas d'identifiant nominatif, mais données potentiellement sensibles industriellement) | Intéressante : apporte du contexte opérationnel (arrêts, maintenance, alertes) pour expliquer les défauts |
| `erp_export.json` | JSON (liste d'objets) | 2 000 ordres × 9 colonnes (~540 Ko) | batch quotidien (69 ordres/jour ordres cré en continue dans la journée granularité jour ) | Bonne qualité seuls quelques manquements dans "ouvrier_id" | Importante (présence explicite de `ouvrier_id` de type `EMP-xxxx`) | moyennement intéressante : utile pour relier la qualité aux ordres, produits, statuts et prioriser les actions |

## 4. Recommandations

> 3-5 puces. Quelles sources ingérer en priorité ? Lesquelles écarter et pourquoi ?

- `capteurs_iot.csv` => à prendre en priorité car fournit des données en continue sur l'état des capteurs
- `logs_machines.log` => intéressant car fournit des données en continue sur l'état des capteurs
- `erp_export.json` => source d'enrichissement

## 5. Points à clarifier avec Sébastien

> 3-5 questions ouvertes restantes — preuve de lucidité sur ce qu'on ne sait pas encore.

1. Comment fonctionne le modèle actuel ? Sur quoi base-t-il ses prédictions actuellement ?
2. Quelle est l'architecture actuelle ?
3. ...

## 6. Limites de cette note

> Ce qu'on n'a **pas** fait, et qu'il faudrait faire plus tard.

- Pas d'analyse statistique fouillée des sources (M3-B1 = identification,
  pas EDA complète)
- Pas d'AIPD juridique formelle (recommandation : escalader au DPO Acerox)
- ...

---

*Note produite par Romain, 30/06/2026, dans le cadre du brief M3-B1 ATOS.*
