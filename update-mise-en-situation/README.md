# `/update-mise-en-situation` — Passer le relais sans réexpliquer

Skill [Claude Code](https://claude.com/claude-code) pour les longues conversations d'exercice (ex. construire un workflow n8n). Quand la conversation devient trop longue et qu'il faut en changer, cette skill capte l'avancement dans un fichier propre à réinjecter dans une nouvelle conversation — sans tout réexpliquer.

## Le principe

Une mise en situation = un dossier, avec deux fichiers séparés :

- `Énoncé.md` — le sujet de l'exercice (fixe, jamais réécrit).
- `Avancée.md` — un instantané de l'état, réécrit à chaque passage (pas un historique qui s'empile).

L'idée : au lieu de recoller un résumé qui se périme, on maintient un fichier vivant. Nouvelle conversation → on donne le fichier → le contexte revient d'un coup.

## Le flux

1. **Au lancement** — la skill te donne un prompt prêt à coller, pour générer le récap de l'état du workflow dans ta conversation.
2. **Titre** — tu donnes le titre de la mise en situation (= nom du dossier).
3. **Dossier** — s'il n'existe pas, on crée l'`Énoncé.md` (tu colles l'énoncé, en plusieurs messages si besoin) ; s'il existe, on met à jour l'`Avancée.md`.
4. **Screens** (optionnels) — servent de source de vérité de l'état réel ; ils aident à la compréhension, pas obligatoires.
5. **Écriture** — l'`Avancée.md` est (ré)écrit : les nodes dans l'ordre + leur rôle, les points importants/décisions, la prochaine étape.

## Ce qui la rend utile

- **Anti-mélange** : un dossier par mise en situation, énoncé et avancée séparés, uniquement ce qui est fourni (jamais inventer, jamais fusionner deux situations).
- **Instantané, pas changelog** : l'avancée reflète l'état courant, on ne raconte pas ce qui a bougé — l'ordre des nodes suffit à voir où on en est.
- **Autonome** : le fichier se suffit à lui-même pour reprendre ailleurs, même sans les screens.

## Prérequis

- [Claude Code](https://claude.com/claude-code)
- Un vault [Obsidian](https://obsidian.md)

## Installation

```bash
cp -r update-mise-en-situation ~/.claude/skills/update-mise-en-situation
```

Disponible au démarrage de session suivant. On la déclenche avec `/update-mise-en-situation`.

## Configuration

Dans `SKILL.md` (section **Variables fixes**) :

- `VAULT` — la racine du vault Obsidian
- `DOSSIER` — où sont rangées les mises en situation (par défaut `03_knowledge/formations/Mise en situation/`)

## Licence

MIT
