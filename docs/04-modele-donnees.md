# 04 — Modèle de données

## Principe

On distingue trois niveaux de données. Plus on monte, plus la donnée est agrégée, plus elle vit longtemps et plus elle s'éloigne du magasin.

| Niveau | Contenu | Où | Rétention cible |
|---|---|---|---|
| **N0 — Image** | Frames vidéo | Magasin, en mémoire seulement | 0 (jamais écrite par Retail-Twin) |
| **N1 — Pistes** | Positions plan (x, y) par piste éphémère | Magasin | Courte (heures à quelques jours, à fixer avec le DPO) |
| **N2 — Événements** | Visites de zone, franchissements de ligne | Magasin → plateforme | Moyenne |
| **N3 — Agrégats** | Comptes par zone et par créneau, heatmaps | Plateforme | Longue (historique pour la prévision) |

## N1 — `TrackPoint`

| Champ | Type | Description |
|---|---|---|
| `track_id` | str | Identifiant **éphémère**, propre à une caméra et à une session. Ne permet pas de retrouver une personne. |
| `camera_id` | str | Caméra source |
| `t` | float | Horodatage (secondes epoch) |
| `x`, `y` | float | Position sur le plan, en mètres |

Aucune image, aucun descripteur d'apparence, aucun attribut (âge, genre…) n'est stocké.

## N2 — `ZoneVisit`

| Champ | Type | Description |
|---|---|---|
| `track_id` | str | Piste éphémère |
| `zone_id` | str | Zone du fichier de config |
| `t_enter`, `t_exit` | float | Entrée et sortie de zone |
| `dwell_s` | float | Durée de présence |

## N3 — `ZoneOccupancy` (agrégat par créneau)

| Champ | Type | Description |
|---|---|---|
| `zone_id` | str | Zone |
| `bucket_start` | float | Début du créneau |
| `mean_occupancy` | float | Nombre moyen de personnes présentes |
| `max_occupancy` | int | Pic sur le créneau |
| `entries` | int | Nouvelles visites démarrées dans le créneau |

## Types de zones

Le champ `type` d'une zone dans le YAML pilote les indicateurs calculés :

| Type | Indicateurs | Cas d'usage |
|---|---|---|
| `comptage` | entrées / sorties | UC-04 |
| `file_attente` | occupation, attente (Little), abandons | UC-01, UC-08 |
| `comptoir_approche` / `comptoir_service` | abandons de comptoir | UC-02 |
| `allee` | occupation par créneau, heatmap, gêne | UC-03, UC-07 |
| `tete_de_gondole` | passages, arrêts, taux d'arrêt | UC-05 |
| `rayon` | visiteurs, dwell, conversion | UC-06 |
| `securite` | objet statique, densité | UC-10 |
