### Documentation Symfony : Messenger

#### Description

Symfony Messenger est un composant permettant de gérer l'envoi, la réception et le traitement des messages dans une application. Il facilite l'implémentation de l'architecture asynchrone, en utilisant des files d'attente et des transports comme RabbitMQ, SQS, ou même des bases de données.

---

### Installation du Composant Messenger

Pour installer Messenger dans votre projet Symfony, utilisez Composer :

```bash
composer require symfony/messenger
```

Cela ajoute le composant Messenger ainsi que les dépendances nécessaires au projet.

---

### Configuration de Messenger

La configuration de Messenger se fait principalement dans le fichier `config/packages/messenger.yaml`. Vous pouvez définir les différents **transports** (systèmes de files d'attente) et **routes** (comment les messages sont acheminés).

Exemple de configuration de transport utilisant Doctrine :

```yaml
framework:
    messenger:
        buses:
            default:
                default_middleware: true
                middleware:
                    - 'Symfony\Component\Messenger\Middleware\AddBusNameStamp'
        routing:
            'App\Message\YourMessage': async # Route vers un transport spécifique
        transports:
            async: '%env(MESSENGER_TRANSPORT_DSN)%'
```

- **Transports** : définissent où et comment les messages seront envoyés (par exemple, vers RabbitMQ, une base de données ou un autre système).
- **Buses** : organisent la gestion des messages, en permettant de définir les bus de messages (par exemple, `default`, `command`, `event`).
- **Routing** : permet de spécifier quel transport utiliser pour chaque type de message.

---

### Création d'un Message

Un message dans Symfony Messenger est une simple classe PHP. Vous pouvez créer un message en définissant ses propriétés et ses méthodes nécessaires.

Exemple d'une classe `Message` :

```php
namespace App\Message;

class MyMessage
{
    private string $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent(): string
    {
        return $this->content;
    }
}
```

---

### Envoi d'un Message

Pour envoyer un message, vous utilisez le service `MessageBusInterface`, qui permet de dispatcher les messages vers leurs handlers.

```php
use Symfony\Component\Messenger\MessageBusInterface;
use App\Message\MyMessage;

class MyController
{
    private MessageBusInterface $bus;

    public function __construct(MessageBusInterface $bus)
    {
        $this->bus = $bus;
    }

    public function sendMessage()
    {
        $message = new MyMessage('Hello, Symfony Messenger!');
        $this->bus->dispatch($message);
    }
}
```

---

### Traitement des Messages (Handler)

Un **handler** est une classe responsable du traitement d'un message. Un handler est lié à un message spécifique.

Exemple d'un handler pour le message `MyMessage` :

```php
namespace App\Handler;

use App\Message\MyMessage;
use Symfony\Component\Messenger\Handler\MessageHandlerInterface;

class MyMessageHandler implements MessageHandlerInterface
{
    public function __invoke(MyMessage $message)
    {
        // Traitement du message
        echo 'Message reçu : ' . $message->getContent();
    }
}
```

Le handler doit implémenter l'interface `MessageHandlerInterface` et définir une méthode `__invoke()` qui sera appelée pour traiter le message.

---

### Traitement Asynchrone

Symfony Messenger permet de traiter les messages de manière **asynchrone** en les envoyant dans une file d'attente (comme RabbitMQ, SQS, etc.) et en les traitant via un **worker**.

Configuration de la route pour le transport `async` :

```yaml
framework:
    messenger:
        routing:
            'App\Message\MyMessage': async
        transports:
            async: 'amqp://localhost'
```

Exécution du worker qui traite les messages asynchrones :

```bash
php bin/console messenger:consume async
```

Le worker consommera les messages de la file d'attente et exécutera les handlers correspondants.

---

### Middleware

Les **middlewares** sont des composants qui peuvent être ajoutés à la pile de traitement des messages. Chaque message passera par les middlewares avant d'être envoyé ou après sa réception.

Exemple de middleware personnalisé :

```php
namespace App\Middleware;

use Symfony\Component\Messenger\Envelope;
use Symfony\Component\Messenger\Middleware\StackInterface;
use Symfony\Component\Messenger\Middleware\MiddlewareInterface;

class MyMiddleware implements MiddlewareInterface
{
    public function handle(Envelope $envelope, StackInterface $stack): Envelope
    {
        // Code exécuté avant le traitement du message
        echo 'Avant le traitement du message';

        $envelope = $stack->next()->handle($envelope, $stack);

        // Code exécuté après le traitement du message
        echo 'Après le traitement du message';

        return $envelope;
    }
}
```

Les middlewares peuvent être configurés dans `messenger.yaml` et sont utilisés pour effectuer des tâches comme la journalisation, la gestion des erreurs ou des transactions.

---

### Environnement de Production : Worker

En production, vous devez lancer un worker pour consommer les messages de manière continue. Utilisez la commande suivante pour démarrer un worker en arrière-plan :

```bash
php bin/console messenger:consume async --time-limit=3600
```

Cette commande consommera les messages de la file `async` pendant une heure, par exemple.

---

### Conseils et Bonnes Pratiques

- **Traitement des erreurs** : Utilisez les middlewares pour gérer les erreurs ou les échecs de traitement, et considérez l'utilisation d'un transport comme un système de `retry` en cas d'échec.
- **Performance** : Les workers doivent être exécutés en permanence dans des environnements de production pour garantir un traitement rapide des messages. Utilisez des outils comme `supervisord` pour gérer le processus en arrière-plan.
- **Scalabilité** : Lorsque vous travaillez avec un grand nombre de messages, assurez-vous d'utiliser un transport scalable (comme RabbitMQ ou SQS) et d'avoir des workers qui consomment les messages en parallèle.
- **Sécurisation des messages** : Si vous travaillez avec des données sensibles, assurez-vous que les messages sont cryptés ou qu'ils ne contiennent pas d'informations privées.

---

### Commandes Utiles

- `php bin/console messenger:consume async` : Démarre un worker pour consommer les messages.
- `php bin/console messenger:flush` : Vide les messages qui n'ont pas encore été traités dans les files d'attente.
- `php bin/console messenger:show` : Affiche les informations de transport pour les messages.

---

### Ressources Complémentaires

- [Symfony Messenger Documentation](https://symfony.com/doc/current/messenger.html)
- [RabbitMQ Symfony Messenger Example](https://symfony.com/doc/current/messenger/rabbitmq.html)