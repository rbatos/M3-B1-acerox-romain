# Note d'identification des sources — Acerox Métallurgie

> Document remis à **Sébastien Marchand** (chef de projet industrialisation
> Acerox). **2-3 pages max.** Public : décideur métier non-technique —
> langage courant, pas de jargon scikit-learn ou SQL.
> Auteur : `Romain` — Date : `30/06/2026`

## 1. Contexte

> 1 paragraphe — qui est Acerox ? quel est leur existant ? quelle est la
> demande FastIA ?

Acerox est un industriel de la métalurgie qui produit des pièces techniques depuis 3 lignes de production situées sur 3 site (Lyon, Roubaix et ?).
Ils ont déjà un modèle de prédiction de défauts qualité, ils veulent l'enrichir avec de nouvelles sources.

## 2. Demande métier reformulée

> 1 paragraphe — ce que Sébastien veut **vraiment**, distingué de ce qu'il
> a *dit*. Reformulation en termes de décision métier à améliorer.

Ce que Sébastien a demandé : Il veut une meilleur prédiction de la détection de défaut qualité dans leur chaine de production 24h en amont en observant les signaux envoyés par des logs machines et des capteurs présent le long de la chaine de production, ainsi que des informations récupérées depuis leur ERP.

Ce que je comprends qu'il cherche vraiment : ce que j'ai dit au dessus... j'ai je pense déjà interprété ce qu'il a dit pour reformuler ça demande selon ce que j'en ai compris.

## 3. Inventaire des sources

> Une ligne par source que **vous** avez identifiée (fichiers reçus +
> ce que vous découvrez en explorant les données). Le nom exact du fichier
> fait partie de l'inventaire à dresser.

| Source | Format | Volume | Fréquence | Qualité observée | Risques RGPD | Pertinence métier |
|---|---|---|---|---|---|---|
| `capteurs_iot.csv` | CSV tabulaire | 51 000 lignes (~3,7 Mo) | Mesure horodatée fine (cohérente avec un rythme proche de la seconde) | Schéma clair et stable (`timestamp`, `site`, `line_id`, `sensor_id`, `temperature_c`, `vibration_mms`, `debit_uh`), pas d'anomalie évidente en tête de fichier | Faible à modéré (pas d'identifiant nominatif direct, mais données potentiellement sensibles industriellement) | Très élevée : source principale pour détecter les dérives techniques et anticiper un défaut |
| `logs_machines.log` | Log texte semi-structuré | 30 000 lignes (~1,9 Mo) | Événementiel (arrivées irrégulières selon incidents et activité de ligne) | Données exploitables mais nécessitent un parsing; présence de niveaux (`INFO`, `WARN`, `ERROR`) et de signaux utiles (`vibration_overlimit`, `emergency_stop`) | Modéré à élevé (événements opérateur type `operator_login`, possibilité de recoupement avec des personnes) | Élevée : apporte le contexte opérationnel (arrêts, maintenance, alertes) pour expliquer les défauts |
| `erp_export.json` | JSON (liste d'objets) | 2 000 enregistrements (~0,56 Mo) | Plutôt batch/transactionnel (ordres de fabrication) | Structure homogène; champs métier utiles (`ordre_id`, `produit_ref`, `statut`, `quantite_kg`, dates), qualité lisible | Élevé (présence explicite de `ouvrier_id` de type `EMP-xxxx`) | Élevée : utile pour relier la qualité aux ordres, produits, statuts et prioriser les actions |

## 4. Recommandations

> 3-5 puces. Quelles sources ingérer en priorité ? Lesquelles écarter et
> pourquoi ?

- ...
- ...
- ...

## 5. Points à clarifier avec Sébastien

> 3-5 questions ouvertes restantes — preuve de lucidité sur ce qu'on ne
> sait pas encore.

1. ...
2. ...
3. ...

## 6. Limites de cette note

> Ce qu'on n'a **pas** fait, et qu'il faudrait faire plus tard.

- Pas d'analyse statistique fouillée des sources (M3-B1 = identification,
  pas EDA complète)
- Pas d'AIPD juridique formelle (recommandation : escalader au DPO Acerox)
- ...

---

*Note produite par <prénom>, <date>, dans le cadre du brief M3-B1 ATOS.*
