# Estampify 🖨️

Application web locale pour ajouter des filigranes personnalisables aux documents PDF.

![100% Client-Side](https://img.shields.io/badge/100%25-Client--Side-brightgreen)
![No Backend Required](https://img.shields.io/badge/No-Backend-blue)
![Privacy First](https://img.shields.io/badge/Privacy-First-orange)

## � Description

Estampify est une application web 100% client-side qui permet d'ajouter des filigranes aux fichiers PDF directement dans votre navigateur, sans jamais envoyer vos documents sur un serveur.

## ✨ Fonctionnalités

- ✅ **Upload de PDF** : Glissez-déposez ou sélectionnez un fichier PDF (max 50MB, 500 pages)
- ✅ **Filigrane personnalisable** :
  - Texte libre ou modèle prédéfini
  - Taille de police ajustable (10-100)
  - Opacité réglable (10-100%)
  - Couleur personnalisable
  - Rotation fixée à 45° pour un effet diagonal
  - Mode répétition pour couvrir toute la page
- ✅ **Aperçu en temps réel** : Visualisez le rendu avant de générer le PDF
- ✅ **Traitement local** : Aucune donnée n'est envoyée sur un serveur
- ✅ **Interface moderne** : Design épuré et responsive

## 🚀 Utilisation

### Option 1 : Ouvrir directement dans le navigateur

1. Téléchargez le fichier `estampify-standalone.html`
2. Double-cliquez dessus pour l'ouvrir dans votre navigateur
3. C'est tout ! L'application est prête à l'emploi

### Option 2 : Héberger localement

```bash
# Cloner le dépôt
git clone https://github.com/YannMSFT/Estampify.git
cd Estampify

# Ouvrir avec un serveur local (optionnel)
python -m http.server 8000
# ou
npx serve

# Accéder à http://localhost:8000/estampify-standalone.html
```

## � Guide d'utilisation

### 1. Télécharger un PDF

- Glissez-déposez un fichier PDF dans la zone prévue
- Ou cliquez sur "Parcourir" pour sélectionner un fichier
- Limite : 50MB et 500 pages maximum

### 2. Configurer le filigrane

#### Texte
- **Option 1** : Entrez votre texte personnalisé (max 150 caractères)
- **Option 2** : Cochez "Utiliser le modèle" pour générer automatiquement :
  ```
  Document exclusivement destiné à l'usage de [NOM]
  ```

#### Apparence
- **Taille de police** : Ajustez de 10 à 100 (défaut: 20)
- **Opacité** : Réglez de 10% à 100% (défaut: 30%)
- **Couleur** : Choisissez n'importe quelle couleur (défaut: noir)
- **Rotation** : Fixée à 45° pour un effet diagonal optimal

#### Mode répétition
- Cochez "Répéter le filigrane" pour couvrir toute la page
- L'espacement est calculé automatiquement en fonction de la taille du texte
- Le filigrane est optimisé pour éviter les superpositions

### 3. Aperçu et génération

- L'aperçu se met à jour en temps réel lors des modifications
- Cliquez sur "Appliquer le filigrane" pour générer le PDF final
- Le fichier sera téléchargé avec le suffixe `_watermarked.pdf`

## 🔒 Sécurité et confidentialité

- **100% local** : Tout le traitement se fait dans votre navigateur
- **Aucun upload** : Vos fichiers ne quittent jamais votre ordinateur
- **Aucun serveur** : Pas de backend, pas de stockage distant
- **Open source** : Le code est auditable et transparent

## 🛠️ Technologies utilisées

- **PDF-lib.js** (v1.17.1) : Manipulation de PDF côté client
- **HTML5/CSS3** : Interface moderne et responsive
- **JavaScript Vanilla** : Aucune dépendance framework
- **GitHub Pages ready** : Déployable en un clic

## 📦 Structure du projet

```
Estampify/
├── estampify-standalone.html    # Application complète (fichier unique)
├── README.md                   # Cette documentation
├── LICENSE                     # Licence MIT
└── .github/
    └── copilot-instructions.md # Instructions de développement
```

## 🔧 Configuration technique

### Limites

- **Taille de fichier** : 50 MB maximum
- **Nombre de pages** : 500 pages maximum
- **Longueur du texte** : 150 caractères (200 avec le modèle)

### Compatibilité navigateurs

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Opera 76+

### Algorithme d'espacement

Le mode répétition utilise un algorithme intelligent :
1. Calcule la boîte englobante du texte pivoté à 45°
2. Ajoute une marge proportionnelle à la taille de police
3. Distribue les filigranes uniformément sur toute la page
4. Évite automatiquement les superpositions

## 🎨 Personnalisation

Le fichier `estampify-standalone.html` peut être personnalisé :

- **Couleurs** : Modifiez les variables CSS dans la section `:root`
- **Logo** : Remplacez l'URL du logo dans la section `<header>`
- **Limites** : Ajustez `MAX_FILE_SIZE` et `MAX_PAGES` dans le JavaScript
- **Valeurs par défaut** : Modifiez les valeurs dans les contrôles HTML

## 🐛 Résolution de problèmes

### Le PDF ne se génère pas
- Vérifiez que le fichier fait moins de 50MB
- Vérifiez qu'il y a moins de 500 pages
- Essayez avec un navigateur récent

### L'aperçu ne correspond pas au résultat
- L'aperçu est une simulation visuelle
- Le rendu final utilise les dimensions exactes du PDF
- Les espacements peuvent légèrement varier selon la taille du document

### Erreur "Fichier corrompu"
- Le PDF source est peut-être endommagé
- Essayez de l'ouvrir avec un lecteur PDF pour vérifier
- Essayez de le ré-enregistrer avec un autre outil

## 📄 Licence

MIT License - Voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 👤 Auteur

**Yann** - [YannMSFT](https://github.com/YannMSFT)

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à :
- 🐛 Signaler des bugs
- 💡 Proposer des nouvelles fonctionnalités
- 🔧 Soumettre des pull requests

## 📝 Changelog

### Version 1.0.0 (Octobre 2025)
- ✨ Version initiale
- ✅ Upload et traitement de PDF
- ✅ Filigrane personnalisable
- ✅ Mode répétition avec espacement optimisé
- ✅ Aperçu en temps réel
- ✅ 100% client-side

---

**Note** : Cette application a été développée avec l'assistance de GitHub Copilot pour garantir un code propre et maintenable.

## 📦 Fichiers

```
estampify-standalone.html    # Fichier unique autonome (tout le code est inclus)
```

## 🌐 Compatibilité

Fonctionne sur tous les navigateurs modernes :
- ✅ Chrome / Edge
- ✅ Firefox
- ✅ Safari
- ✅ Opera

## 🎯 Avantages de la version standalone

1. **Aucune installation** : Pas de Python, Flask, ou dépendances
2. **Portable** : Copiez le fichier HTML sur n'importe quel ordinateur
3. **Sécurisé** : Vos documents restent privés
4. **Rapide** : Pas de communication réseau
5. **Simple** : Double-clic et c'est parti !

## 📝 Utilisation

1. **Ouvrir** le fichier `estampify-standalone.html`
2. **Sélectionner** votre fichier PDF
3. **Configurer** le filigrane (texte, taille, rotation, etc.)
4. **Prévisualiser** le rendu en temps réel
5. **Appliquer** le filigrane
6. **Télécharger** votre PDF modifié

## 🆚 Comparaison avec la version Flask

| Caractéristique | Version Flask | Version Standalone |
|----------------|---------------|-------------------|
| Installation | Python + dépendances | Aucune |
| Serveur requis | Oui (local) | Non |
| Sécurité | Données locales | Données locales |
| Portabilité | Moyenne | Excellente |
| Performance | Rapide | Rapide |
| Simplicité | Moyenne | Très simple |

## 🎨 Interface

Interface minimaliste avec :
- Zone de drag & drop pour l'upload
- Curseurs intuitifs pour les réglages
- Aperçu en temps réel
- Messages d'erreur clairs
- Design responsive (mobile-friendly)

## 📄 Limites

- Taille maximale : 50 MB
- Nombre de pages : 500 maximum
- Format : PDF uniquement

## 🔧 Personnalisation

Le fichier HTML contient tout le code (HTML, CSS, JavaScript). Vous pouvez facilement :
- Modifier les couleurs dans la section `:root`
- Ajuster les limites (taille, pages)
- Personnaliser le texte et les labels
- Ajouter des fonctionnalités

## 📞 Support

Pour toute question ou suggestion, créez une issue sur le repository.

---

**Estampify Standalone** - La solution la plus simple pour ajouter des filigranes à vos PDF ! 🎉
