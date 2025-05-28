# Documentation sur PHPStan

## Introduction

PHPStan est un outil d'analyse statique de code PHP qui détecte des bugs dans votre code sans l'exécuter. Il aide les développeurs à améliorer la qualité de leur code en signalant les erreurs potentielles et en fournissant des suggestions pour les corriger.

---

## Installation

### Installation via Composer

Pour installer PHPStan en tant que dépendance de développement dans votre projet :

```bash
composer require --dev phpstan/phpstan
```

### Installation globale

Si vous souhaitez utiliser PHPStan globalement :

```bash
composer global require phpstan/phpstan
```

Assurez-vous d'ajouter le chemin global de Composer à votre variable `PATH` pour pouvoir exécuter PHPStan depuis n'importe où.

---

## Configuration

PHPStan peut être configuré via un fichier `phpstan.neon` ou `phpstan.neon.dist` à la racine de votre projet.

### Exemple de configuration basique

```neon
parameters:
    level: max
    paths:
        - src
        - tests
    bootstrapFiles:
        - vendor/autoload.php
```

### Niveaux d'analyse

PHPStan offre 9 niveaux d'analyse (de 0 à 8). Le niveau 0 signale uniquement les erreurs critiques, tandis que le niveau 8 offre l'analyse la plus stricte. Pour définir un niveau :

```neon
parameters:
    level: 5
```

---

## Utilisation

### Commande de base

Pour analyser le dossier `src` :

```bash
vendor/bin/phpstan analyse src
```

### Spécifier un niveau d'analyse

```bash
vendor/bin/phpstan analyse src --level=5
```

### Utiliser un fichier de configuration

Si un fichier de configuration existe :

```bash
vendor/bin/phpstan analyse
```

### Fournir une configuration explicite

```bash
vendor/bin/phpstan analyse --configuration=phpstan.neon
```

---

## Intégration avec des Frameworks

PHPStan propose des extensions pour les frameworks comme Symfony, Laravel, etc. Ces extensions peuvent être installées via Composer.

### Exemple avec Symfony

```bash
composer require --dev phpstan/phpstan-symfony
```

Ajoutez l'extension à votre fichier de configuration :

```neon
includes:
    - vendor/phpstan/phpstan-symfony/extension.neon
```

---

## Commandes utiles

### Vérifier l'installation de PHPStan

Pour vérifier que PHPStan est installé :

```bash
composer show phpstan/phpstan
```

### Afficher l'aide

Pour afficher toutes les options disponibles :

```bash
vendor/bin/phpstan --help
```

---

## Résolution des problèmes

### Problème de mémoire

Si PHPStan consomme trop de mémoire, augmentez la limite de mémoire PHP :

```bash
php -d memory_limit=2G vendor/bin/phpstan analyse
```

### Suppression des faux positifs

Si PHPStan signale des erreurs incorrectes, vous pouvez les ignorer dans le fichier de configuration :

```neon
parameters:
    ignoreErrors:
        - "#Pattern de l'erreur#"
```

---

## Bonnes pratiques

1. **Intégrez PHPStan dans votre CI/CD** : Ajoutez PHPStan dans votre pipeline de déploiement pour détecter les problèmes tôt.
2. **Montez progressivement les niveaux** : Commencez par un niveau bas et augmentez progressivement.
3. **Utilisez des extensions** : Adaptez PHPStan à votre projet en ajoutant des extensions spécifiques à vos bibliothèques et frameworks.
4. **Documentez vos suppressions** : Justifiez les erreurs ignorées pour éviter les oublis ou la confusion.

---

## Ressources utiles

- [Documentation officielle](https://phpstan.org/)
- [Guide des extensions](https://phpstan.org/user-guide/extensions)
- [Référentiel GitHub](https://github.com/phpstan/phpstan)

---

En utilisant PHPStan, vous assurez une meilleure qualité et maintenabilité de votre code. Adoptez-le dès aujourd'hui pour un développement PHP plus robuste !