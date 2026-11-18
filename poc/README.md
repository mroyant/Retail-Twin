# POC

> Pas encore de code. Ce dossier fixe la structure et le périmètre du POC pour que le développement puisse démarrer à deux sans se marcher dessus.

## Objectif

Démontrer les trois cas d'usage P0 ([docs/02-cas-usage.md](../docs/02-cas-usage.md)) sur de la vidéo enregistrée :

- UC-01 : ouverture de caisse anticipée (file, attente estimée) ;
- UC-02 : staffing des comptoirs (abandons au comptoir) ;
- UC-03 : créneaux de remplissage (occupation des allées par créneau).

## Structure prévue

```
poc/
├── configs/      # Un YAML par magasin : plan, caméras + calibration, zones, lignes
├── data/         # Vidéos et sorties : IGNORÉ PAR GIT, ne jamais committer de vidéo
├── notebooks/    # Exploration et visualisations de démo
├── src/          # Code du pipeline (voir découpage ci-dessous)
└── tests/        # Tests unitaires de la logique géométrie / indicateurs
```

## Découpage prévu du pipeline

Voir [docs/03-architecture.md](../docs/03-architecture.md).

| Module | Rôle | Dépend de la vidéo ? |
|---|---|---|
| `config` | Lecture du YAML magasin | Non |
| `geometry` | Homographie image → plan, point dans polygone, franchissement de ligne | Non |
| `tracking` | Vidéo → pistes (YOLO + ByteTrack), projection des pieds sur le plan | **Oui** |
| `analytics` | Visites, dwell, occupation, abandons, attente (loi de Little), heatmap | Non |
| `simulate` | Pistes synthétiques pour développer sans vidéo | Non |

**Principe :** tout ce qui est en aval du tracking travaille sur des pistes `(track_id, t, x, y)` en mètres. Les deux personnes peuvent donc avancer en parallèle : l'une sur la vision (`tracking`), l'autre sur les indicateurs (`analytics`) à partir de pistes simulées.

## Choix techniques envisagés (à confirmer par ADR)

- Python 3.11+
- Détection et tracking : Ultralytics YOLO + ByteTrack
- Lecture vidéo : OpenCV
- Tests : pytest ; lint : ruff

## Configuration d'exemple

[configs/magasin_demo.yaml](configs/magasin_demo.yaml) décrit un magasin fictif (caisses, comptoir fromage, tête de gondole, allées). Il sert de référence pour le format des zones.

## Données de test

Voir [docs/05-rgpd-conformite.md](../docs/05-rgpd-conformite.md#pour-les-données-de-test-du-poc) : datasets publics avec une licence compatible, ou vidéos tournées avec consentement. Jamais de vidéo client sur un poste perso.
