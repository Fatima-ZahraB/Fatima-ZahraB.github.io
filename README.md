# 🧠 Portfolio Data Scientist Machine Learning

Portfolio professionnel moderne conçu avec Jekyll et GitHub Pages.

## 🚀 Déploiement sur GitHub Pages

### 1. Configuration du Repository

1. Créez un nouveau repository sur GitHub nommé `votre-username.github.io`
2. Clonez le repository localement
3. Copiez tous les fichiers de ce portfolio dans votre repository

### 2. Structure des fichiers

```
votre-username.github.io/
├── _config.yml                 # Configuration Jekyll
├── _layouts/
│   └── default.html           # Template principal
├── assets/
│   └── css/
│       └── style.scss         # Styles personnalisés
├── images/
│   └── carte_mentale.jpeg     # Vos images
├── index.html                 # Page d'accueil
├── Gemfile                    # Dépendances Ruby
└── README.md                  # Ce fichier
```

### 3. Personnalisation

#### Dans `_config.yml` :
- Modifiez `title`, `description`, `author`
- Ajoutez votre email
- Mettez à jour l'URL de votre site
- Ajoutez vos usernames GitHub et LinkedIn

#### Dans `index.html` :
- Personnalisez le contenu de chaque section
- Mettez à jour les liens vers vos projets GitHub
- Ajoutez vos informations de contact

#### Dans `assets/css/style.scss` :
- Vous pouvez personnaliser les couleurs dans les variables CSS
- Modifier les polices, espacements, etc.

### 4. Ajout d'images

1. Placez vos images dans le dossier `images/`
2. Référencez-les dans le HTML avec : `images/nom-de-fichier.jpg`

### 5. Activation GitHub Pages

1. Allez dans Settings > Pages de votre repository
2. Source : sélectionnez "Deploy from a branch"
3. Branch : sélectionnez `main` (ou `master`) et `/root`
4. Cliquez sur "Save"

Votre site sera disponible à : `https://votre-username.github.io`

## 🛠️ Test en local (optionnel)

Pour tester votre site localement avant de le déployer :

```bash
# Installer les dépendances
bundle install

# Lancer le serveur local
bundle exec jekyll serve

# Ouvrir dans le navigateur
# http://localhost:4000
```

## 📝 Mise à jour du contenu

### Ajouter un projet

Dans `index.html`, section `#projects`, ajoutez un nouveau bloc :

```html
<div class="project-card">
  <span class="project-icon">🎯</span>
  <h3>Titre du Projet</h3>
  <p>Description du projet...</p>
  <div class="tech-stack">
    <span class="tech-tag">Python</span>
    <span class="tech-tag">ML</span>
  </div>
  <a href="lien-github" class="project-link" target="_blank">
    Voir le projet →
  </a>
</div>
```

### Modifier les compétences

Dans `index.html`, section `#skills`, modifiez les niveaux :

```html
<div class="skill-progress" style="width: 85%"></div>
```

## 🎨 Personnalisation du design

### Changer les couleurs

Dans `assets/css/style.scss`, modifiez les variables :

```css
:root {
  --primary: #0a192f;        /* Couleur principale */
  --secondary: #64ffda;      /* Couleur secondaire */
  --accent: #f76c6c;         /* Couleur d'accent */
}
```

### Changer les polices

Modifiez les imports Google Fonts et les variables :

```css
@import url('https://fonts.googleapis.com/css2?family=VotrePolice&display=swap');

:root {
  --font-display: 'VotrePolice', monospace;
}
```

## 📱 Responsive Design

Le portfolio est entièrement responsive et s'adapte automatiquement aux :
- 📱 Mobiles
- 📱 Tablettes  
- 💻 Ordinateurs

## ✨ Fonctionnalités

- ✅ Design moderne et professionnel
- ✅ Animations fluides
- ✅ Navigation smooth scroll
- ✅ Sections animées au scroll
- ✅ Barres de compétences animées
- ✅ Cards projets avec effets hover
- ✅ Timeline d'expérience
- ✅ Footer avec liens sociaux
- ✅ SEO optimisé
- ✅ Performance optimisée

## 🆘 Problèmes courants

### Le site ne s'affiche pas
- Vérifiez que GitHub Pages est activé dans les settings
- Attendez quelques minutes après l'activation
- Vérifiez que le nom du repository est correct

### Les styles ne s'appliquent pas
- Vérifiez que le fichier `assets/css/style.scss` existe
- Assurez-vous qu'il commence par `---` (front matter Jekyll)

### Les images ne s'affichent pas
- Vérifiez les chemins des images
- Les images doivent être dans le dossier `images/`

## 📧 Contact

Pour toute question, n'hésitez pas à me contacter !

---

**Fait en utilisant Jekyll et GitHub Pages**
