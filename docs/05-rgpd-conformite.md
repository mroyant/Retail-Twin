# 05 — RGPD & conformité

> ⚠️ Ce document est une **grille de travail technique**, pas un avis juridique. Un déploiement réel doit être validé par le DPO du client et, idéalement, par un avocat spécialisé.

## Pourquoi c'est le sujet n°1

Réutiliser des caméras de **vidéoprotection** (finalité : sécurité des biens et des personnes) pour faire de l'**analyse commerciale et opérationnelle** est un **changement de finalité**. En France, la CNIL a publié en 2022 une position sur les caméras dites « augmentées » ou « intelligentes » dans les lieux ouverts au public, qui s'applique directement à notre cas. Ce qu'il faut en retenir pour le produit :

- un traitement d'analyse vidéo de personnes est un **traitement de données personnelles**, même sans reconnaissance faciale, tant qu'il s'appuie sur des images de personnes ;
- il faut une **base légale** (en pratique, l'intérêt légitime du magasin) et une **mise en balance** avec les droits des personnes ;
- le **droit d'opposition** des personnes filmées est une difficulté majeure pour ce type de dispositif. La CNIL a évoqué une exception pour les traitements **strictement statistiques**, qui produisent immédiatement des données anonymes ;
- une **AIPD** (analyse d'impact relative à la protection des données) est très probablement requise.

→ Le produit doit être conçu pour entrer dans le cadre le plus favorable : **traitement statistique, anonymisation à la volée, aucune image conservée, aucun suivi individuel exploité.**

## Principes de conception (non négociables)

| # | Principe | Traduction technique |
|---|---|---|
| 1 | Pas de biométrie | Aucune reconnaissance faciale, aucun gabarit de visage, aucune estimation d'âge ou de genre |
| 2 | Aucune image stockée | Les frames sont traitées en mémoire puis détruites ; pas d'enregistrement de clips, même pour du debug en production |
| 3 | Identifiants éphémères | Les `track_id` sont réinitialisés par caméra et par session ; aucun lien entre deux visites |
| 4 | Agrégation au plus tôt | Seuls des agrégats (N3) quittent le magasin ; les pistes (N1) ont une rétention courte |
| 5 | Seuil de k-anonymat sur les restitutions | Pas d'indicateur affiché quand il repose sur moins de k personnes (ex. k = 5) sur un créneau |
| 6 | Pas de mesure individuelle des salariés | Les salariés ne sont jamais isolés dans les indicateurs ; information et consultation du CSE |
| 7 | Traitement en local (edge) | Les images ne transitent pas sur Internet |

## Checklist avant pilote

- [ ] Désigner le responsable de traitement (le magasin) et notre rôle (sous-traitant) ; signer un contrat de sous-traitance (art. 28 RGPD).
- [ ] Réaliser l'AIPD avec le DPO du client.
- [ ] Mettre à jour le registre des traitements du client.
- [ ] Mettre à jour la signalétique en magasin : information claire sur l'analyse statistique des flux, distincte de la vidéoprotection.
- [ ] Informer et consulter le CSE (les salariés apparaissent dans les images).
- [ ] Vérifier le cadre de la vidéoprotection existante (autorisation préfectorale) et l'impact de la nouvelle finalité.
- [ ] Documenter les durées de conservation N1 / N2 / N3.
- [ ] Mettre en place la sécurité : chiffrement, accès restreints, journalisation.

## Pour les données de test du POC

- Utiliser en priorité des **datasets publics** dont la licence permet l'usage (vérifier la licence de chaque dataset avant de l'ajouter) ou des vidéos tournées **avec le consentement** des personnes filmées.
- **Ne jamais committer de vidéo dans le repo** : le dossier `poc/data/` est ignoré par git.
- Pas de vidéo réelle de magasin client sur un poste personnel ou un cloud non validé.
