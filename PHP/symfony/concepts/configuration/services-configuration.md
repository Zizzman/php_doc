# Configuration des services dans `services.yaml`

## Description
Le fichier `services.yaml` est l'un des principaux fichiers de configuration dans Symfony. Il est utilisé pour définir, configurer et personnaliser les services de votre application. Les services sont des objets gérés par le conteneur de services Symfony.

---

## Syntaxe de base
Un service est défini en utilisant la clé `services`. Vous pouvez spécifier les classes, arguments, et autres paramètres.

### Exemple simple :
```yaml
services:
    App\Service\MyService: ~
````

Cela enregistre la classe `App\Service\MyService` comme un service dans le conteneur.

---

## Exemples d'utilisation

### **1. Enregistrer un service avec des arguments**

Vous pouvez spécifier les arguments que le service attend dans son constructeur.

#### Exemple :

```yaml
services:
    App\Service\MyService:
        arguments:
            $apiKey: '123456'
            $debug: true
```

#### Exemple de classe :

```php
namespace App\Service;

class MyService {
    private $apiKey;
    private $debug;

    public function __construct(string $apiKey, bool $debug) {
        $this->apiKey = $apiKey;
        $this->debug = $debug;
    }

    public function getApiKey(): string {
        return $this->apiKey;
    }
}
```

---

### **2. Utiliser un paramètre comme argument**

Vous pouvez utiliser des paramètres définis dans `parameters` ou des variables d’environnement.

#### Exemple avec un paramètre défini :

```yaml
parameters:
    app.api_key: '123456'

services:
    App\Service\MyService:
        arguments:
            $apiKey: '%app.api_key%'
```

---

### **3. Autowire : Activer l'injection automatique**

L'injection automatique permet à Symfony de deviner les dépendances à partir du type des arguments.

#### Exemple :

```yaml
services:
    App\Service\MyService:
        autowire: true
```

Avec l’option `autowire: true`, Symfony détecte et injecte automatiquement les services requis.

---

### **4. Public vs Private**

Par défaut, tous les services sont privés, ce qui signifie qu'ils ne peuvent pas être directement récupérés depuis le conteneur. Vous pouvez rendre un service public si nécessaire.

#### Exemple :

```yaml
services:
    App\Service\MyService:
        public: true
```

---

### **5. Alias de service**

Vous pouvez créer un alias pour un service.

#### Exemple :

```yaml
services:
    App\Service\MyService: ~
    my_service_alias: '@App\Service\MyService'
```

Cela vous permet d'accéder au service via `my_service_alias`.

---

### **6. Configurer un service existant**

Symfony offre des services prédéfinis que vous pouvez configurer ou surcharger.

#### Exemple : Configurer le service `mailer`

```yaml
services:
    mailer:
        arguments:
            $defaultFrom: 'noreply@example.com'
```

---

### **7. Ajouter une méthode de configuration (calls)**

Vous pouvez appeler une méthode spécifique après la création d’un service.

#### Exemple :

```yaml
services:
    App\Service\MyService:
        calls:
            - method: 'setLogger'
              arguments:
                  - '@logger'
```

#### Exemple de classe :

```php
namespace App\Service;

use Psr\Log\LoggerInterface;

class MyService {
    private $logger;

    public function setLogger(LoggerInterface $logger) {
        $this->logger = $logger;
    }
}
```

---

### **8. Inclure d'autres fichiers de configuration**

Vous pouvez diviser la configuration des services en plusieurs fichiers pour une meilleure organisation.

#### Exemple :

Créer un fichier `config/services/custom_services.yaml` :

```yaml
services:
    App\Service\CustomService: ~
```

Inclure ce fichier dans `services.yaml` :

```yaml
imports:
    - { resource: 'services/custom_services.yaml' }
```

---

## Bonnes pratiques

1. **Utilisez l’autowire et l’autoconfigure** pour réduire la complexité de la configuration :
    
    ```yaml
    services:
        _defaults:
            autowire: true
            autoconfigure: true
            public: false
    ```
    
2. **Centralisez les paramètres globaux** dans `parameters` pour les rendre faciles à modifier.
    
3. **Utilisez des alias significatifs** pour améliorer la lisibilité du code.
    

---

## Liens connexes

- [[configuration-overview]] : Introduction à la configuration dans Symfony.
- [[get-parameter]] : Comment accéder aux paramètres définis.
- [[environment-variables]] : Gestion des variables d’environnement.
- [[parameter-management]] : Approfondir l’utilisation des paramètres.

---

## Ressources supplémentaires

- [Documentation officielle des services](https://symfony.com/doc/current/service_container.html)
- [Documentation sur les paramètres](https://symfony.com/doc/current/configuration.html#configuration-parameters)

---

## Tags

#symfony #services #configuration #yaml #parameters #autowire #services 
