# Schéma des flux de données — Acerox Métallurgie

> Schéma Mermaid à compléter. Doit montrer :
> - **Sources** (capteurs IoT, ERP, logs, *bonus PDF*)
> - **Ingestion** (à concevoir en M3-B2)
> - **BDD pivot** (à modéliser en M3-B2)
> - **Modèle existant** Acerox (placeholder, hors-sujet ici)
>
> Légende explicite : qui produit, qui consomme, contraintes.

## Schéma

```mermaid
flowchart LR
    %% Sources
    SRC1["📡 capteurs_iot.csv<br>CSV tabulaire<br>51 000 × 7 colonnes (~3,5 Mo) / mois<br>Continue (~1 mesure / 5 min)"]
    SRC2["📋 erp_export.json<br>JSON (liste d'objets)<br>2k ordres × 9 colonnes (~540 Ko) / mois<br>Batch quotidien"]
    SRC3["📝 logs_machines.log<br>Texte non structuré<br>30k lignes (~1,9 Mo) / mois<br>Continue (1 evènement/min)"]

    %% Ingestion
    INGEST[🔄 Ingestion<br/>pandas + SQLAlchemy]

    %% Stockage
    BDD[(🗄️ BDD pivot<br/>SQLite)]

    %% Modèle existant
    MODEL[🧠 Modèle prédiction défauts<br/>Acerox — existant]

    %% Liens
    SRC1 -->|temps réel| INGEST
    SRC2 -->|batch journalier| INGEST
    SRC3 -.->|Transformation CSV : les 4 premiers espaces remplacés par des virgules| INGEST
    INGEST -->|normalisation| BDD
    BDD -->|consommée par| MODEL

    %% Styles
    classDef source fill:#e1f5ff,stroke:#0277bd
    classDef modele fill:#fff4e1,stroke:#c97a00
    classDef B2 fill:#d4f4dd,stroke:#1a7a3a
    class SRC1,SRC2,SRC3 source
    class BDD,INGEST B2
    class MODEL, modele
```

## Légende

> Reformule en 5 lignes max ce que le schéma raconte (qui produit quelle
> donnée, qui consomme, contraintes critiques).

- **Producteur** : Capteurs IoT (temps réel), ERP (batch quotidien), logs machines (texte non structuré)
- **Consommateur final** : Modèle prédictif Acerox (consomme la BDD normalisée)
- **Contraintes critiques** (fréquence / RGPD / qualité) : Fréquence élevée (1 mesure/5 min à 1 événement/min), qualité des données (transformation des logs), potentiel RGPD sur les logs machines

## Décisions associées

- Source(s) retenues en priorité : `capteurs_iot.csv`
- Source(s) écartées : aucune
- Source bonus (PDF) traitée ? oui / non, pourquoi : ?

---

*Schéma produit par Romain, 30/06/2026, dans le cadre du brief M3-B1 ATOS.*
