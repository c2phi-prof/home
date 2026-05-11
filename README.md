# Dashboard

Dashboard personnel hébergé sur GitHub Pages. Interface canvas : une barre de navigation, des colonnes configurables, des boxes iframe.

---

## Structure des fichiers

```
├── index.html          ← shell principal (ne jamais modifier)
├── config.json         ← configuration des pages et boxes
└── widgets/
    ├── exemple.html    ← template de widget (à dupliquer)
    ├── horloge.html    ← widget horloge
    ├── notes.html      ← widget notes (localStorage)
    └── liens.html      ← widget liens rapides
```

---

## Déploiement sur GitHub Pages

1. Créer un dépôt GitHub (public ou privé avec GitHub Pro)
2. `Settings → Pages → Source : Deploy from a branch → main / root`
3. Le site sera accessible à `https://votre-nom.github.io/nom-du-repo/`

---

## Ajouter une page

Dans `config.json`, ajouter un objet dans le tableau `pages` :

```json
{
  "id":         "ma-page",
  "label":      "Ma page",
  "background": "#1a1a2e",
  "gap":        "10px",
  "padding":    "10px",
  "columns": [ ... ]
}
```

| Paramètre    | Type     | Défaut        | Description                      |
|:-------------|:---------|:--------------|:---------------------------------|
| `id`         | string   | —             | Identifiant unique (sans espace) |
| `label`      | string   | —             | Texte affiché dans le menu       |
| `background` | string   | transparent   | Couleur CSS de fond de la page   |
| `gap`        | string   | `"8px"`       | Espacement entre colonnes        |
| `padding`    | string   | `"8px"`       | Marge autour de la grille        |

---

## Configurer les colonnes

Chaque page contient un tableau `columns`. La largeur des colonnes utilise directement les valeurs CSS Grid :

```json
"columns": [
  { "width": "260px", ... },
  { "width": "1fr",   ... },
  { "width": "2fr",   ... }
]
```

Exemples de valeurs pour `width` :
- `"300px"` — largeur fixe
- `"30%"`   — pourcentage
- `"1fr"`   — espace restant (équivalent à `flex: 1`)
- `"2fr"`   — double de `1fr`
- `"minmax(200px, 1fr)"` — toute valeur CSS Grid valide

| Paramètre | Type   | Défaut       | Description                     |
|:----------|:-------|:-------------|:--------------------------------|
| `width`   | string | `"1fr"`      | Largeur CSS Grid de la colonne  |
| `gap`     | string | gap de page  | Espacement entre les boxes      |
| `boxes`   | array  | —            | Liste des boxes dans la colonne |

---

## Ajouter ou modifier une box

### 1. Créer le fichier widget

Sur GitHub : **Add file → Create new file** → nommez-le `widgets/mon-widget.html`

Copiez le contenu de `widgets/exemple.html` comme point de départ.

### 2. Référencer dans config.json

```json
{
  "src":          "widgets/mon-widget.html",
  "height":       250,
  "background":   "#1c1c1c",
  "borderRadius": "4px",
  "scroll":       true
}
```

| Paramètre      | Type    | Défaut  | Description                               |
|:---------------|:--------|:--------|:------------------------------------------|
| `src`          | string  | —       | Chemin vers le fichier widget             |
| `height`       | number  | `200`   | Hauteur de l'iframe en pixels             |
| `background`   | string  | transp. | Couleur de fond derrière le widget        |
| `borderRadius` | string  | `"4px"` | Rayon des coins de la box                 |
| `scroll`       | boolean | `true`  | Active/désactive le scroll dans la box    |

---

## Créer un widget

Un widget est un fichier HTML autonome. Règles importantes :

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: transparent; /* ← obligatoire pour que la couleur de config s'affiche */
    padding: 12px;
  }
</style>
</head>
<body>
  <!-- Votre contenu ici -->
<script>
  /* Votre JS ici — tout est permis : fetch, localStorage, etc. */
</script>
</body>
</html>
```

**Ce qui fonctionne dans un widget :**
- `localStorage` / `sessionStorage`
- `fetch()`, `WebSocket`
- Toutes les APIs Web standard
- Import de librairies via CDN

**Règle unique :** `body { background: transparent }` — la couleur de fond est définie dans `config.json`.

---

## Modifier la barre de navigation

Dans `config.json`, bloc `nav` :

```json
"nav": {
  "background":   "#0a0a0a",
  "color":        "#444444",
  "hoverColor":   "#888888",
  "activeColor":  "#dddddd"
}
```

---

## Workflow GitHub au quotidien

| Action                  | Fichiers à modifier              |
|:------------------------|:---------------------------------|
| Modifier le contenu d'une box | `widgets/mon-widget.html` uniquement |
| Ajouter une box         | Créer `widgets/x.html` + ajouter dans `config.json` |
| Changer les colonnes    | `config.json` uniquement         |
| Changer le fond d'une page | `config.json` uniquement      |
| Ajouter une page        | `config.json` uniquement         |

> `index.html` n'a jamais besoin d'être modifié.
