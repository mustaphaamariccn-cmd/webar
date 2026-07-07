# 🔮 WebAR — Réalité Augmentée sans application

Expérience AR déclenchée par QR code, 100% dans le navigateur mobile. Aucune app à installer. Fonctionne sur Chrome Android et Safari iOS.

**Stack :** HTML/JS pur · A-Frame 1.4 · AR.js · Déployable sur GitHub Pages

---

## Structure des fichiers

```
/
├── index.html        ← Expérience AR principale (à ouvrir sur mobile)
├── marker.html       ← Marqueur Hiro à imprimer ou afficher
├── qrcode.html       ← Générateur de QR code pointant vers index.html
├── README.md         ← Ce fichier
└── assets/           ← Vos médias (créez ce dossier)
    ├── video.mp4     ← Votre vidéo AR (optionnel)
    └── image.png     ← Votre image AR (optionnel)
```

---

## Déploiement sur GitHub Pages (5 minutes)

### Étape 1 — Créer le dépôt GitHub

1. Connectez-vous sur [github.com](https://github.com) (créez un compte si besoin, c'est gratuit)
2. Cliquez sur **New repository**
3. Nommez-le `mon-ar` (ou ce que vous voulez)
4. Cochez **Public** (obligatoire pour GitHub Pages gratuit)
5. Cliquez **Create repository**

### Étape 2 — Uploader les fichiers

1. Dans votre dépôt, cliquez **Add file → Upload files**
2. Glissez-déposez tous les fichiers : `index.html`, `marker.html`, `qrcode.html`, `README.md`
3. Si vous avez des médias, créez le dossier `assets/` en nommant un fichier `assets/video.mp4` lors de l'upload
4. Cliquez **Commit changes**

### Étape 3 — Activer GitHub Pages

1. Allez dans **Settings → Pages** (colonne de gauche)
2. Sous **Source**, sélectionnez **Deploy from a branch**
3. Branch : **main**, dossier : **/ (root)**
4. Cliquez **Save**
5. Attendez 1–2 minutes → votre URL s'affiche : `https://votre-pseudo.github.io/mon-ar/`

### Étape 4 — Générer le QR code

1. Ouvrez `https://votre-pseudo.github.io/mon-ar/qrcode.html`
2. Collez l'URL `https://votre-pseudo.github.io/mon-ar/`
3. Téléchargez le PNG et intégrez-le sur votre flyer

---

## Personnaliser le contenu AR

### 1. Changer le texte, la couleur, le titre

Ouvrez `index.html` et modifiez le bloc `CONFIG` en haut du fichier (lignes 10–35) :

```javascript
const CONFIG = {
  titre: "Mon Expérience AR",          // Titre affiché dans l'interface
  messageInstruction: "Pointez vers le marqueur imprimé",
  couleurPrincipale: "#E74C3C",        // Couleur du cube (rouge par défaut)
  texteAR: "Scannez-moi !",            // Texte affiché en AR
  urlLienAR: "https://example.com",    // Lien si l'utilisateur touche le cube
  vitesseRotation: 3000,               // Vitesse de rotation (ms par tour)
};
```

### 2. Afficher une vidéo

1. Placez votre vidéo dans `assets/video.mp4`
   - Format : MP4, codec H.264, optimisé pour le web
   - Pour convertir : [cloudconvert.com](https://cloudconvert.com) ou HandBrake
   - Taille recommandée : < 10 Mo pour un chargement rapide sur mobile
2. Dans `index.html`, renseignez `urlVideo` dans CONFIG :
   ```javascript
   urlVideo: "assets/video.mp4",
   ```
3. La vidéo remplace automatiquement le cube 3D

> **Note iOS :** La vidéo démarre silencieuse (muted) car iOS l'exige pour l'autoplay. Pour activer le son, l'utilisateur doit toucher l'écran.

### 3. Afficher une image (ou GIF animé)

1. Placez votre image dans `assets/image.png`
2. Dans `index.html`, renseignez `urlImage` dans CONFIG :
   ```javascript
   urlImage: "assets/image.png",
   ```
3. Les GIF animés sont supportés (formats : PNG, JPG, GIF, WebP)

### 4. Afficher du contenu personnalisé (avancé)

Dans `index.html`, trouvez la section `<!-- CONTENU AR -->` et décommentez le bloc correspondant à votre besoin (A, B, C, D ou E). Chaque bloc est documenté avec les paramètres disponibles.

---

## Utiliser une image cible personnalisée (flyer, affiche)

Par défaut, le système reconnaît le **marqueur Hiro** (noir et blanc, imprimable depuis `marker.html`).

Pour déclencher l'AR sur votre propre flyer ou photo :

### Option A — Marqueur personnalisé (plus simple)

1. Allez sur le [générateur de marqueurs AR.js](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)
2. Chargez votre logo ou une image simple en N&B
3. Téléchargez le fichier `.patt` et l'image imprimable
4. Placez le `.patt` dans `assets/mon-marqueur.patt`
5. Dans `index.html`, remplacez `preset="hiro"` par :
   ```html
   <a-marker type="pattern" url="assets/mon-marqueur.patt">
   ```

### Option B — Reconnaissance d'image naturelle NFT (plus puissant)

La reconnaissance NFT (Natural Feature Tracking) permet de pointer sur n'importe quelle photo ou illustration riche en détails.

**Contrainte :** nécessite de générer des fichiers d'entraînement (`.fset`, `.fset3`, `.iset`).

**Outil :** [NFT Marker Creator](https://carnaux.github.io/NFT-Marker-Creator/) (en ligne, gratuit)

1. Uploadez votre image cible (recommandé : riche en textures, contrastes élevés, pas de zones unies)
2. Téléchargez le dossier généré (3 fichiers)
3. Placez-les dans `assets/nft/`
4. Dans `index.html`, remplacez le bloc `<a-marker>` par :
   ```html
   <a-nft
     type="nft"
     url="assets/nft/nom-du-fichier-sans-extension"
     smooth="true"
     smoothCount="10"
     smoothTolerance="0.01">
     <!-- Votre contenu AR ici -->
   </a-nft>
   ```
5. Changez la balise script AR.js :
   ```html
   <!-- Remplacez aframe-ar.js par : -->
   <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar-nft.js"></script>
   ```

> **Qualité de l'image cible NFT :** préférez des images avec beaucoup de détails, de textures et de contrastes. Les fonds unis, logos simples ou textes seuls fonctionnent mal.

---

## Générer le QR code

### Via la page intégrée (recommandé)

1. Déployez d'abord sur GitHub Pages (voir ci-dessus)
2. Ouvrez `https://votre-pseudo.github.io/mon-ar/qrcode.html`
3. Collez votre URL, générez, téléchargez

### Via un service externe

- [qr-code-generator.com](https://www.qr-code-generator.com)
- [qrcode-monkey.com](https://www.qrcode-monkey.com) (personnalisation couleur, logo)

L'URL à encoder : `https://votre-pseudo.github.io/nom-du-repo/`

> **L'URL doit être en HTTPS** — c'est obligatoire pour accéder à la caméra sur mobile. GitHub Pages fournit HTTPS automatiquement.

---

## Compatibilité navigateurs

| Navigateur | Android | iOS |
|---|---|---|
| Chrome | ✅ Complet | ✅ iOS 15+ |
| Safari | — | ✅ iOS 14.5+ |
| Firefox | ✅ | ⚠️ Partiel |
| Samsung Internet | ✅ | — |

**Conditions requises :**
- HTTPS (GitHub Pages ✓)
- Autoriser l'accès à la caméra quand le navigateur le demande
- Navigateur récent (2021+)

---

## Dépannage

**La caméra ne s'active pas**
→ Vérifiez que l'URL est bien en `https://`
→ Sur iOS, utilisez Safari (pas Chrome)
→ Autorisez l'accès caméra dans les réglages du navigateur

**Le marqueur n'est pas reconnu**
→ Assurez-vous d'avoir une bonne luminosité
→ Vérifiez que le fond blanc autour du marqueur est présent (au moins 1 cm)
→ Tenez le téléphone à 20–50 cm du marqueur, bien face à lui

**La vidéo ne se lance pas**
→ Sur iOS : la vidéo doit avoir l'attribut `muted` pour l'autoplay
→ Vérifiez que le fichier est accessible (chemin `assets/video.mp4`)
→ Encodez la vidéo au format MP4/H.264 avec [HandBrake](https://handbrake.fr)

**Page blanche après le scan QR**
→ L'URL dans le QR code est incorrecte — vérifiez dans qrcode.html
→ GitHub Pages n'est peut-être pas encore actif (attendez 2 min)

---

## Ressources utiles

- [Documentation AR.js](https://ar-js-org.github.io/AR.js-Docs/)
- [Documentation A-Frame](https://aframe.io/docs/)
- [Générateur de marqueurs](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html)
- [NFT Marker Creator](https://carnaux.github.io/NFT-Marker-Creator/)
- [Convertir vidéo pour le web](https://cloudconvert.com/mp4-converter)

---

*Projet WebAR · Stack open-source AR.js + A-Frame · Licence MIT*
