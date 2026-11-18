# ADR-0001 — Traitement vidéo en magasin, sans biométrie

- **Statut :** accepté
- **Date :** 2026-11-18

## Contexte

Le produit analyse des images de clients et de salariés. Le cadre RGPD et la position de la CNIL sur les caméras augmentées rendent risqué tout dispositif qui conserve des images, identifie des personnes ou les suit dans le temps. Les clients (enseignes) seront eux aussi très sensibles à ce point.

## Décision

1. Les images sont traitées **en local dans le magasin** et ne sont jamais stockées ni transmises.
2. Aucune donnée biométrique ni aucun attribut de personne (visage, âge, genre, apparence) n'est extrait.
3. Seules des **coordonnées sur le plan** liées à des identifiants éphémères (N1), puis des agrégats (N3), sont produites.

## Alternatives considérées

- **Traitement cloud des flux :** plus simple à opérer, mais les images sortent du magasin, ce qui pose des problèmes de bande passante et un risque RGPD majeur. Rejeté.
- **Ré-identification par apparence pour suivre une personne dans tout le magasin :** utile pour UC-11 (parcours), mais revient à traiter un descripteur d'apparence de la personne. Reporté : à réévaluer avec un avis juridique.

## Conséquences

- Il faut du calcul en magasin (GPU edge), ce qui est un coût matériel à intégrer à l'offre.
- Le debug en production est plus difficile (pas d'images à rejouer) : il faut des outils de diagnostic sur les pistes elles-mêmes.
- C'est un argument commercial fort : « aucune image ne quitte votre magasin ».
