### Documentation Symfony : Asset Management

#### Description

L'Asset Management dans Symfony permet de gérer les fichiers statiques comme les feuilles de style (CSS), les images et les fichiers JavaScript. Symfony fournit un système puissant et flexible pour inclure, versionner et optimiser ces ressources dans une application web.

---

### Configuration des assets

Symfony utilise le composant **Asset** pour gérer les ressources publiques. Par défaut, les fichiers sont stockés dans le répertoire `public/`, et vous pouvez accéder aux fichiers via l'URL du site.

#### Activer le gestionnaire d'assets

Si vous utilisez Symfony Flex, le gestionnaire d'assets est installé et configuré automatiquement. Sinon, vous pouvez l'ajouter à votre projet en exécutant :

```bash
composer require symfony/asset
```

Vous pouvez ensuite configurer les assets dans `config/packages/assets.yaml` :

```yaml
# config/packages/assets.yaml
framework:
    assets:
        version: 'v1'  # Version de vos assets pour le cache
        version_format: '%%s?version=%%s'  # Format de version
        packages:
            my_assets:
                base_path: '/path/to/assets'  # Chemin relatif de vos assets
```

---

### Utilisation des assets dans les templates Twig

Vous pouvez inclure des fichiers d'assets dans vos templates Twig à l'aide de la fonction `asset()`.

#### Exemple avec CSS :

```twig
<link rel="stylesheet" href="{{ asset('styles.css') }}">
```

Cela générera un lien vers `public/styles.css` ou une version avec un hash si la gestion de version est activée.

#### Exemple avec JavaScript :

```twig
<script src="{{ asset('scripts/app.js') }}"></script>
```

#### Exemple avec des images :

```twig
<img src="{{ asset('images/logo.png') }}" alt="Logo">
```

---

### Versionnement des Assets

Symfony permet de versionner automatiquement vos assets pour assurer qu'ils sont correctement rafraîchis lors de la mise à jour des fichiers, ce qui évite les problèmes de cache dans les navigateurs.

#### Utilisation du versionnement avec Webpack Encore

Symfony recommande d'utiliser [Webpack Encore](https://symfony.com/doc/current/frontend/encore/installation.html) pour gérer les assets. Webpack Encore gère la compilation, la minification et le versionnement des fichiers JavaScript, CSS et images.

1. **Installation de Webpack Encore** :

```bash
composer require symfony/webpack-encore-bundle
npm install
```

2. **Compilation des assets avec Webpack Encore** :

Dans le fichier `webpack.config.js` :

```javascript
const Encore = require('@symfony/webpack-encore');

Encore
    .setOutputPath('public/build/')
    .setPublicPath('/build')
    .addEntry('app', './assets/js/app.js')
    .enableSassLoader()
    .enableVersioning()
;

module.exports = Encore.getWebpackConfig();
```

3. **Utilisation dans Twig** :

```twig
<script src="{{ asset('build/app.js') }}"></script>
```

Cela inclura la version avec un hash unique pour chaque changement de fichier (ex : `app.abc123.js`).

---

### Gestion des assets d'images

Symfony permet de gérer les images dans le répertoire `public/`, mais aussi dans des répertoires spécifiques pour chaque environnement (par exemple, différentes images pour la production et le développement).

#### Exemple de gestion d'image dans Twig :

```twig
<img src="{{ asset('images/logo.png') }}" alt="Logo">
```

Cela générera un chemin vers le fichier image dans le dossier `public/images`.

---

### Gestion des chemins d'assets avec des packages personnalisés

Vous pouvez définir des packages d'assets personnalisés pour gérer des ressources dans plusieurs emplacements, en dehors du répertoire `public/`.

#### Exemple de configuration de packages :

```yaml
# config/packages/assets.yaml
framework:
    assets:
        packages:
            custom_assets:
                base_path: '/assets'
```

Dans votre template Twig :

```twig
<link rel="stylesheet" href="{{ asset('styles.css', 'custom_assets') }}">
```

Cela générera le chemin vers `/assets/styles.css`.

---

### Conseils et Bonnes Pratiques

1. **Utiliser Webpack Encore** : Webpack Encore facilite grandement le travail de gestion des assets. Il vous permet de gérer la compilation, la minification, et la gestion de la version des fichiers.
2. **Versionner les assets** : Toujours versionner vos assets (avec Webpack Encore ou manuellement) pour éviter les problèmes de cache dans les navigateurs des utilisateurs.
3. **Utiliser des fichiers statiques dans `public/`** : Placez toujours vos fichiers statiques (CSS, JS, images) dans le répertoire `public/` ou dans un sous-répertoire de `public/`.
4. **Optimiser les images** : Utilisez des outils de compression d'images pour optimiser la taille de vos fichiers image et réduire les temps de chargement.
5. **Exploiter les CDN pour les assets externes** : Si vous utilisez des bibliothèques JavaScript ou CSS populaires, vous pouvez également les servir à partir de CDN pour améliorer les performances.

---

### Ressources Complémentaires

- [Symfony Asset Component](https://symfony.com/doc/current/components/asset.html)
- [Symfony Webpack Encore](https://symfony.com/doc/current/frontend/encore.html)
- [Symfony Documentation sur les Assets](https://symfony.com/doc/current/templating/assets.html)