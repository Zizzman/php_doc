# Définir et utiliser des paramètres globaux dans `parameters.yaml`

## Description
Les paramètres globaux sont des valeurs que vous pouvez définir dans Symfony pour les utiliser à travers toute l’application. Ils sont particulièrement utiles pour centraliser des configurations comme des clés API, des valeurs par défaut, ou d’autres constantes utilisées fréquemment.

Ces paramètres sont définis dans le fichier `parameters.yaml` ou dans la section `parameters` de `services.yaml`.

---

## Définir des paramètres globaux

### **1. Dans `parameters.yaml`**
Le fichier `parameters.yaml` est l’endroit principal pour définir des paramètres globaux.

#### Exemple :
```yaml
parameters:
    app.name: "My Symfony App"
    app.version: "1.0.0"
    app.default_language: "en"
````

---

### **2. Dans `services.yaml`**

Si vous n’avez pas de fichier `parameters.yaml`, vous pouvez les définir directement dans `services.yaml` sous la clé `parameters`.

#### Exemple :

```yaml
parameters:
    app.api_key: "123456"
    app.timezone: "UTC"
```

---

## Accéder aux paramètres globaux

### **1. Dans un contrôleur**

Vous pouvez utiliser la méthode `getParameter()` pour accéder à un paramètre.

#### Exemple :

```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class ExampleController extends AbstractController {
    public function index(): Response {
        $appName = $this->getParameter('app.name');
        $appVersion = $this->getParameter('app.version');

        return new Response("App: $appName, Version: $appVersion");
    }
}
```

---

### **2. Dans un service**

Pour utiliser un paramètre dans un service, il faut l’injecter via `services.yaml`.

#### Exemple de configuration dans `services.yaml` :

```yaml
services:
    App\Service\MyService:
        arguments:
            $apiKey: '%app.api_key%'
```

#### Exemple de service :

```php
namespace App\Service;

class MyService {
    private $apiKey;

    public function __construct(string $apiKey) {
        $this->apiKey = $apiKey;
    }

    public function getApiKey(): string {
        return $this->apiKey;
    }
}
```

---

## Utiliser des valeurs dynamiques avec les variables d'environnement

Les paramètres peuvent être définis en fonction des variables d'environnement.

#### Exemple dans `parameters.yaml` :

```yaml
parameters:
    app.database_url: '%env(DATABASE_URL)%'
```

Cela permet d'utiliser une valeur définie dans le fichier `.env` :

```dotenv
DATABASE_URL=mysql://user:password@127.0.0.1:3306/my_database
```

---

## Utiliser des paramètres avec des valeurs par défaut

Symfony permet de définir des valeurs par défaut pour les paramètres.

#### Exemple dans `parameters.yaml` :

```yaml
parameters:
    app.theme_color: ~ # Valeur par défaut "null"
```

#### Exemple avec un test de valeur :

```php
$themeColor = $this->getParameter('app.theme_color') ?? 'blue';
```

---

## Vérifier les paramètres globaux définis

Pour voir tous les paramètres globaux disponibles, utilisez la commande suivante :

```bash
php bin/console debug:container --parameters
```

---

## Exemple pratique : centralisation des clés API

Supposons que vous ayez plusieurs intégrations avec des services tiers nécessitant des clés API.

### **1. Définition des paramètres**

```yaml
parameters:
    app.stripe_api_key: "sk_test_1234567890"
    app.google_api_key: "AIzaSyD-1234567890"
```

### **2. Utilisation dans un service**

```yaml
services:
    App\Service\StripeService:
        arguments:
            $apiKey: '%app.stripe_api_key%'

    App\Service\GoogleService:
        arguments:
            $apiKey: '%app.google_api_key%'
```

---

## Bonnes pratiques

1. **Utilisez des noms explicites :** Préfixez vos paramètres avec `app.` pour les différencier des paramètres internes de Symfony.
2. **Centralisez les constantes :** Les paramètres globaux doivent contenir uniquement les constantes de votre application, pas des données temporaires ou calculées.
3. **Utilisez des variables d'environnement pour les données sensibles :** Ne stockez jamais de mots de passe ou clés API directement dans vos fichiers YAML. Préférez les variables d'environnement.
4. **Testez vos paramètres :** Utilisez la commande `php bin/console debug:container --parameters` pour vérifier les valeurs chargées.

---

## Liens connexes

- [[configuration-overview]] : Vue d'ensemble de la configuration dans Symfony.
- [[services-configuration]] : Injecter des paramètres dans les services.
- [[variable-environment]] : Utiliser des variables d'environnement avec `%env()%`.
- [[getParameter()]] : Accéder aux paramètres globaux dans Symfony.

---

## Ressources supplémentaires

- [Documentation officielle : Paramètres](https://symfony.com/doc/current/service_container.html#parameters)
- [Documentation officielle : Variables d'environnement](https://symfony.com/doc/current/configuration.html#configuration-parameters)

---

## Tags

#symfony #configuration #parameters #globals #services #environment-variables