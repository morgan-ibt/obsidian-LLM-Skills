# `/culture` — Capturer le savoir par la méthode Feynman

Skill [Claude Code](https://claude.com/claude-code) qui capte un point d'une source (vidéo, podcast, livre…) dans un vault Obsidian **en le faisant comprendre**, pas en le résumant à ta place.

## Le principe

Au lieu de recopier un résumé, la skill te fait **expliquer la notion avec tes mots**, puis te questionne (méthode Feynman) jusqu'à ce que ce soit clair et juste. Si tu te trompes, elle pointe l'endroit précis qui cloche et repose une question — sans te donner la réponse. Elle ne débloque qu'une fois, si tu cales vraiment.

Résultat : une fiche qui contient **ta** compréhension, dans **tes** mots — pas un cours recopié.

## Le flux

1. **La source** — type (vidéo YouTube, podcast, livre…), titre, lien. Une source = une note ; les points s'y accumulent.
2. **Le brut** — tu expliques la notion en vrac.
3. **Feynman** — reformulation guidée, une question à la fois, jusqu'à maîtrise.
4. **Nommer** — titre du point, tags, thème (proposés, tu valides).
5. **Écrire** — la fiche est rangée dans le vault, chaque point daté.

## Ce qui la rend différente

- **Ta compréhension reste la tienne.** L'IA propose, elle ne réécrit ni ne répond à ta place.
- **Les tags sont un liant** : ils relient un même concept croisé dans plusieurs sources (un point isolé meurt, un point tissé vit).
- **Un seul dossier**, classement par thème (en-tête de note) + tags. Pas de dossier par thème.
- **Chaque point est daté** → prêt pour une révision espacée (courbe de l'oubli).

## Ce que ça produit

```markdown
---
type: cahier-culture
date: 2026-09-24
source: "Titre exact de la source"
lien: https://...
theme: IA
---

# Titre de la source

> 📎 [Titre](https://...) · 🏷️ Thème : IA

## Sommaire
- [[#Titre du point|Titre du point]]

---

## Titre du point

📅 2026-09-24 · #tag1 #tag2

<ta compréhension, dans tes mots>
```

## Prérequis

- [Claude Code](https://claude.com/claude-code)
- Un vault [Obsidian](https://obsidian.md)

## Installation

```bash
cp -r culture ~/.claude/skills/culture
```

Disponible au démarrage de session suivant. On la déclenche avec `/culture`.

## Configuration

Dans `SKILL.md` (section **Variables fixes**) :

- `VAULT` — la racine du vault Obsidian
- `DOSSIER` — où sont créées les fiches (par défaut `03_knowledge/cahier-de-culture/`)

## Licence

MIT
