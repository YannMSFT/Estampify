# Research: Application de filigrane PDF

**Feature**: 001-pdf-watermark  
**Date**: 2025-12-04  
**Status**: Complete

## Research Tasks

### 1. Bibliothèque de manipulation PDF côté client

**Question**: Quelle bibliothèque JavaScript permet de manipuler des PDF entièrement côté client ?

**Decision**: PDF-lib.js v1.17.1

**Rationale**:
- 100% JavaScript, fonctionne entièrement dans le navigateur
- Pas de dépendances externes (standalone)
- Supporte la modification de PDF existants (pas seulement la création)
- API moderne basée sur les Promises
- Licence MIT (compatible usage commercial)
- Taille raisonnable (~300KB minifié)
- Maintenu activement (>14k stars GitHub)

**Alternatives considérées**:
| Bibliothèque | Rejetée car |
|--------------|-------------|
| jsPDF | Création uniquement, ne peut pas modifier un PDF existant |
| PDF.js (Mozilla) | Lecture/rendu uniquement, pas de modification |
| pdfmake | Création uniquement, syntaxe déclarative complexe |
| HummusJS | Nécessite Node.js, pas compatible navigateur pur |

**Intégration**:
```html
<script src="https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js" 
        integrity="sha384-[hash]" 
        crossorigin="anonymous"></script>
```

---

### 2. Rendu d'aperçu PDF dans le navigateur

**Question**: Comment afficher un aperçu de la première page du PDF avec le filigrane superposé ?

**Decision**: Canvas HTML5 + PDF.js pour le rendu, overlay CSS pour le filigrane simulé

**Rationale**:
- PDF.js peut rendre une page PDF sur un canvas
- L'aperçu du filigrane peut être simulé avec un overlay CSS/canvas
- Évite de régénérer le PDF à chaque modification (performance)
- Mise à jour temps réel < 100ms

**Alternatives considérées**:
| Approche | Rejetée car |
|----------|-------------|
| Régénérer le PDF complet à chaque changement | Trop lent (>1s pour PDF multi-pages) |
| iframe avec data URL | Pas de contrôle sur le rendu, pas d'overlay |
| Image statique | Ne représente pas fidèlement le PDF |

**Implementation Pattern**:
1. Charger le PDF avec PDF-lib.js
2. Extraire la première page et la rendre sur canvas avec PDF.js
3. Superposer un canvas transparent pour le filigrane simulé
4. Mettre à jour uniquement le canvas filigrane lors des modifications

---

### 3. Gestion des fichiers volumineux

**Question**: Comment gérer des fichiers PDF jusqu'à 50MB sans bloquer l'UI ?

**Decision**: FileReader avec ArrayBuffer + traitement asynchrone

**Rationale**:
- FileReader.readAsArrayBuffer() est non-bloquant
- ArrayBuffer peut être passé directement à PDF-lib.js
- Le traitement page par page permet d'afficher la progression
- Limite de 50MB définie dans la spec (protège contre les crashes navigateur)

**Best Practices**:
- Valider la taille AVANT de lire le fichier complet
- Utiliser `async/await` pour le traitement séquentiel des pages
- Afficher progression: `Page X sur Y`
- Libérer les références ArrayBuffer après traitement (garbage collection)

---

### 4. Application du filigrane avec rotation

**Question**: Comment appliquer un texte en diagonale (45°) sur chaque page du PDF ?

**Decision**: Utiliser les transformations matricielles de PDF-lib.js

**Rationale**:
- PDF-lib.js supporte `page.drawText()` avec options de rotation
- La rotation se fait autour du point d'origine du texte
- Calcul de la position centrale de la page pour centrer le filigrane

**Implementation Pattern**:
```javascript
const { width, height } = page.getSize();
const centerX = width / 2;
const centerY = height / 2;

page.drawText(watermarkText, {
  x: centerX,
  y: centerY,
  size: fontSize,
  font: font,
  color: rgb(r, g, b),
  opacity: opacity,
  rotate: degrees(45),
});
```

**Mode répétition**:
- Calculer l'espacement basé sur la longueur du texte et la taille de police
- Créer une grille de filigranes couvrant toute la page
- Décaler les lignes paires pour éviter l'alignement vertical

---

### 5. Encodage des polices

**Question**: Quelles polices peuvent être utilisées dans le PDF généré ?

**Decision**: Helvetica (police standard PDF) par défaut

**Rationale**:
- Helvetica est une des 14 polices standards PDF (Type 1)
- Pas besoin d'embarquer la police dans le fichier
- Garantit la compatibilité avec tous les lecteurs PDF
- PDF-lib.js inclut `StandardFonts.Helvetica`

**Alternatives considérées**:
| Option | Rejetée car |
|--------|-------------|
| Polices personnalisées embarquées | Augmente significativement la taille du PDF |
| Polices web (Google Fonts) | Nécessite téléchargement, casse le mode offline |
| Toutes les 14 polices standards | Complexité UI inutile pour l'MVP |

---

### 6. Conversion couleur hex vers RGB

**Question**: Comment convertir les couleurs HTML (hex) vers le format RGB de PDF-lib.js ?

**Decision**: Fonction utilitaire de parsing hex

**Implementation**:
```javascript
function hexToRgb(hex) {
  const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
  return result ? {
    r: parseInt(result[1], 16) / 255,
    g: parseInt(result[2], 16) / 255,
    b: parseInt(result[3], 16) / 255
  } : { r: 0, g: 0, b: 0 };
}
```

**Note**: PDF-lib.js attend des valeurs RGB normalisées (0-1), pas 0-255.

---

## Dependencies Summary

| Dépendance | Version | Usage | Chargement |
|------------|---------|-------|------------|
| PDF-lib.js | 1.17.1 | Modification PDF | CDN avec integrity hash |
| (optionnel) PDF.js | 3.x | Rendu aperçu | CDN ou canvas simplifié |

## Resolved Clarifications

Tous les points techniques ont été clarifiés. Aucun "NEEDS CLARIFICATION" restant.

## Next Steps

→ Procéder à Phase 1: Générer `data-model.md` et `quickstart.md`
