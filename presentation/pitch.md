# Trame du pitch (brouillon)

1. **Accroche** : « Votre magasin a 40 caméras. Elles servent seulement quand il y a un vol. »
2. **Le problème** : on pilote le magasin avec les tickets de caisse, qui ne montrent pas ce qui se passe entre l'entrée et la caisse (files, abandons, allées vides ou bouchées).
3. **La solution** : Retail-Twin transforme les caméras existantes en capteur de flux anonyme, avec zéro matériel neuf.
4. **Comment ça marche** : caméra → silhouettes anonymes → plan du magasin → indicateurs → décisions. Schéma tiré de [docs/03-architecture.md](../docs/03-architecture.md).
5. **Trois décisions dès le premier mois** (les cas P0) :
   - ouvrir une caisse *avant* que la file se forme ;
   - mettre le bon nombre de vendeurs au comptoir fromage le samedi ;
   - remplir l'allée 7 à 14h plutôt qu'à 17h.
6. **Démo** : voir [demo.md](demo.md).
7. **Et ensuite, du revenu** : mesurer l'audience des têtes de gondole et la vendre aux fournisseurs (retail media).
8. **Privacy by design** : pas de visage, pas d'image stockée, traitement en magasin ([ADR-0001](../docs/adr/0001-traitement-edge-sans-biometrie.md)).
9. **Roadmap et demande** : un magasin pilote, 5 caméras, 4 semaines de mesure avant/après.
