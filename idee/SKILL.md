---
name: idee
description: Capte une idée en ~2 min et crée une fiche dans le vault Obsidian (07_idees/). Flux dynamique et cliquable (outil de questions interactives) : idée brute → titre → tags → pistes IA (optionnelles) → validation → création. Trigger : "/idee", "/idée", "capte cette idée", "note cette idée".
---

# idee — Capture d'idée express (dynamique)

Objectif : Morgan balance une idée, elle finit rangée dans Obsidian en ~2 min, **sans une tonne de questions**. Son idée reste la sienne ; l'IA propose mais ne réécrit jamais à sa place sans confirmation.

**Ce flux est DYNAMIQUE et cliquable.** Tout ce qui est un choix passe par l'outil de questions interactives (`AskUserQuestion`) : Morgan **clique** des options et n'a jamais à retaper. Chaque question offre automatiquement un champ libre « Autre » pour écrire sa propre réponse — ne jamais ajouter d'option manuelle « écris le tien », c'est déjà là. Seule exception : la toute première étape (l'idée elle-même) se tape dans le chat, c'est le texte brut.

## Variables fixes

- VAULT : `/home/morgan/vault`
- DOSSIER IDÉES : `/home/morgan/vault/07_idees`
- Rangement : **tags, un seul dossier** (pas de sous-dossiers). Catégorie = tag.

## Règle d'or

- **Le texte brut de Morgan est intact.** Il va tel quel dans la section `## 💡 L'idée`, mot pour mot, jamais reformulé.
- Les suggestions de l'IA vivent **uniquement** dans `## 🔧 Pistes & annotations` et **seulement si Morgan les valide**.
- N'utilise **que** les questions du flux ci-dessous. Aucune question de confort en plus.

## Flux

### Étape 0 — Ouvrir

Affiche exactement, puis attends (frappe libre dans le chat) :

```
💡 Balance ton idée (une phrase ou deux suffisent).
```

Le texte qu'il donne = **idée brute**, à conserver mot pour mot.

### Étape 1 — Carte dynamique (un seul appel `AskUserQuestion`, 3 questions)

Dès qu'il a écrit son idée, lance **un seul appel** `AskUserQuestion` regroupant ces 3 questions. Il clique, une question après l'autre.

1. **Titre** — question : `"Quel titre ?"`, `header: "Titre"`, choix simple. Propose 3 titres **propres et cohérents avec le sujet** de l'idée. Ne **jamais** compresser mécaniquement les phrases de Morgan (ça sonne faux) : reformule le fond en un titre lisible, sous 3 angles différents. Ne pas ajouter d'option « écris le tien » — le champ « Autre » est automatique.
2. **Catégories** — question : `"Dans quelle(s) catégorie(s) ?"`, `header: "Tags"`, `multiSelect: true`. Propose 2 à 4 tags (kebab-case, sans `#`). **Règle tag** : un tag doit **discriminer** — servir à retrouver un sous-ensemble d'idées. Interdits : les tags qui s'appliquent à *tout* (`obsidian`, `capture`, `note`, `idee`…) : ils ne trient rien. Reste sur des tags de **sujet** cohérents, en réutilisant le vocabulaire du vault quand c'est logique (ex. `n8n`, `rag`, `automatisation`, `logistique`, `perso`, `alphanes`, `formation`, `skill`). Le tag `idee` est ajouté d'office à la création, ne pas le proposer.
3. **Pistes** — question : `"Des pistes à ajouter ?"`, `header: "Pistes"`, `multiSelect: true`. Propose 2 à 3 améliorations/annotations brèves (le `label` = la piste). Ajoute une dernière option `label: "Aucune, garder l'idée seule"`. Formule sans menace : ce que Morgan coche va dans la fiche, le reste est simplement ignoré (ne pas écrire « est jeté » dans la question).

Chaque option porte une `description` d'une ligne qui explique le choix.

**Champ « Autre » = virgule sépare.** Dès qu'une réponse « Autre » contient des virgules (surtout pour les tags), découpe sur la virgule → une valeur par élément, chacune nettoyée (trim, kebab-case pour les tags). Ex. `cours, capture rapide` → `cours` + `capture-rapide`. Les cases cochées et le texte « Autre » se **cumulent**, rien n'est écrasé.

### Étape 2 — Validation (un appel `AskUserQuestion`, 1 question)

Fais un bref récap en une ligne (Titre retenu · tags · nb de pistes gardées), puis un `AskUserQuestion` `header: "Valider"` avec les options :

- `Créer la fiche` — écrit la note maintenant.
- `Revenir sur le titre`
- `Revenir sur les tags`
- `Revenir sur les pistes`

Si « Revenir sur X » : relance **uniquement** la question X (un `AskUserQuestion` d'une seule question), puis reviens à cette validation. Ne repose jamais tout le bloc.

### Étape 3 — Création

Quand Morgan choisit « Créer la fiche » :

1. Date du jour — **exclusivement** via :
   ```bash
   date +%F
   ```
2. Slug du titre retenu : minuscules, accents retirés, espaces → `-`, ponctuation retirée.
3. **Anti-doublon** : liste `07_idees/` et vérifie qu'aucune fiche ne couvre déjà le sujet. Si un slug identique existe, suffixe `-2`, `-3`… S'il existe une fiche visiblement sur le même sujet, signale-le et demande (via `AskUserQuestion`) si on complète l'existante plutôt que d'en créer une.
4. Crée le dossier si besoin :
   ```bash
   mkdir -p /home/morgan/vault/07_idees
   ```
5. Écris `/home/morgan/vault/07_idees/<slug>.md` avec ce gabarit :

   ```markdown
   ---
   type: idee
   date: <YYYY-MM-DD>
   statut: brute
   tags:
     - idee
     - <tag1>
     - <tag2>
   ---

   # <Titre retenu>

   ## 💡 L'idée

   <texte brut de Morgan, mot pour mot, non modifié>

   ## 🔧 Pistes & annotations

   <- puces des pistes cochées ; si "Aucune", laisse la section vide (juste le titre)>
   ```

6. Confirme en une ligne avec le chemin, ex. :
   ```
   ✅ Créée : 07_idees/<slug>.md — tags : idee, <tag1>, <tag2>
   ```

## Notes

- Toujours **Write** direct sur le fichier neuf (pas de Templater : il ne se déclenche que via l'UI Obsidian).
- Ne crée pas d'index/README dans `07_idees/` (règle vault : pas d'INDEX.md, tout se retrouve par tags/Dataview).
- Si l'idée est déjà longue et détaillée, propose quand même 3 titres (le clic reste rapide) — ne bascule pas en texte statique.
