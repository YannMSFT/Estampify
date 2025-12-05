# Quickstart: Stampify - Application de filigrane PDF

**Feature**: 001-pdf-watermark  
**Date**: 2025-12-04

## Prérequis

- Un navigateur web moderne (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- C'est tout ! Aucune installation requise.

## Démarrage rapide

### Option 1: Utilisation directe (recommandée)

1. **Télécharger** le fichier `stampify-standalone.html`
2. **Double-cliquer** sur le fichier pour l'ouvrir dans votre navigateur
3. L'application est prête à l'emploi !

### Option 2: Serveur local (développement)

```bash
# Cloner le repository
git clone https://github.com/YannMSFT/Stampify.git
cd Stampify

# Servir avec Python
python -m http.server 8000

# Ou avec Node.js
npx serve

# Ouvrir dans le navigateur
open http://localhost:8000/stampify-standalone.html
```

## Workflow utilisateur

### Étape 1: Charger un PDF

- **Glisser-déposer** un fichier PDF dans la zone centrale
- Ou **cliquer** sur "Parcourir" pour sélectionner un fichier

**Limites**:
- Taille maximale: 50 MB
- Nombre de pages maximum: 500

### Étape 2: Configurer le filigrane

| Paramètre | Description | Valeurs |
|-----------|-------------|---------|
| **Texte** | Texte du filigrane | 1-150 caractères |
| **Modèle** | Utiliser le template prédéfini | Cocher + saisir nom |
| **Taille** | Taille de police | 10-100 |
| **Opacité** | Transparence | 10%-100% |
| **Couleur** | Couleur du texte | Sélecteur couleur |
| **Répétition** | Couvrir toute la page | Cocher/décocher |

**Modèle prédéfini**:
Génère automatiquement: `Document exclusivement destiné à l'usage de [Nom saisi]`

### Étape 3: Prévisualiser

L'aperçu se met à jour **en temps réel** lors des modifications.

### Étape 4: Appliquer et télécharger

1. Cliquer sur **"Appliquer le filigrane"**
2. Suivre la progression (Page X sur Y)
3. Le téléchargement démarre automatiquement
4. Le fichier est nommé `[original]_watermarked.pdf`

## Structure du code

```html
stampify-standalone.html
├── <head>
│   └── <style>           # CSS (variables, layout, composants)
├── <body>
│   ├── <header>          # Logo et titre
│   ├── <main>
│   │   ├── .upload-zone  # Zone de drop
│   │   ├── .config-panel # Contrôles de configuration
│   │   ├── .preview      # Canvas d'aperçu
│   │   └── .actions      # Bouton appliquer + progression
│   └── <footer>          # Crédits
└── <script>
    ├── PDF-lib.js        # Bibliothèque (CDN)
    └── App               # Logique applicative
        ├── FileHandler
        ├── WatermarkConfig
        ├── PreviewRenderer
        ├── PDFProcessor
        └── UIController
```

## API interne (modules JavaScript)

### FileHandler

```javascript
// Charger et valider un fichier PDF
async function loadFile(file) → Promise<PDFDocument>
function validateFile(file) → { valid: boolean, error?: string }
```

### WatermarkConfig

```javascript
// Gérer la configuration du filigrane
function getConfig() → WatermarkConfig
function updateConfig(partial) → void
function getEffectiveText() → string
```

### PreviewRenderer

```javascript
// Rendre l'aperçu sur canvas
async function renderPreview(pdfDoc, config) → void
function updateWatermarkOverlay(config) → void
```

### PDFProcessor

```javascript
// Appliquer le filigrane et générer le PDF
async function applyWatermark(pdfDoc, config, onProgress) → Uint8Array
function triggerDownload(bytes, filename) → void
```

### UIController

```javascript
// Orchestrer l'UI
function init() → void
function bindEvents() → void
function updateUI(state) → void
function showError(message) → void
```

## Tests manuels

### Test 1: Upload basique
1. Ouvrir l'application
2. Charger un PDF de test
3. ✅ L'aperçu s'affiche

### Test 2: Configuration filigrane
1. Charger un PDF
2. Modifier le texte → ✅ Aperçu se met à jour
3. Modifier la taille → ✅ Aperçu se met à jour
4. Modifier l'opacité → ✅ Aperçu se met à jour
5. Modifier la couleur → ✅ Aperçu se met à jour

### Test 3: Génération
1. Charger un PDF multi-pages
2. Configurer un filigrane
3. Cliquer "Appliquer"
4. ✅ Progression affichée
5. ✅ Téléchargement automatique
6. ✅ Ouvrir le PDF → filigrane sur toutes les pages

### Test 4: Validation erreurs
1. Charger un fichier non-PDF → ✅ Message d'erreur
2. Charger un PDF > 50MB → ✅ Message d'erreur
3. Laisser texte vide → ✅ Bouton désactivé

## Troubleshooting

| Problème | Solution |
|----------|----------|
| "Format non supporté" | Vérifier que le fichier est un PDF valide |
| "Fichier trop volumineux" | Réduire la taille du PDF (max 50 MB) |
| "Trop de pages" | Utiliser un PDF de moins de 500 pages |
| Aperçu ne s'affiche pas | Actualiser la page, essayer un autre navigateur |
| Téléchargement bloqué | Vérifier les paramètres de téléchargement du navigateur |

## Confidentialité

- ✅ **100% local**: Aucune donnée envoyée au serveur
- ✅ **Pas de tracking**: Aucun analytics ou cookie
- ✅ **Mémoire libérée**: Documents supprimés après téléchargement
