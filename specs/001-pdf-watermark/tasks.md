# Tasks: Application de filigrane PDF

**Input**: Design documents from `/specs/001-pdf-watermark/`  
**Prerequisites**: plan.md ✅, spec.md ✅, research.md ✅, data-model.md ✅, quickstart.md ✅

**Tests**: Non demandés dans la spécification - tests manuels uniquement (voir quickstart.md)

**Organization**: Tasks grouped by user story for independent implementation and testing.

## Format: `[ID] [P?] [Story?] Description`

- **[P]**: Can run in parallel (different sections, no dependencies)
- **[Story]**: User story label (US1, US2, US3, US4, US5)
- File path: `estampify-standalone.html` (single-file architecture)

## Path Conventions

**Single-file project**: Tout le code dans `estampify-standalone.html`
- Sections logiques: `<style>`, `<body>`, `<script>`
- Modules JS: FileHandler, WatermarkConfig, PreviewRenderer, PDFProcessor, UIController

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Créer la structure HTML de base et charger les dépendances

- [x] T001 Create HTML boilerplate with doctype, head, meta tags in estampify-standalone.html
- [x] T002 Add PDF-lib.js CDN script with integrity hash in estampify-standalone.html
- [x] T003 [P] Create CSS variables for colors, spacing, typography in estampify-standalone.html `<style>`
- [x] T004 [P] Create base layout structure (header, main, footer) in estampify-standalone.html `<body>`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Infrastructure partagée par toutes les user stories

**⚠️ CRITICAL**: Ces tâches DOIVENT être terminées avant toute user story

- [x] T005 Create AppState singleton object (document, config, processing) in estampify-standalone.html `<script>`
- [x] T006 [P] Create utility function hexToRgb() for color conversion in estampify-standalone.html `<script>`
- [x] T007 [P] Create utility function degrees() for rotation in estampify-standalone.html `<script>`
- [x] T008 Create error display component showError(message) in estampify-standalone.html `<script>`
- [x] T009 Create CSS for error messages (.error-message) in estampify-standalone.html `<style>`
- [x] T009a [P] Add browser feature detection (FileReader, ArrayBuffer, Blob) with fallback message in estampify-standalone.html `<script>`

**Checkpoint**: ✅ Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - Upload et aperçu du PDF (Priority: P1) 🎯 MVP

**Goal**: L'utilisateur peut charger un PDF et voir un aperçu de la première page

**Independent Test**: Ouvrir l'app, glisser-déposer un PDF, vérifier que l'aperçu s'affiche

**Requirements Coverage**: FR-001, FR-002, FR-003, FR-013

### Implementation for User Story 1

- [x] T010 [US1] Create upload zone HTML (.upload-zone with drag-drop styling) in estampify-standalone.html `<body>`
- [x] T011 [US1] Create CSS for upload zone (dashed border, hover states, drag-over) in estampify-standalone.html `<style>`
- [x] T012 [US1] Create hidden file input element for "Parcourir" button in estampify-standalone.html `<body>`
- [x] T013 [US1] Create preview container HTML (.preview with canvas) in estampify-standalone.html `<body>`
- [x] T014 [US1] Create CSS for preview container (centered, responsive) in estampify-standalone.html `<style>`
- [x] T015 [US1] Implement FileHandler.validateFile(file) - check MIME type in estampify-standalone.html `<script>`
- [x] T016 [US1] Implement FileHandler.loadFile(file) - FileReader.readAsArrayBuffer in estampify-standalone.html `<script>`
- [x] T017 [US1] Implement PDFDocument parsing with PDFLib.PDFDocument.load() in estampify-standalone.html `<script>`
- [x] T018 [US1] Implement PreviewRenderer.renderPreview(pdfDoc) - first page to canvas in estampify-standalone.html `<script>`
- [x] T019 [US1] Bind drag-and-drop events (dragover, dragleave, drop) in estampify-standalone.html `<script>`
- [x] T020 [US1] Bind file input change event in estampify-standalone.html `<script>`
- [x] T021 [US1] Display filename after successful load in estampify-standalone.html `<script>`

**Checkpoint**: ✅ User Story 1 complete - PDF can be loaded and previewed

---

## Phase 4: User Story 2 - Configuration du filigrane (Priority: P1) 🎯 MVP

**Goal**: L'utilisateur peut configurer texte, taille, opacité, couleur et mode répétition

**Independent Test**: Modifier les paramètres et observer les changements dans l'aperçu

**Requirements Coverage**: FR-006, FR-008, FR-009, FR-010, FR-011, FR-012, FR-014, FR-015

### Implementation for User Story 2

- [x] T022 [US2] Create config panel HTML (.config-panel) with all controls in estampify-standalone.html `<body>`
- [x] T023 [US2] Create text input for watermark text (maxlength=150) in estampify-standalone.html `<body>`
- [x] T024 [US2] Create range slider for fontSize (5-25, default 15) in estampify-standalone.html `<body>`
- [x] T025 [US2] Create range slider for opacity (0.05-1.0, default 0.2) in estampify-standalone.html `<body>`
- [x] T026 [US2] Create color picker input for watermark color in estampify-standalone.html `<body>`
- [x] T027 [US2] Create checkbox for repeat mode in estampify-standalone.html `<body>`
- [x] T028 [US2] Create CSS for config panel layout (flex, labels, spacing) in estampify-standalone.html `<style>`
- [x] T029 [US2] Create CSS for sliders and inputs (custom styling) in estampify-standalone.html `<style>`
- [x] T030 [US2] Implement WatermarkConfig.getConfig() returning current values in estampify-standalone.html `<script>`
- [x] T031 [US2] Implement WatermarkConfig.updateConfig(partial) in estampify-standalone.html `<script>`
- [x] T032 [US2] Implement PreviewRenderer.updateWatermarkOverlay(config) in estampify-standalone.html `<script>`
- [x] T033 [US2] Bind input events to trigger preview update in estampify-standalone.html `<script>`
- [x] T034 [US2] Display current slider values next to sliders in estampify-standalone.html `<script>`

**Checkpoint**: ✅ User Story 2 complete - watermark configuration updates preview in real-time

---

## Phase 5: User Story 4 - Génération et téléchargement (Priority: P1) 🎯 MVP

**Goal**: L'utilisateur peut appliquer le filigrane à toutes les pages et télécharger le PDF

**Independent Test**: Charger PDF, configurer filigrane, cliquer Appliquer, vérifier le téléchargement

**Requirements Coverage**: FR-016, FR-017, FR-018, FR-019, FR-020, FR-021, FR-022, FR-023, FR-024, FR-025, FR-026

### Implementation for User Story 4

- [x] T035 [US4] Create action button "Appliquer le filigrane" in estampify-standalone.html `<body>`
- [x] T036 [US4] Create progress indicator HTML (.progress-bar, .progress-text) in estampify-standalone.html `<body>`
- [x] T037 [US4] Create CSS for button (primary style, disabled state) in estampify-standalone.html `<style>`
- [x] T038 [US4] Create CSS for progress bar (animated, percentage) in estampify-standalone.html `<style>`
- [x] T039 [US4] Implement PDFProcessor.applyWatermark(pdfDoc, config, onProgress) in estampify-standalone.html `<script>`
- [x] T040 [US4] Implement single watermark drawing with rotation (45°) in estampify-standalone.html `<script>`
- [x] T041 [US4] Implement repeat mode watermark grid calculation in estampify-standalone.html `<script>`
- [x] T042 [US4] Implement progress callback updating UI (Page X sur Y) in estampify-standalone.html `<script>`
- [x] T043 [US4] Implement PDFProcessor.triggerDownload(bytes, filename) in estampify-standalone.html `<script>`
- [x] T044 [US4] Generate output filename as [original]_watermarked.pdf in estampify-standalone.html `<script>`
- [x] T045 [US4] Bind apply button click event in estampify-standalone.html `<script>`
- [x] T046 [US4] Disable button during processing, re-enable after in estampify-standalone.html `<script>`
- [x] T047 [US4] Clear memory references after download (garbage collection) in estampify-standalone.html `<script>`

- [x] T047a [US4] Implement calculateWatermarkPositions() for unified preview/PDF rendering (FR-023) in estampify-standalone.html `<script>`
- [x] T047b [US4] Ensure watermark text is fully readable without truncation (FR-024) in estampify-standalone.html `<script>`
- [x] T047c [US4] Implement regular grid spacing with rotation bounding box (FR-025) in estampify-standalone.html `<script>`
- [x] T047d [US4] Validate no watermark overlap in repeat mode (FR-026) in estampify-standalone.html `<script>`

**Checkpoint**: ✅ User Stories 1, 2, 4 complete - MVP is functional!

---

## Phase 6: User Story 3 - Modèle prédéfini (Priority: P2)

**Goal**: L'utilisateur peut utiliser un template "Document destiné à [Nom]"

**Independent Test**: Activer le modèle, saisir un nom, vérifier le texte généré

**Requirements Coverage**: FR-007

### Implementation for User Story 3

- [x] T048 [US3] Create checkbox "Utiliser le modèle" in estampify-standalone.html `<body>`
- [x] T049 [US3] Create text input for template name (maxlength=50) in estampify-standalone.html `<body>`
- [x] T050 [US3] Create CSS for template section (conditional visibility) in estampify-standalone.html `<style>`
- [x] T051 [US3] Implement WatermarkConfig.getEffectiveText() in estampify-standalone.html `<script>`
- [x] T052 [US3] Toggle visibility of text input vs template input in estampify-standalone.html `<script>`
- [x] T053 [US3] Update preview when template name changes in estampify-standalone.html `<script>`

**Checkpoint**: ✅ User Story 3 complete - template mode works independently

---

## Phase 7: User Story 5 - Gestion des erreurs (Priority: P2)

**Goal**: L'utilisateur voit des messages clairs pour les limites et erreurs

**Independent Test**: Charger un fichier > 50MB, vérifier le message d'erreur

**Requirements Coverage**: FR-004, FR-005, Edge Cases (PDF protégé, navigateur incompatible)

### Implementation for User Story 5

- [x] T054 [US5] Add file size validation (max 50MB) before reading in estampify-standalone.html `<script>`
- [x] T055 [US5] Add page count validation (max 500) after parsing in estampify-standalone.html `<script>`
- [x] T056 [US5] Add watermark text length validation (max 150/200 chars) in estampify-standalone.html `<script>`
- [x] T057 [US5] Create specific error messages for each validation failure in estampify-standalone.html `<script>`
- [x] T058 [US5] Handle PDF parsing errors (corrupted files) with user-friendly message in estampify-standalone.html `<script>`
- [x] T058a [US5] Handle password-protected PDF with specific error message in estampify-standalone.html `<script>`
- [x] T059 [US5] Disable apply button when no PDF loaded or text empty in estampify-standalone.html `<script>`
- [x] T060 [US5] Create CSS for validation states (input borders, error colors) in estampify-standalone.html `<style>`

**Checkpoint**: ✅ User Story 5 complete - all validations and error messages working

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Améliorations finales et validation

- [x] T061 [P] Add responsive CSS for mobile devices in estampify-standalone.html `<style>`
- [x] T062 [P] Add header with logo and title in estampify-standalone.html `<body>`
- [x] T063 [P] Add footer with credits in estampify-standalone.html `<body>`
- [x] T064 Ensure all UI states are consistent (loading, error, success) in estampify-standalone.html `<script>`

### Internationalization (i18n) - FR-027, FR-028, FR-029

- [x] T068 Create i18n translations object with EN/FR/ES strings in estampify-standalone.html `<script>`
- [x] T069 Implement detectLanguage() using navigator.language in estampify-standalone.html `<script>`
- [x] T070 Implement t(key, replacements) translation function in estampify-standalone.html `<script>`
- [x] T071 Add data-i18n attributes to all translatable HTML elements in estampify-standalone.html `<body>`
- [x] T072 Implement applyTranslations() to update DOM on load in estampify-standalone.html `<script>`
- [x] T073 Update all error messages to use i18n.t() in estampify-standalone.html `<script>`
- [x] T074 Update template text generation to use i18n in estampify-standalone.html `<script>`
- [x] T075 Add Cloudflare Web Analytics script before closing body tag (NFR-006) in estampify-standalone.html `<body>`
### Validation Tests

- [x] T065 Test offline mode (disconnect network after load) in browser
- [x] T065a Verify PDF-lib.js caches correctly for offline use (browser cache headers) in browser
- [x] T066 Run quickstart.md validation tests in browser
- [x] T066a Validate performance: 100 pages PDF processed in < 30 seconds (NFR-004) in browser
- [x] T067 Verify file size < 500KB in estampify-standalone.html

---

## Dependencies & Execution Order

### Phase Dependencies

```
Phase 1: Setup ──────────────────────────────────────────────────────────────┐
    │                                                                        │
    ▼                                                                        │
Phase 2: Foundational ───────────────────────────────────────────────────────┤
    │                                                                        │
    ├───────────────┬───────────────┬───────────────┐                        │
    ▼               ▼               ▼               │                        │
Phase 3: US1    Phase 4: US2    (blocked)       (blocked)                    │
(Upload)        (Config)            │               │                        │
    │               │               │               │                        │
    └───────┬───────┘               │               │                        │
            ▼                       │               │                        │
        Phase 5: US4 ◄──────────────┘               │                        │
        (Generate)                                  │                        │
            │                                       │                        │
            ▼                                       ▼                        │
        MVP COMPLETE ─────────────────────► Phase 6: US3                     │
                                            (Template)                       │
                                                │                            │
                                                ▼                            │
                                            Phase 7: US5                     │
                                            (Errors)                         │
                                                │                            │
                                                ▼                            │
                                            Phase 8: Polish ◄────────────────┘
```

### User Story Dependencies

| Story | Depends On | Can Parallel With |
|-------|------------|-------------------|
| US1 (Upload) | Foundational | US2 (Config) |
| US2 (Config) | Foundational | US1 (Upload) |
| US4 (Generate) | US1, US2 | - |
| US3 (Template) | US2 | US5 |
| US5 (Errors) | US1 | US3 |

### Within Single File

Since all code is in one file, [P] markers indicate:
- **Different sections**: CSS vs JS vs HTML
- **No logical dependency**: Can be written in any order within phase

---

## Parallel Example: Phase 3-4 (US1 + US2)

```bash
# These can be developed in parallel:

# Developer A - User Story 1 (Upload):
T010-T021: Upload zone, file handling, preview

# Developer B - User Story 2 (Config):
T022-T034: Config panel, sliders, WatermarkConfig

# Merge point: Both needed for Phase 5 (US4)
```

---

## Implementation Strategy

### MVP First (Recommended Path)

1. ✅ Complete Phase 1: Setup (T001-T004)
2. ✅ Complete Phase 2: Foundational (T005-T009)
3. ✅ Complete Phase 3: US1 - Upload (T010-T021)
4. ✅ Complete Phase 4: US2 - Config (T022-T034)
5. ✅ Complete Phase 5: US4 - Generate (T035-T047)
6. **STOP**: Test MVP manually with quickstart.md scenarios
7. Deploy MVP if satisfactory

### Post-MVP

8. Complete Phase 6: US3 - Template (T048-T053)
9. Complete Phase 7: US5 - Errors (T054-T060)
10. Complete Phase 8: Polish (T061-T067)
11. Final validation

---

## Summary

| Metric | Value |
|--------|-------|
| **Total Tasks** | 83 |
| **Phase 1 (Setup)** | 4 tasks |
| **Phase 2 (Foundational)** | 6 tasks |
| **Phase 3 (US1 - Upload)** | 12 tasks |
| **Phase 4 (US2 - Config)** | 13 tasks |
| **Phase 5 (US4 - Generate)** | 17 tasks |
| **Phase 6 (US3 - Template)** | 6 tasks |
| **Phase 7 (US5 - Errors)** | 8 tasks |
| **Phase 8 (Polish + i18n)** | 16 tasks |
| **MVP Tasks (Phases 1-5)** | 51 tasks |
| **Parallel Opportunities** | US1 ∥ US2, US3 ∥ US5 |

---

## Notes

- Single-file architecture: All tasks target `estampify-standalone.html`
- No test framework: Manual testing per quickstart.md
- [P] = parallelizable within phase (different sections)
- [USn] = user story traceability
- MVP = Phases 1-5 (47 tasks) delivers core value
- Constitution compliance verified at each checkpoint
