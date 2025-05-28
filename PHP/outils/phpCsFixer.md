# Documentation sur PHP CS Fixer

PHP CS Fixer est un outil permettant de formater et de corriger automatiquement le code PHP en fonction de règles de codage définies. Il est particulièrement utile pour garantir la cohérence du style de code dans un projet et respecter les normes établies (comme PSR-12).

---

## Installation

### 1. Installation globale via Composer

```bash
composer global require friendsofphp/php-cs-fixer
```

Assurez-vous que le chemin des binaires de Composer est dans votre variable `PATH`.

### 2. Installation locale via Composer

Ajoutez PHP CS Fixer à votre projet :

```bash
composer require --dev friendsofphp/php-cs-fixer
```

Cela installera l'outil dans le répertoire `vendor/bin`.

### 3. Téléchargement manuel

Téléchargez le fichier phar depuis [https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases](https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases) :

```bash
wget https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/vX.X.X/php-cs-fixer.phar
chmod +x php-cs-fixer.phar
sudo mv php-cs-fixer.phar /usr/local/bin/php-cs-fixer
```

---

## Utilisation de base

### 1. Formater un fichier ou un répertoire

Pour corriger un fichier ou un répertoire :

```bash
php-cs-fixer fix <chemin_du_fichier_ou_dossier>
```

Par exemple :

```bash
php-cs-fixer fix src/
```

### 2. Afficher les changements sans les appliquer

Ajoutez l'option `--dry-run` :

```bash
php-cs-fixer fix --dry-run --diff src/
```

Cela montre uniquement les modifications qui seraient apportées.

### 3. Vérification de la version

```bash
php-cs-fixer --version
```

### 4. Afficher l'aide

```bash
php-cs-fixer --help
```

---

## Configuration

Pour personnaliser les règles et la configuration, créez un fichier `.php-cs-fixer.dist.php` à la racine du projet :

### Exemple de fichier `.php-cs-fixer.dist.php`

```php
<?php

use PhpCsFixer\Config;

return (new Config())
    ->setRiskyAllowed(true) // Permet les règles "risquées"
    ->setRules([
        '@PSR12' => true, // Applique les règles PSR-12
        'array_syntax' => ['syntax' => 'short'], // Utilise la syntaxe courte des tableaux
        'strict_param' => true, // Force l'utilisation de paramètres stricts
    ])
    ->setFinder(
        PhpCsFixer\Finder::create()
            ->in(__DIR__ . '/src') // Définit le répertoire à analyser
            ->name('*.php')
            ->notName('*.blade.php') // Exclut certains fichiers
            ->ignoreDotFiles(true)
            ->ignoreVCS(true)
    );
```

### Lancer PHP CS Fixer avec la configuration

```bash
php-cs-fixer fix --config=.php-cs-fixer.dist.php
```

---

## Principales règles disponibles

### Catégories de règles

- **@PSR1** : Règles de base pour les fichiers PHP.
- **@PSR2** : Extension des règles PSR-1 avec des conventions de style de code.
- **@PSR12** : Remplacement de PSR-2 avec des conventions modernisées.
- **@Symfony** : Normes de style de code utilisées par Symfony.

### Quelques exemples de règles spécifiques

- `array_syntax`: Définit la syntaxe des tableaux (`short` ou `long`).
- `binary_operator_spaces`: Configure les espaces autour des opérateurs binaires.
- `no_trailing_whitespace`: Supprime les espaces en fin de ligne.
- `single_quote`: Utilise des guillemets simples pour les chaînes.

Pour la liste complète, consultez la commande suivante :

```bash
php-cs-fixer describe --list
```

---

## Bonnes pratiques

1. **Utiliser un fichier de configuration** : Cela permet de centraliser et de standardiser les règles au sein d'un projet.
2. **Vérifier les modifications avant de les appliquer** : Utilisez `--dry-run` pour éviter des changements inattendus.
3. **Automatiser avec un pré-commit hook** : Ajoutez un hook Git pour exécuter PHP CS Fixer avant chaque commit.

### Exemple de hook Git

Dans `.git/hooks/pre-commit` :

```bash
#!/bin/sh

php-cs-fixer fix --config=.php-cs-fixer.dist.php --dry-run --diff
if [ $? -ne 0 ]; then
    echo "\nStyle errors detected. Please fix them before committing."
    exit 1
fi
```

Rendez-le exécutable :

```bash
chmod +x .git/hooks/pre-commit
```

---

## Cas d'erreurs courantes

### 1. "php-cs-fixer: command not found"

**Cause** : PHP CS Fixer n'est pas installé ou n'est pas dans le `PATH`. **Solution** : Vérifiez votre installation et ajoutez le chemin des binaires à la variable `PATH`.

### 2. "No rules defined"

**Cause** : Le fichier de configuration n'a pas de règles définies. **Solution** : Vérifiez votre fichier `.php-cs-fixer.dist.php` et ajoutez des règles.

### 3. "Nothing to fix"

**Cause** : Aucun fichier ne correspond au Finder ou les fichiers sont déjà conformes. **Solution** : Vérifiez les chemins configurés dans le Finder.

---

## Conclusion

PHP CS Fixer est un outil puissant pour maintenir un code PHP propre et cohérent. Il s'intègre facilement dans des workflows existants et prend en charge de nombreuses normes de codage. En suivant les bonnes pratiques et en personnalisant les règles, il devient un allié indispensable dans le développement PHP professionnel.

Pour plus d'informations, consultez la documentation officielle : [https://cs.symfony.com/](https://cs.symfony.com/)