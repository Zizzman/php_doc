### Documentation Symfony : Bundles

#### Description

Un **Bundle** dans Symfony est une unité modulaire qui regroupe des fonctionnalités spécifiques à une application. Un bundle peut contenir des contrôleurs, des services, des configurations, des entités, des templates, des routes, et d'autres ressources liées à une fonctionnalité spécifique. L'utilisation de bundles permet d'organiser et de réutiliser facilement du code au sein d'une application ou à travers plusieurs projets.

---

### Types de Bundles

1. **Bundles Internes** : Les bundles créés spécifiquement pour une application Symfony. Ils sont souvent utilisés pour regrouper une fonctionnalité dans un module séparé, mais au sein du même projet.
    
2. **Bundles Externes (ou tiers)** : Les bundles fournis par la communauté ou des entreprises pour ajouter des fonctionnalités à une application Symfony. Ces bundles peuvent être installés via Composer.
    
3. **Bundles Symfony Standards** : Symfony lui-même inclut des bundles standard pour des fonctionnalités de base, tels que :
    
    - **FrameworkBundle** : Contient les composants de base de Symfony.
    - **SecurityBundle** : Permet la gestion de la sécurité (authentification, autorisation).
    - **TwigBundle** : Permet l'intégration du moteur de templates Twig.
    - **DoctrineBundle** : Intégration de Doctrine ORM avec Symfony.

---

### Créer un Bundle

Les bundles sont généralement créés en suivant une structure spécifique dans le dossier `src/` de l'application Symfony.

#### Exemple de Structure d'un Bundle

```bash
src/
  MyBundle/
    Controller/
      DefaultController.php
    DependencyInjection/
      MyBundleExtension.php
    Resources/
      config/
        services.yaml
      views/
        default/index.html.twig
    MyBundle.php
```

- **Controller/** : Contient les contrôleurs spécifiques au bundle.
- **DependencyInjection/** : Contient les classes de configuration pour l'injection de dépendances.
- **Resources/** : Contient les ressources (configurations, templates, traductions, etc.).
- **MyBundle.php** : Le fichier principal du bundle.

#### Exemple de Création d'un Bundle Simple

```php
// src/MyBundle/MyBundle.php
namespace App\MyBundle;

use Symfony\Component\HttpKernel\Bundle\Bundle;

class MyBundle extends Bundle
{
    public function getContainerExtension()
    {
        // Retourne l'extension de configuration
    }
}
```

Le fichier `MyBundle.php` est la classe principale qui hérite de `Symfony\Component\HttpKernel\Bundle\Bundle`. Cette classe sert de point d'entrée pour Symfony afin d’enregistrer le bundle.

---

### Utiliser un Bundle Externe

Les bundles externes peuvent être installés via Composer et ajoutés à la configuration de Symfony dans le fichier `config/bundles.php`.

#### Exemple d'Installation d'un Bundle via Composer

```bash
composer require doctrine/orm
```

Une fois installé, le bundle sera automatiquement ajouté à `config/bundles.php` :

```php
// config/bundles.php
return [
    Symfony\Bundle\FrameworkBundle\FrameworkBundle::class => ['all' => true],
    Doctrine\Bundle\DoctrineBundle\DoctrineBundle::class => ['all' => true],
];
```

---

### Activer un Bundle

Symfony charge automatiquement les bundles listés dans `config/bundles.php`. Toutefois, vous pouvez contrôler la charge des bundles en fonction de l’environnement (par exemple, `dev`, `prod`).

#### Exemple d'Activation d'un Bundle dans un Environnement Spécifique

```php
// config/bundles.php
return [
    Symfony\Bundle\FrameworkBundle\FrameworkBundle::class => ['all' => true],
    Doctrine\Bundle\DoctrineBundle\DoctrineBundle::class => ['dev' => true],
];
```

Dans cet exemple, le bundle `DoctrineBundle` ne sera activé que dans l'environnement `dev`.

---

### Utilisation des Bundles

Les bundles peuvent être utilisés pour organiser des fonctionnalités dans une application. Par exemple, un bundle dédié à l'authentification contiendrait des contrôleurs, des services, et des configurations de sécurité.

#### Exemple d'Utilisation d'un Bundle Personnalisé

1. **Créer un Contrôleur dans le Bundle**

```php
// src/MyBundle/Controller/DefaultController.php
namespace App\MyBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class DefaultController extends AbstractController
{
    public function index(): Response
    {
        return $this->render('@MyBundle/default/index.html.twig');
    }
}
```

2. **Créer une Vue Twig pour le Bundle**

```twig
{# src/MyBundle/Resources/views/default/index.html.twig #}
<h1>Bienvenue dans le Bundle !</h1>
```

3. **Déclarer une Route pour le Contrôleur**

```yaml
# config/routes.yaml
my_bundle_default:
    path: /my-bundle
    controller: App\MyBundle\Controller\DefaultController::index
```

---

### Conseils et Bonnes Pratiques

- **Organisation des Bundles** : Organisez les bundles par fonctionnalité (ex : gestion des utilisateurs, gestion des produits, etc.).
- **Ne pas trop fragmenter** : Si le code n'est pas réutilisable dans d'autres projets, il peut être préférable de ne pas créer un bundle.
- **Composer pour les bundles externes** : Installez des bundles externes via Composer et assurez-vous qu'ils sont bien configurés dans `config/bundles.php`.
- **Surveiller la compatibilité des versions** : Vérifiez que les versions des bundles externes sont compatibles avec la version de Symfony utilisée dans votre projet.

---

### Ressources Complémentaires

- [Documentation Symfony : Bundles](https://symfony.com/doc/current/bundles.html)
- [Composant Bundles sur GitHub](https://github.com/symfony/symfony)