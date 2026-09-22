# `/idee` — Capture d'idées dans Obsidian

Skill [Claude Code](https://claude.com/claude-code) qui capte une idée en ~2 minutes et crée une fiche rangée dans un vault Obsidian, via un flux dynamique et cliquable.

## Le principe

On tape `/idee`, on balance son idée en une phrase. Ensuite, tout est cliquable :

1. **Titre** — 3 propositions cohérentes avec le sujet (ou on écrit le sien).
2. **Tags** — quelques catégories à cocher, pour retrouver l'idée plus tard.
3. **Pistes** — 2-3 améliorations suggérées par l'IA, on garde celles qu'on veut.
4. **Validation** — on valide (ou on revient sur une étape), et la fiche est créée.

Règle du jeu : **l'idée brute reste intacte, mot pour mot.** L'IA propose, elle ne réécrit jamais à la place de l'auteur ; ses suggestions vont dans une section séparée.

## Ce que ça produit

Une note markdown dans le vault :

```markdown
---
type: idee
date: 2026-09-22
statut: brute
tags:
  - idee
  - skill
  - productivite
---

# Titre de l'idée

## 💡 L'idée
<le texte brut, non modifié>

## 🔧 Pistes & annotations
- <les pistes gardées>
```

## Prérequis

- [Claude Code](https://claude.com/claude-code)
- Un vault [Obsidian](https://obsidian.md) (ou n'importe quel dossier de notes markdown)

## Installation

Copier le dossier dans les skills de Claude Code :

```bash
cp -r idee ~/.claude/skills/idee
```

La skill est disponible au démarrage de session suivant. On la déclenche avec `/idee`.

## Configuration

Deux chemins sont à adapter dans `SKILL.md` (section **Variables fixes**) :

- `VAULT` — la racine du vault Obsidian
- `DOSSIER IDÉES` — où sont créées les fiches (par défaut `07_idees/`)

Le rangement se fait par **tags dans un seul dossier** (pas de sous-dossiers) : une idée peut porter plusieurs tags, filtrable via Dataview.

## Licence

MIT — fais-en ce que tu veux.
