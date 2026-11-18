# 01 — Vision produit

## Le problème

Un supermarché pilote aujourd'hui son exploitation avec deux sources de données :

- **les tickets de caisse** : ce qui a été acheté, mais rien sur ce qui a été cherché, regardé ou abandonné ;
- **l'intuition des managers** : qui connaît le magasin, mais ne peut pas observer 3 000 m² en continu.

Il manque ce qui se passe **entre l'entrée et la caisse** : où vont les clients, combien de temps ils restent, où ça bloque, où personne ne va.

Les magasins ont déjà le capteur pour le mesurer : 20 à 80 caméras de vidéoprotection **(hypothèse, à confirmer par magasin)**, enregistrées en continu et regardées seulement après un incident.

## La proposition

Retail-Twin réutilise ces caméras pour produire un **jumeau numérique des flux** du magasin :

1. on détecte et on suit des **silhouettes anonymes** (pas de visage, pas d'identité) ;
2. on projette leurs positions sur le **plan du magasin** (coordonnées en mètres) ;
3. on agrège ces positions en **indicateurs par zone et par créneau** : passages, temps passé, files d'attente, densité ;
4. on transforme ces indicateurs en **décisions** : des alertes en temps réel et des recommandations de planification.

## Pour qui

| Utilisateur | Ce qu'il veut | Horizon |
|---|---|---|
| Responsable caisses | Savoir quand ouvrir ou fermer une caisse | Temps réel (minutes) |
| Directeur de magasin | Dimensionner les plannings du personnel | Semaine |
| Chef de rayon | Savoir quand remplir sans gêner, et quelles zones sont mortes | Jour / semaine |
| Direction commerciale / category manager | Mesurer l'effet d'une implantation ou d'une promo | Mois |
| Fournisseurs (retail media) | Une preuve d'exposition d'une tête de gondole | Campagne |

## Ce que le produit n'est pas

- **Pas de reconnaissance faciale ni d'identification.** Aucune donnée biométrique n'est traitée.
- **Pas de suivi d'un client d'une visite à l'autre.** Un identifiant de piste ne vit que le temps de la visite.
- **Pas de surveillance des salariés.** Les indicateurs ne descendent jamais au niveau d'une personne identifiable.
- **Pas un outil anti-vol.** Une autre finalité, avec un autre cadre juridique ; on ne mélange pas les deux.

Ces limites sont des choix de conception autant que des contraintes légales : voir [05-rgpd-conformite.md](05-rgpd-conformite.md).

## Ce qui nous différencie (à challenger)

- **Zéro matériel neuf** : on utilise les caméras existantes. Les solutions concurrentes posent souvent leurs propres capteurs (comptage 3D, LiDAR).
- **Orienté décision** : chaque indicateur est rattaché à une action et à la personne qui la prend, pas seulement à un dashboard.
- **Privacy by design** : traitement en local, seules des données agrégées sortent du magasin.
