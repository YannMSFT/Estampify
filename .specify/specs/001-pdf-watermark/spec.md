# Feature Specification: Application de filigrane PDF

**Feature Branch**: `001-pdf-watermark`  
**Created**: 2025-12-04  
**Status**: Draft  
**Input**: Application locale HTML permettant d'appliquer un filigrane à un fichier PDF, 100% client-side

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Upload et aperçu du PDF (Priority: P1)

En tant qu'utilisateur, je veux pouvoir télécharger un fichier PDF depuis mon ordinateur et voir un aperçu de la première page pour confirmer que le bon document est sélectionné.

**Why this priority**: Sans upload de PDF, aucune autre fonctionnalité n'est possible. C'est le point d'entrée obligatoire de l'application.

**Independent Test**: Ouvrir l'application, glisser-déposer un PDF, vérifier que l'aperçu s'affiche correctement.

**Acceptance Scenarios**:

1. **Given** l'application est ouverte, **When** je glisse-dépose un fichier PDF dans la zone d'upload, **Then** un aperçu de la première page s'affiche
2. **Given** l'application est ouverte, **When** je clique sur "Parcourir" et sélectionne un PDF, **Then** le fichier est chargé et son nom est affiché
3. **Given** un PDF est chargé, **When** je sélectionne un autre PDF, **Then** l'ancien est remplacé par le nouveau
4. **Given** l'application est ouverte, **When** je dépose un fichier non-PDF, **Then** un message d'erreur s'affiche

---

### User Story 2 - Configuration du filigrane texte (Priority: P1)

En tant qu'utilisateur, je veux pouvoir configurer le texte et l'apparence de mon filigrane (texte, taille, opacité, couleur, mode répétition) pour personnaliser le rendu final.

> **Note**: La rotation est fixée à 45° (diagonal) conformément à FR-011 et n'est pas configurable par l'utilisateur.

**Why this priority**: La configuration du filigrane est la fonctionnalité principale de l'application - elle doit être disponible dès le MVP.

**Independent Test**: Modifier les paramètres du filigrane et observer les changements en temps réel dans l'aperçu.

**Acceptance Scenarios**:

1. **Given** un PDF est chargé, **When** je saisis un texte de filigrane, **Then** l'aperçu se met à jour en temps réel
2. **Given** un filigrane est configuré, **When** je modifie la taille de police, **Then** le filigrane s'affiche avec la nouvelle taille
3. **Given** un filigrane est configuré, **When** je modifie l'opacité, **Then** le filigrane devient plus ou moins transparent
4. **Given** un filigrane est configuré, **When** je change la couleur, **Then** le filigrane adopte la nouvelle couleur
5. **Given** un filigrane est configuré, **When** j'active le mode répétition, **Then** le filigrane est dupliqué sur toute la page

---

### User Story 3 - Modèle de filigrane prédéfini (Priority: P2)

En tant qu'utilisateur, je veux pouvoir utiliser un modèle prédéfini de filigrane avec mon nom pour marquer rapidement des documents comme personnels.

**Why this priority**: Fonctionnalité de confort qui accélère le workflow pour les cas d'usage courants.

**Independent Test**: Activer le modèle, saisir un nom, vérifier que le texte généré correspond au format attendu.

**Acceptance Scenarios**:

1. **Given** l'option "Utiliser le modèle" est cochée, **When** je saisis "Jean Dupont", **Then** le texte devient "Document exclusivement destiné à l'usage de Jean Dupont"
2. **Given** le modèle est actif, **When** je décoche l'option, **Then** je peux saisir un texte libre

---

### User Story 4 - Génération et téléchargement du PDF (Priority: P1)

En tant qu'utilisateur, je veux pouvoir appliquer le filigrane à toutes les pages du PDF et télécharger le fichier résultant.

**Why this priority**: C'est le résultat final attendu - sans export, l'application n'a pas de valeur.

**Independent Test**: Charger un PDF, configurer un filigrane, cliquer sur "Appliquer", vérifier que le PDF téléchargé contient le filigrane.

**Acceptance Scenarios**:

1. **Given** un PDF est chargé et un filigrane configuré, **When** je clique sur "Appliquer le filigrane", **Then** le traitement démarre avec indicateur de progression
2. **Given** le traitement est terminé, **When** le fichier est prêt, **Then** le téléchargement démarre automatiquement
3. **Given** le PDF original s'appelle "document.pdf", **When** le traitement est terminé, **Then** le fichier téléchargé s'appelle "document_watermarked.pdf"
4. **Given** un PDF de 10 pages, **When** j'applique un filigrane, **Then** toutes les 10 pages contiennent le filigrane

---

### User Story 5 - Gestion des limites et erreurs (Priority: P2)

En tant qu'utilisateur, je veux être informé clairement si mon fichier dépasse les limites ou si une erreur survient pour comprendre comment résoudre le problème.

**Why this priority**: Améliore l'expérience utilisateur et évite les frustrations liées aux échecs silencieux.

**Independent Test**: Tenter de charger un fichier trop volumineux, vérifier que le message d'erreur est explicite.

**Acceptance Scenarios**:

1. **Given** l'application est ouverte, **When** je charge un PDF > 50MB, **Then** un message indique "Le fichier dépasse la limite de 50 MB"
2. **Given** l'application est ouverte, **When** je charge un PDF > 500 pages, **Then** un message indique "Le document dépasse la limite de 500 pages"
3. **Given** un filigrane > 150 caractères est saisi, **When** je tente de l'appliquer, **Then** un message indique la limite de caractères
4. **Given** un PDF corrompu est chargé, **When** le traitement échoue, **Then** un message d'erreur explicatif s'affiche

---

### Edge Cases

- Que se passe-t-il si le PDF est protégé par mot de passe ? → Message d'erreur clair
- Que se passe-t-il si le texte du filigrane est vide ? → Bouton "Appliquer" désactivé
- Que se passe-t-il si le navigateur ne supporte pas les fonctionnalités requises ? → Message de compatibilité
- Que se passe-t-il si l'utilisateur ferme l'onglet pendant le traitement ? → Données perdues (comportement attendu, pas de persistance)
- Que se passe-t-il avec un PDF contenant uniquement des images ? → Le filigrane s'applique par-dessus les images

## Requirements *(mandatory)*

### Functional Requirements

#### Upload et validation
- **FR-001**: L'application DOIT accepter les fichiers PDF via glisser-déposer
- **FR-002**: L'application DOIT accepter les fichiers PDF via sélection de fichier (input file)
- **FR-003**: L'application DOIT rejeter les fichiers non-PDF avec message d'erreur
- **FR-004**: L'application DOIT rejeter les fichiers > 50 MB avec message d'erreur
- **FR-005**: L'application DOIT rejeter les PDF > 500 pages avec message d'erreur

#### Configuration du filigrane
- **FR-006**: L'utilisateur DOIT pouvoir saisir un texte de filigrane libre (max 150 caractères)
- **FR-007**: L'utilisateur DOIT pouvoir utiliser un modèle prédéfini avec nom personnalisable (max 200 caractères total)
- **FR-008**: L'utilisateur DOIT pouvoir ajuster la taille de police (5-25, défaut 15)
- **FR-009**: L'utilisateur DOIT pouvoir ajuster l'opacité (5%-100%, défaut 20%)
- **FR-010**: L'utilisateur DOIT pouvoir choisir la couleur du filigrane
- **FR-011**: Le filigrane DOIT être affiché avec une rotation de 45° (diagonal)
- **FR-012**: L'utilisateur DOIT pouvoir activer/désactiver le mode répétition

#### Aperçu
- **FR-013**: L'application DOIT afficher un aperçu de la première page du PDF
- **FR-014**: L'aperçu DOIT se mettre à jour en temps réel lors des modifications du filigrane
- **FR-015**: L'aperçu DOIT représenter fidèlement le rendu final

#### Génération
- **FR-016**: L'application DOIT appliquer le filigrane à TOUTES les pages du PDF
- **FR-017**: L'application DOIT afficher un indicateur de progression pendant le traitement
- **FR-018**: L'application DOIT déclencher le téléchargement automatique du PDF généré
- **FR-019**: Le fichier généré DOIT être nommé `[nom_original]_watermarked.pdf`

#### Qualité du filigrane
- **FR-023**: Le filigrane dans le PDF final DOIT être IDENTIQUE à l'aperçu (même position, même espacement, même taille)
- **FR-024**: Chaque occurrence du filigrane DOIT être lisible dans son intégralité (aucun texte tronqué)
- **FR-025**: En mode répétition, les filigranes DOIVENT être espacés de manière régulière et homogène (espacement horizontal ET vertical constants)
- **FR-026**: Les filigranes NE DOIVENT PAS se chevaucher entre eux

#### Internationalisation (i18n)
- **FR-027**: L'application DOIT détecter automatiquement la langue du navigateur au chargement
- **FR-028**: L'application DOIT supporter 3 langues : Anglais (en), Français (fr), Espagnol (es)
- **FR-029**: Si la langue du navigateur n'est pas supportée, l'application DOIT utiliser l'anglais par défaut

#### Sécurité et confidentialité
- **FR-020**: AUCUNE donnée ne DOIT être envoyée vers un serveur externe
- **FR-021**: Tout le traitement DOIT s'effectuer côté client (JavaScript)
- **FR-022**: Les fichiers DOIVENT être libérés de la mémoire après téléchargement

### Non-Functional Requirements

- **NFR-001**: L'application DOIT fonctionner sans connexion internet (après chargement initial)
- **NFR-002**: L'application DOIT être contenue dans un seul fichier HTML autonome
- **NFR-003**: L'interface DOIT être responsive (desktop et mobile)
- **NFR-004**: Le traitement d'un PDF de 100 pages DOIT prendre moins de 30 secondes
- **NFR-005**: L'application DOIT fonctionner sur Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **NFR-006**: L'application DOIT inclure le script Cloudflare Web Analytics dans le body HTML

### Key Entities

- **PDFDocument**: Fichier PDF source avec ses métadonnées (nom, taille, nombre de pages)
- **WatermarkConfig**: Configuration du filigrane (texte, taille, opacité, couleur, rotation, répétition)
- **WatermarkedPDF**: PDF résultant avec filigrane appliqué sur toutes les pages

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: L'utilisateur peut appliquer un filigrane à un PDF en moins de 5 clics
- **SC-002**: 100% des traitements s'effectuent localement (0 requête réseau pour les données utilisateur)
- **SC-003**: Le fichier HTML autonome fonctionne en double-cliquant dessus (aucune installation)
- **SC-004**: Le traitement d'un PDF de 50 pages s'effectue en moins de 15 secondes
- **SC-005**: L'application supporte les 4 navigateurs majeurs sans dégradation fonctionnelle

## Constitution Compliance

Cette spécification respecte les principes de la constitution Estampify :

- ✅ **Client-Side First**: FR-020, FR-021 garantissent le traitement 100% local
- ✅ **Single-File Architecture**: NFR-002 impose le fichier HTML unique
- ✅ **Progressive Enhancement**: NFR-005 définit la compatibilité navigateurs
- ✅ **UX Simplicity**: SC-001 limite à 5 clics maximum
- ✅ **Defensive Processing**: FR-003 à FR-005 définissent les validations
