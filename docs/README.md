# Documentation

| # | Document | Question à laquelle il répond |
|---|---|---|
| 01 | [Vision](01-vision.md) | Pourquoi ce produit, pour qui, et ce qu'il n'est pas |
| 02 | [Cas d'usage](02-cas-usage.md) | Quelles décisions concrètes on aide à prendre, et dans quel ordre on les attaque |
| 03 | [Architecture](03-architecture.md) | Comment la vidéo devient un indicateur |
| 04 | [Modèle de données](04-modele-donnees.md) | Ce qu'on stocke, et ce qu'on ne stocke jamais |
| 05 | [RGPD & conformité](05-rgpd-conformite.md) | Ce qu'il faut régler avant de filmer un vrai client |
| 06 | [Roadmap](06-roadmap.md) | Les étapes du POC à un pilote |
| — | [ADR](adr/) | Les décisions techniques importantes et leur justification |

## Conventions

- Les docs sont en Markdown, en français, et versionnées avec le code. Une modification de doc passe par une PR comme le reste.
- Une décision technique structurante (choix de modèle, edge ou cloud, format de stockage…) donne lieu à un ADR : copier [adr/0000-template.md](adr/0000-template.md).
- Les chiffres non mesurés sont marqués **(hypothèse)**. On ne les présente pas comme des faits tant qu'ils n'ont pas été validés sur le terrain.
