# travel-agent

Planificateur d'itinéraires raisonnant sur des faits vérifiés, sourcés et
périssables. **Le modèle compose, le code oppose son veto.** Aucune donnée
personnelle de voyage ne quitte la machine du voyageur.

Projet de la constellation [Libre AI](https://libre-ai.fr) — couche 1.

## Le problème

La planification de voyage assistée échoue de quatre façons, et les quatre
coûtent au voyageur :

1. **Le rythme n'est pas dérivé de la durée.** Une règle « trois visites
   majeures par jour » appliquée à un long séjour exige plus de visites
   majeures qu'une ville n'en compte. Elle fabrique du remplissage et sature le
   voyageur autour du sixième jour.
2. **Le city-pass est traité comme un booléen.** Quand un pass plafonne à
   quelques jours et que le séjour est plus long, « rentable ou non » est une
   question mal posée : la vraie question est _où placer la fenêtre_.
3. **Les connaissances de destination sont enfouies dans le prompt.** Chaque
   nouvelle ville devient alors un fork. Ingérable à trois villes.
4. **Les faits volatils périment en silence.** Un horaire erroné coûte une
   matinée. Un itinéraire faux est pire qu'une absence d'itinéraire.

## Les principes

**Trois strates, étanches.** La logique de raisonnement ne contient aucun fait
sur une ville ; un pack de ville ne contient aucune règle de planification ni
aucune fenêtre de séjour ; une instance de voyage ne vit pas dans ce dépôt.

**Le code calcule ce qu'un modèle calcule mal**, et refuse ce qui viole une
contrainte vérifiable. Il ne rédige pas d'itinéraire : composer de la prose en
TypeScript réimplémenterait le modèle, en moins bien.

**La fraîcheur se mesure contre le séjour, pas contre aujourd'hui.** Un pack
vérifié hors saison passerait un délai absolu tout en portant des horaires faux.

**Rien n'est réservé.** L'agent planifie ; toute action irréversible sort en
liste d'actions humaines ordonnées par échéance.

**Les faits curés sont reversés en amont.** Le pack est le cache local d'un
commun, pas une base privée — et c'est la seule sortie connue de la dette de
re-vérification.

## État

Spécifié. Zéro code de planification à ce jour ; le socle d'étanchéité et son
gate sont en place. L'état fait autorité dans [`project.v1.yaml`](project.v1.yaml),
qui porte les phases et leurs critères de sortie.

## Documents

| Document                                                                                       | Rôle                                                    |
| ---------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| [`AGENTS.md`](AGENTS.md)                                                                       | périmètre canonique et règles de travail                |
| [`project.v1.yaml`](project.v1.yaml)                                                           | autorité d'état : phases, critères, prédicats d'abandon |
| [`docs/specs/2026-08-07-travel-agent-design.md`](docs/specs/2026-08-07-travel-agent-design.md) | architecture                                            |
| [`docs/specs/2026-08-07-travel-agent-phases.md`](docs/specs/2026-08-07-travel-agent-phases.md) | phases, livrables, organisation                         |
| [`docs/adr/`](docs/adr/)                                                                       | décisions d'architecture                                |

## Licences

Double licence, par nature d'artefact — attribution par fichier faisant foi
dans [`REUSE.toml`](REUSE.toml), vérifiée par `reuse lint` :

| Artefact             | Licence                  |
| -------------------- | ------------------------ |
| Code                 | [Apache-2.0](LICENSE)    |
| `data/`              | [ODbL-1.0](data/LICENSE) |
| Itinéraires produits | libres, avec attribution |

## Vérifications

```sh
bun run check:trip-isolation   # aucune instance de voyage dans l'arbre suivi
bun run check:local            # gate d'isolation, lint, types, tests
```

<!-- libre-ai:project-status:begin -->
<!-- Section générée depuis project.v1.yaml — ne pas éditer à la main. -->

- Situation actuelle : Spec approuvée et couche de raisonnement écrite, city-agnostic. Zéro code. Le pack de la première ville porte sa structure mais son sous-ensemble planifiable est vide : aucun itinéraire ne peut être chiffré tant qu'il l'est. La v1 vise un usage personnel sur une à deux villes ; le passage à l'échelle se tranche à la troisième, sur mesures et non sur projection.
- Maturité : specified
- Exposition : spec-published
- Confiance : medium
- Preuves vérifiées le : 2026-08-18
- Avancement : 14,6 % du périmètre actuellement déclaré

<!-- libre-ai:project-status:end -->
