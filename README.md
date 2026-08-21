# EmerPlan

Tableau de bord et plans d'urgence interactifs (flotte Boeing 777), en lecture seule une fois publiés.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | Tableau de bord public (accès aux plans) |
| `plan.html` | Visionneuse en lecture seule (zoom, clic sur un numéro → surbrillance) |
| `editeur.html` | Éditeur **local** : définir zones, chemins et rectangles puis exporter le JSON |
| `plans/777-200/` `plans/777-300/` | Un dossier par appareil |

## Structure des plans (plusieurs sections par appareil)

```
plans/777-200/
  sections.json        -> [{ "id": "fwd", "name": "Avant (FWD)" }, { "id": "aft", "name": "Arrière (AFT)" }]
  fwd/
    plan.png           -> image du plan
    donnees.json       -> éléments interactifs
  aft/
    plan.png
    donnees.json
```

S'il y a plusieurs sections, `plan.html?model=777-200` affiche un choix de sections. Une seule section = pas de `sections.json` (accès direct).

## Intégrer un plan

Pour chaque appareil (ex. `777-200`), déposer dans `plans/777-200/` :

1. `plan.png` — l'image du plan (export du scan/PDF en PNG, 200 dpi recommandé)
2. `donnees.json` — les éléments interactifs, au format :

```json
{
  "1": {
    "color": "#e6194B",
    "hotspots": [{ "x": 612, "y": 1307 }],
    "paths": [[{ "x": 0, "y": 0 }, { "x": 100, "y": 80 }]],
    "rects": [{ "x1": 54, "y1": 1307, "x2": 1728, "y2": 2003 }]
  }
}
```

- `hotspots` : points cliquables (numéros du plan) ; toutes les zones d'un même groupe s'allument ensemble.
- `paths` : chemins (fils) liés au groupe, affichés en surbrillance.
- `rects` : zones rectangulaires liées au groupe.
- Les coordonnées sont en pixels de l'image `plan.png`.

Pour créer ce JSON : ouvrir `editeur.html` dans un navigateur, charger l'image dans le dossier local, tracer les éléments, puis **Exporter** (fichier `plan_interactif.json` → renommer en `donnees.json`).

> La visionneuse doit être servie par un serveur HTTP (GitHub Pages ou serveur local) ; ouvrir `plan.html` par double-clic ne charge pas le JSON (sécurité des navigateurs).

## Publication (GitHub Pages)

1. Créer un dépôt sur GitHub et y pousser ce dossier.
2. `Settings` → `Pages` → Source : `Deploy from a branch` → branche `main`, dossier `/ (root)` → Save.
3. L'URL publique est `https://<utilisateur>.github.io/<dépôt>/`.

Le site publié est statique : aucune modification n'est possible par les visiteurs.
