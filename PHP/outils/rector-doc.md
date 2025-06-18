
## 🎯 Qu'est-ce que Rector ?

Rector est votre assistant personnel pour la modernisation de code PHP ! Imaginez-le comme un "correcteur automatique" ultra-intelligent pour votre code. Au lieu de modifier manuellement des centaines de fichiers, Rector le fait pour vous en quelques secondes.

## 💻 Installation : Premiers pas

### 1️⃣ Installez Rector via Composer
```bash
# Cette commande installe Rector comme outil de développement
composer require rector/rector --dev

# Vérifiez que l'installation a réussi
./vendor/bin/rector --version
```

## 🛠 Configuration : Créons votre fichier rector.php

La configuration de Rector se fait dans un fichier `rector.php`. Voyons comment le construire étape par étape :

### Version basique :
```php
<?php

// 1. On déclare qu'on veut du PHP strict
declare(strict_types=1);

// 2. On importe les classes nécessaires
use Rector\Config\RectorConfig;

// 3. On crée notre configuration
return static function (RectorConfig $rectorConfig): void {
    // 4. On indique où chercher les fichiers
    $rectorConfig->paths([
        __DIR__ . '/src'  // 👈 Dossier principal
    ]);
};
```

### Version plus avancée :
```php
<?php

declare(strict_types=1);

use Rector\Config\RectorConfig;
use Rector\Set\ValueObject\LevelSetList;  // 👈 Pour les règles de niveau PHP
use Rector\Symfony\Set\SymfonySetList;    // 👈 Pour les règles Symfony

return static function (RectorConfig $rectorConfig): void {
    // 🎯 Où chercher les fichiers
    $rectorConfig->paths([
        __DIR__ . '/src',          // Dossier principal
        __DIR__ . '/tests',        // Tests également
    ]);

    // 🎨 Quelles règles appliquer
    $rectorConfig->sets([
        LevelSetList::UP_TO_PHP_74,         // ⭐ Mise à jour vers PHP 7.4
        SymfonySetList::SYMFONY_54,         // ⭐ Migration vers Symfony 5.4
        SymfonySetList::SYMFONY_CODE_QUALITY, // ⭐ Bonnes pratiques Symfony
    ]);
};
```

## 🎮 Utilisation : Comment lancer Rector ?

### Mode test (dry-run) :
```bash
# 👀 Voir ce qui va changer sans rien modifier
./vendor/bin/rector process src --dry-run

# 🔍 Avec plus de détails
./vendor/bin/rector process src --dry-run --debug
```

### Mode réel :
```bash
# ✨ Appliquer les changements
./vendor/bin/rector process src

# 🚀 Sur un fichier spécifique
./vendor/bin/rector process src/Controller/MonController.php
```

## 📚 Exemples concrets de migrations

### 1️⃣ Exemple : Migration d'un Controller

**👉 Avant (Symfony 4) :**
```php
<?php

namespace App\Controller;

/**
 * @Route("/api")
 */
class ApiController extends Controller
{
    /**
     * @Route("/users", name="api_users")
     * @return Response
     */
    public function getUsers()
    {
        return $this->json($this->getDoctrine()->getRepository(User::class)->findAll());
    }
}
```

**👉 Après (Symfony 5/6) :**
```php
<?php

namespace App\Controller;

#[Route('/api')]  // ✨ Nouvelle syntaxe d'attribut
class ApiController extends AbstractController  // 👈 Nouveau controller de base
{
    #[Route('/users', name: 'api_users')]
    public function getUsers(
        ManagerRegistry $doctrine  // 👈 Injection de dépendance
    ): Response  // 👈 Type de retour explicite
    {
        return $this->json(
            $doctrine->getRepository(User::class)->findAll()
        );
    }
}
```

### 2️⃣ Exemple : Migration d'une Entity

**👉 Avant :**
```php
<?php

namespace App\Entity;

/**
 * @ORM\Entity(repositoryClass="App\Repository\UserRepository")
 */
class User
{
    /**
     * @ORM\Id()
     * @ORM\GeneratedValue()
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=255)
     */
    private $email;
}
```

**👉 Après :**
```php
<?php

namespace App\Entity;

#[ORM\Entity(repositoryClass: UserRepository::class)]  // ✨ Attribut moderne
class User
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private ?int $id = null;  // 👈 Type explicite + nullable

    #[ORM\Column(length: 255)]
    private string $email;  // 👈 Type explicite
}
```

## 🎯 Cas pratiques : Configuration pour des besoins spécifiques

### 1️⃣ Migration PHP 7.4 vers PHP 8.0 :
```php
$rectorConfig->sets([
    LevelSetList::UP_TO_PHP_80,  // ⬆️ Mise à jour vers PHP 8.0
]);
```

### 2️⃣ Migration Symfony avec des règles spécifiques :
```php
$rectorConfig->sets([
    SymfonySetList::SYMFONY_54,         // 🎯 Base Symfony 5.4
    SymfonySetList::SYMFONY_52_VALIDATOR_ATTRIBUTES,  // ✨ Validateurs en attributs
    SymfonySetList::SYMFONY_CODE_QUALITY,  // 🎨 Qualité de code
]);
```

### 3️⃣ Exclure certains fichiers :
```php
$rectorConfig->skip([
    __DIR__ . '/src/Legacy/*.php',          // 🚫 Ignorer le code legacy
    __DIR__ . '/src/Generated/*.php',       // 🚫 Ignorer le code généré
    __DIR__ . '/src/Kernel.php',            // 🚫 Ignorer un fichier spécifique
]);
```

## 🔧 Astuces pour déboguer

### Si vous avez des erreurs de mémoire :
```bash
# 💪 Augmenter la mémoire disponible
php -d memory_limit=-1 vendor/bin/rector process src
```

### Pour plus de détails sur les changements :
```bash
# 🔍 Mode verbose
vendor/bin/rector process src --dry-run --debug
```

### Pour ignorer certaines règles :
```php
$rectorConfig->skip([
    \Rector\Symfony\Rector\ClassMethod\ParamTypeFromRouteRequiredRegexRector::class,
]);
```

## 📝 Bonnes pratiques

1. 🔄 **Toujours faire un commit avant** :
```bash
git checkout -b rector-migration
```

2. 📋 **Procéder par étapes** :
   - D'abord les règles PHP
   - Ensuite les règles Symfony
   - Enfin les règles spécifiques

3. ✅ **Tester après chaque étape** :
```bash
# Tests unitaires
php bin/phpunit

# Tests fonctionnels
symfony server:start
```