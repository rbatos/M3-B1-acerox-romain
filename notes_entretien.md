# Notes d'entretien — Sébastien Marchand (Acerox)

> Notes prises au fil de l'eau pendant l'entretien fictif 9h30-10h00.
> **5 catégories à pré-remplir** avant l'entretien. Tu complètes au fil
> de l'eau. Distinction explicite *dit* (ce que Sébastien a effectivement
> formulé) vs *interprété* (ta lecture).
>
> Conservé en livrable du brief.

**Date** : `30/06/2026` — **Durée** : 30 min — **Présents** : Sébastien Marchand (chef de projet industrialisation Acerox) + `Groupe de formation ATOS ^^` (FastIA)

---

## 1. Besoin métier

> Quelle décision Sébastien veut-il améliorer ? Quel KPI métier ?

**Dit** : Sébastien veut améliorer la détection de défaut sur les lignes de production de leur 3 sites, principalement celle de Roubaix qui présente le plus grand nombre de défaut pour la faire baisser de 20% d'ici 3 mois. Il voudrait prédire 24h à l'avance l'apparition de défaut.

**Interprété** : Sébastien veut améliorer la détection de défaut sur les lignes de production de leur 3 sites, principalement celle de Roubaix qui présente le plus grand nombre de défaut pour la faire baisser de 20% d'ici 3 mois. Il voudrait prédire 24h à l'avance l'apparition de défaut.

## 2. Sources et formats

> Qu'a-t-il à disposition ? Où ? Sous quel format ?

**Dit** : il y a 3 sources de log :
- les logs machine le long de la chaine de production (contiennent potentiellement des informations sensible avec les ID des ouvriers en fonction de leur posisiotn sur les machines)
- les informations remontées pas les capteurs le long de la chaine de production sous forme de CSV
- les données de l'ERP

**Interprété** : 3 sources différentes à analyser, auditer... surement à approfondir avec l'équipe data.

## 3. Volumétrie et fréquence

> Combien de données ? À quelle cadence arrivent-elles ?

**Dit** : la seule volumétrie abordée est celle des capteurs : ils remontent une mesure par seconde, donc volumétrie importante d'après Sébastien

**Interprété** : pas assez d'information sur le sujet, il faudra approfondir le point avec l'équipe data.

## 4. Contraintes (RGPD, sécurité, propriété)

> Quelles obligations légales ? Qui possède les données ? Quels accès
> sécurisés ?

**Dit** : RGPD : présence des ID des ouvriers (sous la forme EMP1234) dans les logs machines
Propriété : tout est en local, les données sont souveraines et doivent le rester

**Interprété** : il faudra donc un modèle local, et faire potentiellement de l'anonymisation sur les logs machines qui contiennent les ID des ouvriers, à confirmer avec le DPO.

## 5. Critères de succès

> Comment Sébastien saura-t-il qu'on a réussi ?

**Dit** : le principal critère de réussite est une baisse du taux de non conformité en sortie de ligne (notamment avec une baisse de 20% sur la ligne de Roubaix). Il est préférable d'arrêter la ligne de production pour vérifier une non conformité plutôt que de laisser partie une pièce défectueuse chez le client (ce qui leur coute 12k€)

**Interprété** : Il faudra donc entrainer un nouveau modèle en baseline et améliorer ses prédictions pour faire baisser le taux d'erreur en sortie de ligne.

---

## Questions restées sans réponse

> Notes-le honnêtement — c'est précieux pour la note d'identification.

- le modèle actuel ainsi que son fonctionnement.
- ...

## Mes impressions à chaud (10 min après)

> 1 paragraphe, à toi.

C'est pas facile de prendre des notes, et d'en tirer des interprétations.

---

*Notes d'entretien produites par Romain, 30/06/2026.*
