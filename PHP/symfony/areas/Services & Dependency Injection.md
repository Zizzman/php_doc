### Documentation Symfony : Services & Dependency Injection

#### Description

Le **Service Container** et l'**Injection de Dépendances (DI)** sont des concepts clés dans Symfony. Le conteneur de services permet de centraliser et gérer la configuration des services, tandis que l'injection de dépendances consiste à fournir à un objet ses dépendances au lieu de les créer lui-même. Symfony utilise le conteneur de services pour injecter ces dépendances automatiquement.

---

### Concepts de base

1. **Service** : Un service est une classe qui effectue une tâche spécifique dans l'application. Par exemple, un service peut être un gestionnaire de base de données, un service de mail ou un service de calculs.
    
2. **Injection de Dépendances (DI)** : L'injection de dépendances est un design pattern qui consiste à passer les dépendances d'une classe par son constructeur ou un setter plutôt que de les créer à l'intérieur de la classe elle-même.
    
3. **Conteneur de Services** : Symfony utilise un conteneur de services pour gérer l'instanciation des objets. Le conteneur est responsable de la gestion de la création, de la configuration et de l'injection des dépendances.
    

---

### Déclaration des Services

Les services peuvent être déclarés de deux manières principales : par **autoconfiguration** (automatique) ou dans le fichier **services.yaml**.

#### Exemple de Déclaration Manuelle (services.yaml)

```yaml
# config/services.yaml
services:
    App\Service\Mailer:
        arguments:
            $mailerTransport: '@mailer_transport_service'
```

#### Exemple de Déclaration avec Autoconfiguration

```yaml
# config/services.yaml
services:
    App\Service\Mailer:
        autowire: true  # Active l'autowiring
        autoconfigure: true  # Active la configuration automatique des services
```

---

### Autowiring

L'**autowiring** permet à Symfony de deviner automatiquement quelles dépendances injecter dans un service. Il suffit de déclarer le type des dépendances dans le constructeur de votre service, et Symfony les injecte automatiquement.

Exemple avec autowiring :

```php
// src/Service/Mailer.php
namespace App\Service;

use Symfony\Component\Mailer\MailerInterface;

class Mailer
{
    private $mailer;

    // Symfony injecte automatiquement MailerInterface
    public function __construct(MailerInterface $mailer)
    {
        $this->mailer = $mailer;
    }

    public function sendMail($recipient, $message)
    {
        // Utiliser $this->mailer pour envoyer un e-mail
    }
}
```

Dans cet exemple, Symfony injecte automatiquement le service `MailerInterface` dans le service `Mailer`.

---

### Injection via le Constructeur

L'injection par le constructeur est la méthode la plus courante pour fournir des dépendances à un service.

```php
// src/Service/MyService.php
namespace App\Service;

class MyService
{
    private $logger;

    public function __construct(\Psr\Log\LoggerInterface $logger)
    {
        $this->logger = $logger;
    }

    public function performAction()
    {
        $this->logger->info('Action performed!');
    }
}
```

Le service `LoggerInterface` sera automatiquement injecté par Symfony, en fonction des besoins.

---

### Injection via les Setters

Il est également possible d'injecter des dépendances via des méthodes **setter**.

```php
// src/Service/MyService.php
namespace App\Service;

class MyService
{
    private $logger;

    // Injection via setter
    public function setLogger(\Psr\Log\LoggerInterface $logger)
    {
        $this->logger = $logger;
    }

    public function performAction()
    {
        $this->logger->info('Action performed!');
    }
}
```

---

### Utilisation de Services dans un Contrôleur

Les services sont automatiquement injectés dans les contrôleurs grâce à l'autowiring ou en les déclarant explicitement.

#### Exemple avec Autowiring dans un Contrôleur :

```php
// src/Controller/MyController.php
namespace App\Controller;

use App\Service\Mailer;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class MyController extends AbstractController
{
    private $mailer;

    public function __construct(Mailer $mailer)
    {
        $this->mailer = $mailer;
    }

    public function sendEmail()
    {
        // Utiliser le service Mailer
        $this->mailer->sendMail('example@example.com', 'Hello World');
    }
}
```

#### Exemple sans Autowiring dans un Contrôleur :

```php
// src/Controller/MyController.php
namespace App\Controller;

use App\Service\Mailer;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class MyController extends AbstractController
{
    private $mailer;

    public function __construct(Mailer $mailer)
    {
        $this->mailer = $mailer;
    }

    public function sendEmail()
    {
        $this->mailer->sendMail('example@example.com', 'Hello World');
    }
}
```

---

### Services de Périphérie (Singleton vs Prototype)

Par défaut, les services sont créés en **singleton**, c'est-à-dire qu'une seule instance du service est partagée. Cependant, vous pouvez créer des services avec une portée **prototype** pour que Symfony en crée une nouvelle instance à chaque fois qu'ils sont injectés.

Exemple de service **prototype** :

```yaml
# config/services.yaml
services:
    App\Service\RandomService:
        scope: prototype
```

Cela signifie qu'à chaque appel ou injection de ce service, Symfony crée une nouvelle instance de `RandomService`.

---

### Accéder aux Services dans le Code

Il est également possible d'accéder manuellement aux services via le conteneur de services dans un contrôleur ou un autre service. Cependant, cela est généralement déconseillé car il contourne l'injection de dépendances.

```php
// Dans un contrôleur
$myService = $this->container->get(MyService::class);
```

---

### Conseils et Bonnes Pratiques

- **Privilégier l'injection par le constructeur** : Utilisez l'injection par constructeur pour garantir que vos services ont toutes les dépendances nécessaires dès leur création.
- **Evitez l'accès direct au conteneur** : Utilisez l'injection de dépendances plutôt que d'accéder directement aux services via le conteneur.
- **Séparer les responsabilités** : Utilisez des services pour encapsuler des fonctionnalités spécifiques et réutilisables dans votre application, et ne mettez pas trop de logique dans les contrôleurs.
- **Utilisez des groupes de services** : Utilisez les tags dans le fichier `services.yaml` pour organiser vos services et créer des services groupés si nécessaire.

---

### Ressources complémentaires

- [Documentation Symfony : Services](https://symfony.com/doc/current/service_container.html)
- [Injection de dépendances Symfony](https://symfony.com/doc/current/service_container.html#dependency-injection)