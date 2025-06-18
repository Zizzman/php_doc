### Documentation Symfony : Event Dispatcher

#### Description

L'**Event Dispatcher** (ou **distributeur d'événements**) est un composant de Symfony permettant de gérer la communication entre les différentes parties d'une application en utilisant des événements et des écouteurs d'événements. Ce mécanisme permet de séparer les préoccupations et d'ajouter des fonctionnalités à votre application sans toucher au code existant.

---

### Concepts de Base

1. **Événements** : Un événement est une notification qu'un changement ou une action importante s'est produite dans l'application. Un événement contient souvent des informations sur ce qui s'est passé.
    
2. **Écouteurs d'événements** : Un écouteur est une fonction ou une méthode qui "écoute" un événement particulier. Lorsqu'un événement est déclenché, l'écouteur réagit à l'événement.
    
3. **Distributeur d'événements** : Le distributeur est responsable de l'enregistrement des écouteurs et de la gestion de l'exécution des écouteurs lorsqu'un événement est déclenché.
    

---

### Déclencher un Événement

Pour déclencher un événement, vous utilisez la méthode `dispatch()` du distributeur d'événements.

#### Exemple de Déclenchement d'Événement

```php
// src/Event/CustomEvent.php
namespace App\Event;

use Symfony\Contracts\EventDispatcher\Event;

class CustomEvent extends Event
{
    public const NAME = 'app.custom_event';

    private $message;

    public function __construct($message)
    {
        $this->message = $message;
    }

    public function getMessage()
    {
        return $this->message;
    }
}
```

```php
// src/Controller/CustomController.php
namespace App\Controller;

use App\Event\CustomEvent;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\EventDispatcher\EventDispatcherInterface;
use Symfony\Component\HttpFoundation\Response;

class CustomController extends AbstractController
{
    public function triggerEvent(EventDispatcherInterface $eventDispatcher)
    {
        $event = new CustomEvent('Hello, Event!');
        $eventDispatcher->dispatch($event, CustomEvent::NAME);

        return new Response('Event triggered!');
    }
}
```

Dans cet exemple, un événement personnalisé `CustomEvent` est déclenché dans un contrôleur.

---

### Créer un Écouteur d'Événement

Les écouteurs réagissent aux événements déclenchés. Vous devez les configurer dans le fichier `services.yaml`.

#### Exemple de Création d'un Écouteur

```php
// src/EventListener/CustomEventListener.php
namespace App\EventListener;

use App\Event\CustomEvent;

class CustomEventListener
{
    public function onCustomEvent(CustomEvent $event)
    {
        // Action effectuée lors du déclenchement de l'événement
        echo $event->getMessage();
    }
}
```

#### Configurer l'Écouteur dans `services.yaml`

```yaml
# config/services.yaml
services:
    App\EventListener\CustomEventListener:
        tags:
            - { name: 'kernel.event_listener', event: 'app.custom_event', method: 'onCustomEvent' }
```

L'écouteur `CustomEventListener` réagit à l'événement `app.custom_event` en appelant la méthode `onCustomEvent`.

---

### Paramètres et Retour de `dispatch()`

La méthode `dispatch()` permet de déclencher un événement et de passer des données supplémentaires à l'événement. Elle retourne l'objet événement, ce qui permet de chaîner les appels.

#### Exemple de Passage de Paramètres et Chaining

```php
$event = new CustomEvent('Hello, Event!');
$returnedEvent = $eventDispatcher->dispatch($event, CustomEvent::NAME);

// Vous pouvez accéder aux données de l'événement après sa propagation
echo $returnedEvent->getMessage();
```

---

### Événements avec Priorité

Vous pouvez spécifier la priorité d'un écouteur d'événement. Par défaut, la priorité est `0`, mais vous pouvez la modifier pour exécuter un écouteur avant ou après d'autres écouteurs.

```yaml
# config/services.yaml
services:
    App\EventListener\CustomEventListener:
        tags:
            - { name: 'kernel.event_listener', event: 'app.custom_event', method: 'onCustomEvent', priority: 10 }
```

Dans cet exemple, l'écouteur sera exécuté avant les autres écouteurs avec une priorité inférieure.

---

### Événements de Type Subscriber

Un **Event Subscriber** (ou abonné à un événement) est une autre manière d'écouter des événements. Un subscriber est une classe qui écoute plusieurs événements. Vous devez implémenter l'interface `EventSubscriberInterface`.

#### Exemple de Subscriber

```php
// src/EventSubscriber/CustomEventSubscriber.php
namespace App\EventSubscriber;

use App\Event\CustomEvent;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;

class CustomEventSubscriber implements EventSubscriberInterface
{
    public static function getSubscribedEvents()
    {
        return [
            CustomEvent::NAME => 'onCustomEvent',
        ];
    }

    public function onCustomEvent(CustomEvent $event)
    {
        // Logic to handle event
        echo $event->getMessage();
    }
}
```

#### Configurer le Subscriber dans `services.yaml`

```yaml
# config/services.yaml
services:
    App\EventSubscriber\CustomEventSubscriber:
        tags:
            - { name: 'kernel.event_subscriber' }
```

---

### Conseils et Bonnes Pratiques

- **Séparer les responsabilités** : Utilisez des événements pour séparer les préoccupations dans votre application. Par exemple, utilisez un événement pour notifier que des données ont été mises à jour, puis laissez un autre service ou une autre partie de l'application gérer ce changement.
- **Utilisation de Priorités** : Gérez l'ordre d'exécution des écouteurs en utilisant la priorité. Cela vous permet de garantir que certains écouteurs sont exécutés avant ou après d'autres.
- **Abonnés aux événements pour plus de flexibilité** : Les abonnés sont idéaux si vous devez écouter plusieurs événements avec une seule classe.
- **Testez vos événements et écouteurs** : Assurez-vous que les événements et leurs écouteurs sont correctement testés pour garantir leur bon fonctionnement.

---

### Ressources Complémentaires

- [Documentation Symfony : Event Dispatcher](https://symfony.com/doc/current/components/event_dispatcher.html)
- [EventDispatcherInterface](https://symfony.com/doc/current/reference/api/Symfony_Component_EventDispatcher_EventDispatcherInterface.html)