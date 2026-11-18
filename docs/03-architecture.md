# 03 — Architecture

## Vue d'ensemble

```
┌──────────────────────── MAGASIN (edge) ─────────────────────────┐
│                                                                  │
│  Caméras IP ──RTSP──► Ingestion ──► Détection ──► Tracking       │
│  (existantes)        (décodage,     (personnes,   (ByteTrack,    │
│                       5-10 fps)      objets)       id éphémère)  │
│                                                       │          │
│                                                       ▼          │
│                                          Projection plan          │
│                                          (homographie / caméra)   │
│                                                       │          │
│                                                       ▼          │
│                       Moteur de zones ──► Événements & agrégats   │
│                       (entrée/sortie,     (visites, occupation,   │
│                        dwell, lignes)      heatmap par 1-15 min)  │
│                                                       │          │
│   ✗ aucune image ne sort de cette boîte               │          │
└───────────────────────────────────────────────────────┼──────────┘
                                                        │ données agrégées
                                                        ▼
┌──────────────────────── PLATEFORME ─────────────────────────────┐
│  Stockage séries temporelles ──► Règles d'alerte ──► Notifications│
│                │                                                 │
│                └──► Prévision (staffing) ──► Dashboard / API     │
│                                                                  │
│  + données externes : tickets agrégés, planning, calendrier      │
└──────────────────────────────────────────────────────────────────┘
```

## Étapes du pipeline

| Étape | Rôle | Choix POC | Alternatives à évaluer |
|---|---|---|---|
| Ingestion | Lire un flux RTSP ou un fichier, sous-échantillonner | OpenCV | GStreamer, NVIDIA DeepStream |
| Détection | Boîtes englobantes des personnes | YOLO (Ultralytics), classe `person` | RT-DETR ; modèle *fine-tuné* sur vues plongeantes |
| Tracking | Associer les détections d'une frame à l'autre | ByteTrack (intégré à Ultralytics) | BoT-SORT, OC-SORT |
| Projection | Point bas-centre de la boîte (les pieds) → coordonnées plan (m) | Homographie à partir de 4 points ou plus | Calibration complète avec distorsion (objectifs fish-eye) |
| Zones | Point dans polygone, franchissement de ligne | Python pur | Shapely / PostGIS si beaucoup de zones |
| Agrégation | Visites, dwell, occupation par créneau | Python pur, sortie CSV | Flink / streaming en production |
| Stockage | Séries temporelles | CSV / Parquet | TimescaleDB, ClickHouse |
| Restitution | Dashboard, alertes | Notebook | Grafana, Streamlit, app mobile |

## Choix structurants

1. **Tout ce qui touche à l'image reste en magasin (edge).** Seules des coordonnées et des agrégats sortent. Voir [ADR-0001](adr/0001-traitement-edge-sans-biometrie.md).
2. **Le plan du magasin est le référentiel commun.** Toutes les caméras projettent dans le même repère en mètres, ce qui permet de fusionner des caméras qui se chevauchent et de définir les zones une seule fois.
3. **Les zones sont de la configuration, pas du code.** Un fichier YAML par magasin ([poc/configs/](../poc/configs/)).
4. **Pas de ML là où une règle suffit.** Les alertes v1 sont des seuils explicables ; le ML intervient pour la prévision (UC-04) et le tracking.

## Points techniques durs (à dérisquer tôt)

| Risque | Impact | Piste |
|---|---|---|
| Caméras de sécurité orientées vers les portes ou les caisses, pas vers les allées | Couverture partielle | Inventaire de couverture dès le pilote ; accepter des cas d'usage par caméra |
| Vue plongeante ou fish-eye : les détecteurs génériques sont moins bons | Comptages faux | Fine-tuning sur des vues de magasin ; dé-distorsion |
| Occlusions dans les files et les zones denses | Sous-comptage | Caméra en plongée sur les caisses ; estimation par densité plutôt que par piste |
| Accès aux flux (NVR propriétaire, réseau isolé) | Bloquant | Valider l'accès RTSP au premier rendez-vous technique |
| Puissance de calcul en magasin | Coût | Mesurer les fps par caméra sur GPU edge (Jetson, petit GPU) ; échantillonner à 5 fps |
| Fusion multi-caméras (une personne vue par 2 caméras) | Double comptage | Dédoublonnage par proximité sur le plan ; zones attribuées à une seule caméra en v1 |

## Correspondance avec le POC

Le découpage en modules prévu est décrit dans [poc/README.md](../poc/README.md#découpage-prévu-du-pipeline).
