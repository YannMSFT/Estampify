<!--
## Sync Impact Report
- Version change: N/A → 1.0.0
- Modified principles: Initial constitution (no prior version)
- Added sections: Core Principles (5), Privacy & Security, Technology Stack, Governance
- Removed sections: None
- Templates requiring updates:
  - ✅ plan-template.md - Compatible with principles
  - ✅ spec-template.md - Compatible with user story approach
  - ✅ tasks-template.md - Compatible with single-file structure
- Follow-up TODOs: None
-->

# Stampify Constitution

## Core Principles

### I. Client-Side First (NON-NEGOTIABLE)
All document processing MUST occur entirely within the user's browser. No files, document 
content, or user data may be transmitted to any external server. This principle ensures 
complete privacy and eliminates data breach risks.

**Rationale**: Users trust Stampify with potentially sensitive documents. 100% client-side 
processing is the foundation of this trust and the primary differentiator from competing 
solutions.

### II. Single-File Architecture
The application MUST remain a standalone HTML file with all dependencies (CSS, JavaScript, 
external libraries) bundled inline. No build step, package manager, or external assets 
required at runtime.

**Rationale**: Maximum portability—users can download one file, double-click, and use the 
application immediately. No installation, no server setup, no technical expertise required.

### III. Progressive Enhancement
Core functionality (PDF upload, watermark application, download) MUST work on all modern 
browsers without JavaScript framework dependencies. Advanced features may use modern APIs 
but MUST degrade gracefully.

**Rationale**: Accessibility and reliability across diverse user environments. The 
application should "just work" without requiring specific browser versions or 
configurations.

### IV. User Experience Simplicity
The interface MUST remain minimal and intuitive. Features SHOULD require zero 
documentation to use. Visual feedback (previews, progress indicators, error messages) 
MUST be immediate and clear.

**Rationale**: Users want to watermark PDFs quickly, not learn complex software. Every UI 
element must justify its existence by directly serving the watermarking workflow.

### V. Defensive Processing
All user inputs MUST be validated and sanitized. File size, page count, and text length 
limits MUST be enforced. Error states MUST be handled gracefully with user-friendly 
messages and recovery paths.

**Rationale**: Robust error handling prevents data loss and user frustration. Clear limits 
prevent browser crashes and ensure consistent performance.

## Privacy & Security

- Documents MUST never leave the browser memory
- No analytics, tracking, or telemetry may be implemented
- No cookies or persistent storage of document content
- External library loads (PDF-lib.js) MUST use integrity hashes
- All processing occurs in-memory; documents are garbage collected after download

## Technology Stack

**Core Technologies**:
- HTML5/CSS3 for structure and styling
- Vanilla JavaScript (ES6+) for logic
- PDF-lib.js for PDF manipulation

**Constraints**:
- Maximum file size: 50 MB
- Maximum page count: 500 pages
- Maximum watermark text: 150 characters (200 with template)
- Supported browsers: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+, Opera 76+

**Forbidden**:
- No JavaScript frameworks (React, Vue, Angular, etc.)
- No build tools (Webpack, Vite, etc.) in production
- No server-side components
- No external API calls for core functionality

## Governance

This constitution supersedes all other development practices for Stampify. All changes 
MUST be evaluated against these principles before implementation.

**Amendment Process**:
1. Proposed amendments MUST document the rationale and impact
2. Changes to NON-NEGOTIABLE principles require explicit justification of exceptional 
   circumstances
3. Version increments follow semantic versioning:
   - MAJOR: Principle removal or backward-incompatible governance changes
   - MINOR: New principles or significant guidance additions
   - PATCH: Clarifications and wording improvements

**Compliance**:
- All code changes must verify compliance with principles
- Violations must be documented and justified in the change record
- Use README.md for user-facing documentation and changelog

**Version**: 1.0.0 | **Ratified**: 2025-12-04 | **Last Amended**: 2025-12-04
