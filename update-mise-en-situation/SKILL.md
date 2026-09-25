---
name: update-mise-en-situation
description: Quand une conversation de mise en situation (workflow n8n) devient trop longue, produit un fichier d'avancement propre à réinjecter dans une nouvelle conversation — sans tout réexpliquer. Un dossier par mise en situation. Trigger : "/update-mise-en-situation", "mets à jour la mise en situation".
---

# update-mise-en-situation — Passer le relais sans réexpliquer

## Le besoin

Morgan fait une mise en situation (construire un workflow n8n) dans une conversation claude.ai. Quand ça devient trop long, il change de conversation. Cette skill capte l'avancement dans un fichier propre : il colle la conversation ici, lance la commande, récupère un `Avancée.md` à réinjecter. Fini le réexpliquer à chaque fois.

## Variables fixes

- VAULT : `/home/morgan/vault`
- DOSSIER : `/home/morgan/vault/03_knowledge/formations/Mise en situation`

## 🔴 Anti-mélange (règles dures)

- **Un dossier par mise en situation**, nommé par son **titre exact**. Jamais deux situations dans le même dossier.
- **Deux fichiers séparés** : `Énoncé.md` (le sujet, fixe — on le lit, on n'y touche pas) et `Avancée.md` (l'instantané, réécrit à chaque run).
- **Uniquement ce qui est fourni** (conversation collée + screens). Ne **jamais inventer**, ne **jamais fusionner** deux situations. Si deux infos se contredisent, le **signaler**, ne pas trancher.

## Étapes

### 0 — Donner le prompt de récap (au lancement)

Dès que la commande est lancée, **fournis à Morgan ce prompt prêt à coller**, pour qu'il génère le récap de l'état du workflow dans sa conversation claude.ai :

```
Fais-moi un récap dense de cette mise en situation, sans blabla, pour que je le reprenne dans une autre conversation :

1. Le workflow n8n tel qu'il est maintenant : les nodes dans l'ordre, avec le rôle de chacun (1 à 3 phrases par node).
2. Les points importants et les décisions qu'on a prises.
3. Où on en est et la prochaine étape.

Écris-le ici, en texte.
```

Puis enchaîne sur l'étape 1 (titre) et attends qu'il te colle le récap (+ screens éventuels).

### 1 — Titre

**Demande le titre à Morgan** (ex. « Klaviyo »). Ne cherche pas à le déduire : le dossier peut être vide ou inexistant, donc il n'y a rien à déduire — c'est plus simple et sans ambiguïté de le demander. Ce titre = **nom du dossier**.

### 2 — Chemin (déduit du titre)

`Mise en situation/<Titre>/`.

- **Le dossier existe** → on met à jour. Lis `Énoncé.md` (contexte) et ouvre `Avancée.md`.
- **Le dossier n'existe pas** (première fois) → on crée l'`Énoncé.md`.

  **Règle de création de l'énoncé :** Morgan l'envoie en **plusieurs messages séparés**, dans l'ordre.
  - **N'écris rien tant qu'il n'a pas dit que c'est complet.** On attend.
  - **Ne re-transcris pas** le contenu au fil de l'eau dans le chat — il ira dans le fichier, que Morgan vérifiera lui-même. Accuse juste réception brièvement.
  - Garde les messages **dans l'ordre**.
  - Au signal de fin : crée `<Titre>/` + `<Titre>/attachments/`, écris `Énoncé.md` (transcription **fidèle**, dans l'ordre), puis on passe à l'`Avancée.md`.

### 3 — Screens (si fournis)

Si Morgan donne des images du workflow :
- lis-les (le canvas n8n = **source de vérité de l'état réel**),
- copie-les dans `<Titre>/attachments/`,
- référence-les datées dans `Avancée.md`.

### 4 — Écrire `Avancée.md` (instantané réécrit, on n'empile pas)

Date via `date +%F` (jamais de mémoire). Structure :

```markdown
# <Titre> — Avancée

Maj : <YYYY-MM-DD>

## Le workflow (nodes dans l'ordre)

1. <node> — <rôle : 1 à 3 phrases, autant qu'il en faut pour être clair et juste. Ne pas sacrifier la qualité pour la brièveté.>
2. …

(le dernier node = là où Morgan en est)

## Points importants

- <les décisions, choix et insights clés dits dans la conversation, utiles pour la suite. Pas un changelog des ajouts/retraits — la substance.>

## Prochaine étape

<écrite par toi, pour toi, sans consulter Morgan>

## Screens

- <YYYY-MM-DD> : ![[attachments/<fichier>.png]]
```

### 5 — Comportement (important)

- **N'interroge pas Morgan** à chaque fois (« as-tu fait ci, as-tu fait ça »). Il **avance parfois seul** entre deux sessions, c'est **normal** — pas une faute. Un node vu au screen mais absent de la conversation n'est **pas** « posé seul » : intègre-le simplement.
- **Assume** qu'il a pu progresser de son côté ; **intègre** ce qu'il te donne (conversation + screens). Il te dira ce qui compte.
- La ligne « Prochaine étape », tu l'écris **pour toi**, sans lui demander. Lui écrit la sienne de son côté.

## Sortie

- Vire le superflu, garde la qualité.
- À la fin, dis **juste** le chemin de `Avancée.md`. Rien d'autre.
