# Data Model: Application de filigrane PDF

**Feature**: 001-pdf-watermark  
**Date**: 2025-12-04  
**Status**: Complete

## Entities

### 1. PDFDocument

Représente le fichier PDF source chargé par l'utilisateur.

| Attribut | Type | Description | Validation |
|----------|------|-------------|------------|
| `file` | `File` | Objet File du navigateur | Required |
| `name` | `string` | Nom du fichier original | Extrait de file.name |
| `size` | `number` | Taille en bytes | ≤ 52,428,800 (50 MB) |
| `pageCount` | `number` | Nombre de pages | ≤ 500 |
| `arrayBuffer` | `ArrayBuffer` | Contenu binaire du PDF | Chargé async |
| `pdfDoc` | `PDFDocument` | Instance PDF-lib.js | Après parsing |

**État du cycle de vie**:
```
[empty] → [loading] → [loaded] → [processing] → [complete]
                  ↘ [error]
```

**Règles de validation**:
- MIME type doit être `application/pdf`
- Taille ≤ 50 MB (vérifiée AVANT lecture)
- Nombre de pages ≤ 500 (vérifiée APRÈS parsing)

---

### 2. WatermarkConfig

Configuration du filigrane à appliquer.

| Attribut | Type | Défaut | Validation |
|----------|------|--------|------------|
| `text` | `string` | `""` | 1-150 caractères (200 avec template) |
| `useTemplate` | `boolean` | `false` | - |
| `templateName` | `string` | `""` | Si useTemplate, max 50 caractères |
| `fontSize` | `number` | `20` | 10-100 |
| `opacity` | `number` | `0.3` | 0.1-1.0 |
| `color` | `string` | `"#000000"` | Format hex valide |
| `rotation` | `number` | `45` | Fixé (non modifiable) |
| `repeat` | `boolean` | `false` | - |

**Texte effectif** (calculé):
```javascript
get effectiveText() {
  if (this.useTemplate && this.templateName) {
    return `Document exclusivement destiné à l'usage de ${this.templateName}`;
  }
  return this.text;
}
```

**Validation couleur**:
```javascript
const isValidHex = /^#[0-9A-Fa-f]{6}$/.test(color);
```

---

### 3. WatermarkStyle

Représentation interne du style calculé pour le rendu PDF.

| Attribut | Type | Description |
|----------|------|-------------|
| `rgb` | `{r, g, b}` | Couleur normalisée (0-1) |
| `font` | `PDFFont` | Police PDF-lib.js (Helvetica) |
| `rotationRadians` | `number` | Rotation en radians |

**Calcul**:
```javascript
{
  rgb: hexToRgb(config.color),
  font: StandardFonts.Helvetica,
  rotationRadians: (config.rotation * Math.PI) / 180
}
```

---

### 4. ProcessingState

État du traitement en cours.

| Attribut | Type | Description |
|----------|------|-------------|
| `status` | `enum` | `idle` \| `processing` \| `complete` \| `error` |
| `currentPage` | `number` | Page en cours de traitement |
| `totalPages` | `number` | Nombre total de pages |
| `progress` | `number` | Pourcentage (0-100) |
| `errorMessage` | `string?` | Message d'erreur si status=error |

**Calcul progression**:
```javascript
progress = Math.round((currentPage / totalPages) * 100);
```

---

### 5. AppState

État global de l'application (singleton).

| Attribut | Type | Description |
|----------|------|-------------|
| `document` | `PDFDocument?` | Document chargé (null si aucun) |
| `config` | `WatermarkConfig` | Configuration actuelle |
| `processing` | `ProcessingState` | État du traitement |

---

## State Transitions

### Document Loading

```
User drops/selects file
    ↓
Validate MIME type → [error: "Format non supporté"]
    ↓ (valid)
Validate size → [error: "Fichier trop volumineux (max 50 MB)"]
    ↓ (valid)
Read as ArrayBuffer (async)
    ↓
Parse with PDF-lib.js
    ↓
Validate page count → [error: "Trop de pages (max 500)"]
    ↓ (valid)
[loaded] - Display preview
```

### Watermark Application

```
User clicks "Appliquer"
    ↓
Validate config.effectiveText not empty → [error: "Texte requis"]
    ↓ (valid)
[processing] status=processing, currentPage=0
    ↓
For each page:
    ├─ Apply watermark(s)
    ├─ Update currentPage++
    └─ Update progress UI
    ↓
Generate PDF bytes
    ↓
Trigger download
    ↓
[complete] - Reset state, release memory
```

---

## Relationships

```
┌─────────────┐     1:1     ┌──────────────────┐
│ PDFDocument │────────────▶│ ProcessingState  │
└─────────────┘             └──────────────────┘
       │
       │ uses
       ▼
┌─────────────────┐   generates   ┌─────────────────┐
│ WatermarkConfig │──────────────▶│ WatermarkStyle  │
└─────────────────┘               └─────────────────┘
```

---

## UI Data Binding

| UI Element | Bound To | Update Trigger |
|------------|----------|----------------|
| Zone drop | `document.status` | File drop/select |
| Nom fichier | `document.name` | Document loaded |
| Input texte | `config.text` | User input |
| Checkbox template | `config.useTemplate` | User click |
| Input nom | `config.templateName` | User input |
| Slider taille | `config.fontSize` | User drag |
| Slider opacité | `config.opacity` | User drag |
| Color picker | `config.color` | User pick |
| Checkbox répétition | `config.repeat` | User click |
| Preview canvas | `config.*` | Any config change |
| Progress bar | `processing.progress` | Page processed |
| Bouton Appliquer | `document != null && config.effectiveText` | State change |
