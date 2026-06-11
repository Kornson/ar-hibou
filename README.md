# AR Hibou — DMH & Associés

Expérience de réalité augmentée web : un hibou 3D apparaît au-dessus d'une carte physique, directement dans le navigateur mobile. Aucune application à installer.

## Stack

- **AR.js 2.3.4** — détection de marqueur via webcam
- **Three.js 0.150** — rendu 3D + chargement .glb
- **Draco** — compression du modèle 3D (34 Mo → 956 Ko)

## Structure

```
ar-hibou/
├── index.html          ← application principale
├── models/
│   └── hibou-final.glb ← modèle 3D compressé
├── markers/
│   ├── hiro-preview.png    ← image du marqueur Hiro (test)
│   └── carte.patt          ← marqueur de la vraie carte (prod)
├── vercel.json
└── README.md
```

## Déploiement Vercel

```bash
npm i -g vercel
vercel --prod
```

Le projet est statique, aucun build nécessaire.

## Phase test (marqueur Hiro)

Le projet utilise le marqueur **Hiro** par défaut (marqueur standard AR.js).

Imprimez ou affichez ce marqueur sur écran :
👉 https://raw.githubusercontent.com/AR-js-org/AR.js/master/data/images/hiro.png

## Passage en production (marqueur carte)

Quand l'image de la carte est disponible :

1. Générez les fichiers `.patt` sur : https://ar-js-org.github.io/AR.js/three.js/examples/marker-training/examples/get-marker.html
2. Placez `carte.patt` dans `/markers/`
3. Dans `index.html`, changez :
   ```js
   const MARKER_TYPE = 'pattern'; // était 'hiro'
   ```
4. Redéployez : `vercel --prod`

### Recommandations pour l'image de la carte
- Fort contraste, riche en détails (logos, texte)
- Évitez les zones uniformes ou très symétriques
- Format PNG, 500–800px de large, fond blanc net

## Ajustements du modèle

Dans `index.html`, trois constantes à adapter selon rendu réel :

```js
const MODEL_SCALE = 0.6;   // taille du hibou (augmenter si trop petit)
const MODEL_Y     = 0;     // hauteur au-dessus de la carte
```

## Compatibilité

| Navigateur | Support |
|---|---|
| iOS Safari 14+ | ✅ |
| Android Chrome 80+ | ✅ |
| Desktop Chrome/Firefox | ✅ (test) |

> HTTPS obligatoire en production pour l'accès caméra. Vercel le fournit automatiquement.

## Ajouter des animations

Le modèle actuel (Meshy AI) ne contient pas d'animations riggées.
Une animation idle (flottement + légère rotation) est appliquée par défaut via Three.js.

Pour des animations riggées (battement d'ailes) :
- Importer le .glb dans Blender
- Rigger + animer
- Réexporter en .glb avec animations
- Le code détecte automatiquement les animations et les joue via `AnimationMixer`
