# 06 — Roadmap

## Phase 0 — Cadrage (en cours)

- [x] Structure du repo, docs initiales, cas d'usage priorisés
- [ ] Choisir 1 ou 2 vidéos de test (dataset public ou tournage consenti)
- [ ] Valider la priorisation des cas d'usage avec un ou deux interlocuteurs terrain (directeur de magasin, responsable caisses)

## Phase 1 — POC technique

Objectif : démontrer les **trois cas P0** sur vidéo enregistrée.

- [ ] Tracking de personnes sur la vidéo de test, export des pistes
- [ ] Calibration d'une caméra (homographie) et projection sur un plan
- [ ] Zones configurées : file de caisse, comptoir (approche + service), allées
- [ ] Indicateurs : occupation par créneau, dwell, abandons, attente (Little), heatmap
- [ ] Mesure de la qualité : comptage manuel sur 10 min de vidéo face au comptage automatique (erreur en %)
- [ ] Notebook de démo avec les visualisations

**Critère de sortie :** erreur de comptage < 10 % sur la zone de file **(cible à confirmer)**, avec une démo jouable de bout en bout.

## Phase 2 — Démo produit

- [ ] Dashboard simple (Streamlit ou Grafana) : plan + heatmap + courbes par zone
- [ ] Simulation temps réel : lecture de la vidéo, alerte d'ouverture de caisse (UC-01)
- [ ] Rapport « tête de gondole » type (UC-05) pour la présentation
- [ ] Support de présentation finalisé ([presentation/](../presentation/))

## Phase 3 — Pilote magasin

- [ ] AIPD et conformité ([05-rgpd-conformite.md](05-rgpd-conformite.md))
- [ ] Inventaire caméras et accès aux flux
- [ ] Déploiement edge sur 3 à 5 caméras (entrée, caisses, un comptoir)
- [ ] Mesure avant/après sur 4 semaines des KPI de UC-01 et UC-02
