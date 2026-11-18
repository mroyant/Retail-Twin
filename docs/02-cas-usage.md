# 02 — Analyse du besoin & cas d'usage

> Les chiffres marqués **(hypothèse)** sont des ordres de grandeur à valider avec un magasin pilote. Ils servent à prioriser, pas à vendre.

## 1. Analyse du besoin

### Méthode

Chaque cas d'usage part d'une **décision qu'une personne précise prend déjà aujourd'hui, mais à l'aveugle**. On le retient seulement si on peut répondre à quatre questions :

1. **Qui décide ?** Un rôle identifié en magasin.
2. **Quelle action ?** Un verbe concret : ouvrir, affecter, déplacer, planifier.
3. **Quel signal vidéo ?** Ce qu'on mesure sur le plan : passage, présence, durée, densité, séquence.
4. **Comment on prouve la valeur ?** Un KPI avant/après mesurable.

### Ce que la vidéo sait mesurer (briques de base)

Tous les cas d'usage se ramènent à une combinaison de six primitives. C'est ce que le POC doit savoir produire.

| Primitive | Définition | Exemple |
|---|---|---|
| **Comptage de ligne** | Nombre de pistes qui franchissent une ligne, par sens | Entrées et sorties du magasin |
| **Occupation de zone** | Nombre de personnes présentes dans un polygone à l'instant t | Personnes dans la file de la caisse 4 |
| **Temps de présence (dwell)** | Durée entre l'entrée et la sortie d'une piste dans une zone | 42 s devant la tête de gondole TG-07 |
| **Taux d'arrêt** | Part des passages dans une zone qui deviennent un arrêt (dwell > seuil) | 18 % des passants s'arrêtent au rayon vin |
| **Densité / heatmap** | Présence cumulée par cellule de grille (ex. 1 m × 1 m) | Allées chaudes et froides |
| **Séquence de zones** | Ordre des zones visitées par une piste | Entrée → fruits et légumes → boucherie → caisses |

### Données complémentaires (hors vidéo)

| Source | Usage | Disponibilité |
|---|---|---|
| Tickets de caisse agrégés (CA, nb tickets par 15 min, par rayon) | Taux de conversion, corrélation fréquentation / ventes | À négocier : export du SI caisse |
| Planning du personnel | Comparer besoin et affectation réelle | Export de l'outil de planning |
| Plan du magasin et planogrammes | Définir les zones, rattacher une zone à une catégorie produit | PDF / DWG à numériser |
| Calendrier promo, météo, jours fériés, vacances scolaires | Variables explicatives pour la prévision | Données ouvertes ou internes |

---

## 2. Cas d'usage

Synthèse, triée par priorité :

| ID | Cas d'usage | Décideur | Horizon | Complexité | Valeur | Priorité |
|---|---|---|---|---|---|---|
| UC-01 | Ouverture de caisse anticipée | Resp. caisses | Temps réel | Faible | Forte | **P0** |
| UC-02 | Staffing des comptoirs traditionnels | Manager frais | Temps réel + semaine | Faible | Forte | **P0** |
| UC-03 | Créneaux de remplissage sans gêne | Chef de rayon | Jour | Faible | Moyenne | **P0** |
| UC-04 | Plannings du personnel par créneau de 30 min | Directeur | Semaine | Moyenne | Forte | P1 |
| UC-05 | Mesure de l'exposition des têtes de gondole (retail media) | Dir. commerciale | Campagne | Moyenne | Forte (revenu) | P1 |
| UC-06 | Taux de conversion par rayon | Category manager | Mois | Moyenne | Forte | P1 |
| UC-07 | Détection des zones mortes et test A/B d'implantation | Category manager | Mois | Moyenne | Moyenne | P1 |
| UC-08 | Assistance aux caisses automatiques | Hôte de caisse | Temps réel | Moyenne | Moyenne | P2 |
| UC-09 | Conflits préparateurs drive / clients | Resp. drive | Semaine | Élevée | Moyenne | P2 |
| UC-10 | Sécurité : sorties de secours et densité | Resp. sécurité | Temps réel | Faible | Conformité | P2 |
| UC-11 | Parcours types et produits complémentaires | Category manager | Trimestre | Élevée | Moyenne | P3 |

**Pourquoi ces trois P0 :** ils ne demandent que des primitives simples (occupation, dwell, heatmap), ils produisent une action dans la journée (donc une preuve de valeur rapide pour le pilote), et ils ne dépendent d'aucune donnée externe.

---

### UC-01 — Ouverture de caisse anticipée `P0`

**Problème.** Le responsable caisses ouvre une caisse quand il *voit* la file s'allonger. C'est trop tard : il faut 3 à 5 minutes **(hypothèse)** pour faire venir quelqu'un, et pendant ce temps certains clients abandonnent leur chariot ou repartent agacés.

**Décision.** « Appeler un renfort en caisse **maintenant**, parce que dans 10 minutes la file dépassera le seuil. »

**Signaux vidéo.**
- Occupation de chaque zone de file (polygone devant chaque caisse ouverte).
- **Flux montant** : nombre de personnes qui entrent dans la zone pré-caisse (dernières allées, rayon presse) par minute. C'est l'indicateur avancé, qui précède la file de 5 à 10 minutes **(hypothèse)**.
- Temps d'attente estimé par la **loi de Little** : `W = L / λ`, avec L la longueur de file et λ le débit de sortie de caisse.

**Règle de déclenchement (v1, sans ML).**
`attente_estimée > 4 min` OU `flux_pré-caisse(10 min) > capacité_caisses_ouvertes × 1,2` → alerte sur la tablette ou la radio du responsable.

**KPI de succès.** Temps d'attente moyen et P90 en caisse ; nombre de minutes par jour avec une attente > 5 min ; nombre de « files abandonnées » (piste qui entre dans une zone de file puis en sort sans passer en caisse).

**Données complémentaires.** Aucune pour la v1. En v2, le nombre de caisses ouvertes vient du SI caisse.

**Risques.** Occlusions dans les files compactes, d'où une caméra en plongée nécessaire. Les accompagnants (enfants, couples) gonflent la file : il faudra un facteur de correction « groupe ».

---

### UC-02 — Staffing des comptoirs traditionnels `P0`

**Problème.** Boucherie, fromagerie, poissonnerie, traiteur : un vendeur est seul au comptoir, une file de 4 personnes se forme, et une partie des clients **regarde la file et repart sans attendre**. C'est une vente perdue, et personne ne la voit.

**Décision.**
- Temps réel : « Envoyer le second vendeur au comptoir fromage. »
- Hebdo : « Mettre 2 personnes au comptoir boucherie le samedi de 10h30 à 12h30, une seule le mardi. »

**Signaux vidéo.**
- Occupation de la zone d'attente du comptoir.
- **Abandon de comptoir** : une piste entre dans la zone d'approche, y reste entre 5 et 40 s **(paramètres à calibrer)**, puis repart sans entrer dans la zone de service (contact au comptoir). C'est l'indicateur différenciant : aucun autre outil du magasin ne le mesure.
- Nombre de personnes côté vendeur (zone derrière le comptoir), pour connaître l'effectif réel sans planning.

**KPI de succès.** Taux d'abandon au comptoir ; CA du comptoir par heure d'ouverture ; part des créneaux en sous-effectif.

**Pourquoi c'est fort.** L'abandon est une perte directe, mesurable, et facile à expliquer à un directeur de magasin : « samedi, 37 personnes sont reparties du comptoir fromage sans être servies » **(exemple fictif)**.

---

### UC-03 — Créneaux de remplissage sans gêne `P0`

**Problème.** Les équipes remplissent les rayons en journée avec des palettes et des rolls dans les allées. Aux heures de pointe, cela bloque les clients, crée des tensions et ralentit les deux. Les créneaux de mise en rayon sont aujourd'hui fixés par habitude.

**Décision.** « Remplir l'allée 7 (épicerie salée) entre 14h et 15h30, pas à 17h. Remplir l'allée 12 avant 10h le samedi. »

**Signaux vidéo.**
- Occupation moyenne par allée et par créneau de 30 min, par jour de la semaine (profil hebdomadaire).
- **Détection d'obstacle** : un roll ou une palette dans une allée (classe d'objet supplémentaire, ou zone de l'allée sans passage alors qu'il y a de la fréquentation autour).
- **Indice de gêne** : densité de clients dans une allée *pendant* qu'un obstacle y est présent × durée.

**Livrable.** Un tableau « meilleur créneau de remplissage » par allée et par jour, recalculé chaque semaine, et l'indice de gêne réel des 7 derniers jours.

**KPI de succès.** Indice de gêne cumulé par semaine ; part des remplissages faits sur un créneau recommandé.

---

### UC-04 — Plannings du personnel par créneau de 30 min `P1`

**Problème.** Les plannings sont construits à partir du CA historique par heure. Or le CA arrive *en caisse*, avec un décalage sur la présence réelle en magasin, et il ne dit pas *où* sont les clients.

**Décision.** « La semaine prochaine, il faut 6 ETP en surface le samedi 10h-12h, dont 2 au frais. »

**Signaux.** Courbe d'entrées par 30 min (comptage de ligne) ; occupation par grande zone (frais, épicerie, DPH, caisses) ; temps de visite moyen ; profil par jour de la semaine.

**Méthode.** Modèle de prévision de la fréquentation à J+7 (saisonnalité hebdo, vacances, météo, promos), traduit en besoin d'ETP par zone avec des ratios fixés par le magasin (ex. 1 vendeur par 25 clients présents au rayon frais **(hypothèse)**).

**KPI.** Écart besoin / affectation ; heures de sous-effectif et de sur-effectif ; temps d'attente en caisse et au comptoir (lien avec UC-01 et UC-02).

---

### UC-05 — Mesure de l'exposition des têtes de gondole (retail media) `P1`

**Problème.** Les fournisseurs paient pour des têtes de gondole et des emplacements promo. Le distributeur ne peut leur fournir que les ventes, pas l'**audience**. Le retail media en ligne vend des impressions mesurées ; le magasin physique, non.

**Décision.**
- Côté distributeur : fixer le prix d'un emplacement selon son audience mesurée.
- Côté fournisseur : choisir son emplacement et mesurer le ROI de sa campagne.

**Signaux vidéo.**
- **Passages** devant la TG (zone d'exposition).
- **Arrêts** : dwell > 3 s dans la zone d'attention, à 1 m de la TG.
- **Taux d'arrêt** = arrêts / passages.
- Croisés avec les ventes du produit sur la période → **taux de conversion de l'emplacement**.

**Livrable.** Un rapport de campagne par TG : audience, taux d'arrêt, évolution face à une TG de référence.

**Pourquoi c'est stratégique.** C'est une **source de revenu** et non une simple économie : c'est le cas d'usage qui peut financer le déploiement. À mettre en avant dans la présentation.

---

### UC-06 — Taux de conversion par rayon `P1`

**Problème.** On sait ce qu'un rayon vend, pas combien de personnes y sont passées sans acheter. Un rayon qui vend peu peut être peu visité (problème d'emplacement) ou très visité mais peu converti (problème de prix, d'assortiment ou de rupture).

**Signal.** Visiteurs du rayon (piste avec un dwell > 5 s dans la zone) / tickets contenant au moins un article du rayon, par jour.

**Décision.** Distinguer un problème de trafic (déplacer, signaler) d'un problème d'offre (prix, assortiment, rupture). Une **chute brutale de conversion à trafic égal** sur un rayon est un bon proxy de **rupture de stock**.

**Dépendance.** Export des tickets agrégés par rayon et par jour, sans donnée client.

---

### UC-07 — Zones mortes et test A/B d'implantation `P1`

**Problème.** Un changement d'implantation (déplacer le rayon bio, créer une allée promo) se décide sur avis d'expert et s'évalue au CA total, qui bouge pour mille raisons.

**Signaux.** Heatmap de présence ; pourcentage des visiteurs qui passent au moins une fois dans chaque zone (**taux de couverture**) ; comparaison avant/après sur 4 semaines, à jour de semaine équivalent.

**Décision.** « L'allée 14 n'est visitée que par 9 % des clients **(exemple fictif)**, on y met un produit d'appel » ; « le déplacement du rayon bio a augmenté son trafic de X % sans faire baisser celui du rayon voisin. »

---

### UC-08 — Assistance aux caisses automatiques `P2`

**Signal.** Dwell anormalement long sur un poste de caisse automatique (plus de 2× la médiane) ; client immobile à côté du poste ; file en amont de la zone SCO.
**Décision.** L'hôte de caisse est alerté *avant* que le client ne cherche quelqu'un du regard.
**KPI.** Temps moyen d'intervention ; débit de la zone SCO.

### UC-09 — Conflits préparateurs drive / clients `P2`

**Signal.** Les préparateurs ont un chariot de préparation reconnaissable (classe d'objet dédiée, ou badge visuel). On mesure leur temps passé dans des allées denses.
**Décision.** Fixer les tournées de préparation sur les créneaux creux de chaque allée (la même logique que UC-03).
**Point d'attention.** Le dispositif ne doit pas mesurer la productivité individuelle d'un salarié (voir RGPD). On n'agrège que par allée et par créneau.

### UC-10 — Sécurité : sorties de secours et densité `P2`

**Signal.** Objet statique dans la zone d'une sortie de secours depuis plus de N minutes ; densité dépassant un seuil (personnes/m²) dans une zone, en période d'affluence (fêtes, ouverture de promo).
**Décision.** Dégager l'issue ; réguler l'accès à une zone promo.
**Intérêt.** Argument de conformité, avec une justification simple pour le CSE et la direction.

### UC-11 — Parcours types et produits complémentaires `P3`

**Signal.** Séquences de zones par piste, regroupées (clustering) en 5 à 8 parcours types (ex. « mission express : entrée → boulangerie → caisse », « grosses courses : tout le magasin »).
**Décision.** Placer des produits complémentaires sur les parcours dominants ; adapter le magasin aux missions (ex. caisse express près de la boulangerie).
**Difficulté.** Il faut une **ré-identification inter-caméras** (sans biométrie) pour suivre une piste dans tout le magasin. C'est techniquement difficile, et c'est le point le plus sensible côté RGPD. On le laisse en dernier.

---

## 3. Ce que le POC doit démontrer

Pour valider les trois P0 sur une vidéo de test :

- [ ] Détection et tracking de personnes sur une vidéo de magasin (ou un dataset public).
- [ ] Projection des positions image vers le plan en mètres (homographie par caméra).
- [ ] Zones définies dans un fichier de configuration (polygones sur le plan).
- [ ] Calcul de l'occupation par zone et par créneau → UC-01, UC-03.
- [ ] Calcul des visites et du dwell par zone → UC-02, UC-05.
- [ ] Détection d'abandon de file ou de comptoir (entrée en zone d'approche sans passage en zone de service) → UC-01, UC-02.
- [ ] Estimation du temps d'attente par la loi de Little → UC-01.
- [ ] Heatmap sur le plan → UC-03, UC-07.

## 4. Questions ouvertes à poser au client pilote

1. Combien de caméras, quel modèle, quelle résolution, accès RTSP ou seulement à l'enregistreur (NVR) ?
2. Quelles caméras couvrent les caisses, les comptoirs et l'entrée ? Sont-elles en plongée ?
3. Peut-on obtenir un export de tickets agrégés par 15 min et par rayon ?
4. Qui est le DPO, et une AIPD existe-t-elle pour la vidéoprotection actuelle ?
5. Quel cas d'usage le directeur choisirait-il s'il ne devait en garder qu'un ?
6. Comment l'alerte doit-elle arriver : radio, tablette, montre, écran en réserve ?
