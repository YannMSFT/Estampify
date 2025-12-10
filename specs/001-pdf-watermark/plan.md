# Implementation Plan: Application de filigrane PDF

**Branch**: `001-pdf-watermark` | **Date**: 2025-12-04 | **Spec**: [spec.md](../../.specify/specs/001-pdf-watermark/spec.md)
**Input**: Feature specification from `.specify/specs/001-pdf-watermark/spec.md`

## Summary

Application web standalone permettant d'ajouter des filigranes personnalisables (texte, taille, opacité, couleur, rotation, répétition) à des fichiers PDF. Traitement 100% client-side avec PDF-lib.js, packagé dans un seul fichier HTML autonome.

## Technical Context

**Language/Version**: JavaScript ES6+ (Vanilla)  
**Primary Dependencies**: PDF-lib.js v1.17.1 (bundled inline via CDN with integrity hash)  
**Storage**: N/A (in-memory only, no persistence)  
**Testing**: Manuel (navigateur) - pas de framework de test requis pour MVP  
**Target Platform**: Navigateurs web modernes (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)  
**Project Type**: Single-file standalone HTML  
**Performance Goals**: Traitement PDF 100 pages < 30s, aperçu temps réel < 100ms  
**Constraints**: Fichier unique < 500KB, offline-capable après chargement, max 50MB PDF, max 500 pages  
**Scale/Scope**: Application mono-utilisateur locale, 1 fichier HTML, ~1500 lignes de code

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principe | Statut | Vérification |
|----------|--------|--------------|
| **I. Client-Side First** (NON-NEGOTIABLE) | ✅ PASS | Aucun appel serveur, PDF-lib.js traite tout en mémoire navigateur |
| **II. Single-File Architecture** | ✅ PASS | HTML unique avec CSS/JS inline, PDF-lib.js via CDN |
| **III. Progressive Enhancement** | ✅ PASS | Vanilla JS, pas de framework, fonctionnalités de base garanties |
| **IV. User Experience Simplicity** | ✅ PASS | Workflow en 5 clics max (spec SC-001) |
| **V. Defensive Processing** | ✅ PASS | Validations définies (FR-003 à FR-005, limites explicites) |

**Privacy & Security Compliance**:
- ✅ Documents restent en mémoire navigateur uniquement
- ✅ Pas d'analytics/tracking/telemetry
- ✅ PDF-lib.js chargé avec integrity hash
- ✅ Garbage collection après téléchargement

**GATE RESULT**: ✅ PASS - Aucune violation constitutionnelle

## Project Structure

### Documentation (this feature)

```text
specs/001-pdf-watermark/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
└── contracts/           # N/A (pas d'API backend)
```

### Source Code (repository root)

```text
estampify-standalone.html    # Application complète (fichier unique autonome)
```

**Structure Decision**: Architecture single-file. Tout le code (HTML structure, CSS styles, JavaScript logic) est contenu dans un seul fichier HTML. Cette approche respecte le principe constitutionnel "Single-File Architecture" et maximise la portabilité.

**Modules logiques internes** (sections dans le fichier HTML):
- `<style>`: CSS pour l'interface (variables, layout, composants)
- `<script>` PDF-lib.js: Bibliothèque de manipulation PDF (CDN inline)
- `<script>` App: Logique applicative organisée en modules:
  - `FileHandler`: Upload, validation, lecture de fichiers
  - `WatermarkConfig`: Gestion de la configuration du filigrane
  - `PreviewRenderer`: Rendu de l'aperçu temps réel
  - `PDFProcessor`: Application du filigrane et génération du PDF
  - `UIController`: Gestion de l'interface et des événements

## Complexity Tracking

> Aucune violation constitutionnelle - section non requise.

---

## Post-Design Constitution Re-Check

*Re-evaluation après Phase 1 (data-model.md, quickstart.md)*

| Principe | Statut Post-Design | Notes |
|----------|-------------------|-------|
| **I. Client-Side First** | ✅ CONFIRMED | data-model.md confirme: ArrayBuffer en mémoire, pas de storage externe |
| **II. Single-File Architecture** | ✅ CONFIRMED | quickstart.md confirme: fichier unique, modules internes logiques |
| **III. Progressive Enhancement** | ✅ CONFIRMED | Vanilla JS uniquement, PDF-lib.js comme seule dépendance |
| **IV. User Experience Simplicity** | ✅ CONFIRMED | 4 étapes documentées dans quickstart.md |
| **V. Defensive Processing** | ✅ CONFIRMED | data-model.md définit toutes les validations par entité |

**FINAL GATE RESULT**: ✅ PASS - Design conforme à la constitution

---

## Artifacts Generated

| Phase | Artifact | Status |
|-------|----------|--------|
| Phase 1 | `research.md` | ✅ Complete |
| Phase 1 | `data-model.md` | ✅ Complete |
| Phase 1 | `quickstart.md` | ✅ Complete |
| Phase 1 | `contracts/` | N/A (pas d'API) |
| Phase 1 | Agent context | ✅ Updated |
| Phase 2 | `tasks.md` | ✅ Complete (69 tasks) |
