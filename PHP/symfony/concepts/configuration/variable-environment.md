# Gérer les variables d'environnement avec `.env`

## Description
Les variables d'environnement permettent de configurer votre application Symfony en fonction de l'environnement dans lequel elle s'exécute (local, staging, production). Ces variables sont généralement stockées dans des fichiers `.env` pour éviter de les inclure directement dans le code source.

---

## Fichiers `.env` dans Symfony
Symfony utilise plusieurs fichiers `.env` :
- `.env` : Fichier principal contenant les valeurs par défaut.
- `.env.local` : Fichier pour les valeurs spécifiques à l'environnement local (non versionné).
- `.env.prod` : Fichier pour les valeurs spécifiques à la production.
- `.env.test` : Fichier pour l'environnement de test.

---

## Définir des variables dans `.env`

### Exemple basique :
```dotenv
# Fichier .env
APP_ENV=dev
APP_SECRET=your-secret-key
DATABASE_URL=mysql://user:password@127.0.0.1:3306/my_database
````

---

## Accéder aux variables dans Symfony

### **1. Depuis un contrôleur ou un service**

Vous pouvez accéder aux variables d'environnement via la méthode `getParameter()`.

#### Exemple :

```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class ExampleController extends AbstractController {
    public function index(): Response {
        $appEnv = $this->getParameter('env(APP_ENV)');
        $appSecret = $this->getParameter('env(APP_SECRET)');

        return new Response("Environment: $appEnv, Secret: $appSecret");
    }
}
```

---

### **2. Injecter une variable dans un service**

Vous pouvez injecter directement une variable d'environnement dans un service.

#### Configuration dans `services.yaml` :

```yaml
services:
    App\Service\MyService:
        arguments:
            $databaseUrl: '%env(DATABASE_URL)%'
```

#### Exemple de service :

```php
namespace App\Service;

class MyService {
    private $databaseUrl;

    public function __construct(string $databaseUrl) {
        $this->databaseUrl = $databaseUrl;
    }

    public function getDatabaseUrl(): string {
        return $this->databaseUrl;
    }
}
```

---

## Utiliser des valeurs par défaut

Vous pouvez définir des valeurs par défaut pour les variables non spécifiées.

#### Exemple dans `.env` :

```dotenv
DATABASE_PORT=3306
```

#### Exemple d’utilisation :

```yaml
parameters:
    database_port: '%env(default:3306:DATABASE_PORT)%'
```

---

## Sécurité des variables sensibles

### **1. Ne jamais inclure `.env` en production**

Utilisez un gestionnaire de variables d'environnement comme :

- **Docker** (via `docker-compose.yml`).
- **Heroku** (via leur interface d’administration).
- **AWS Parameter Store** ou **Vault**.

### **2. Exemple avec Docker**

Fichier `docker-compose.yml` :

```yaml
services:
  app:
    environment:
      - APP_ENV=prod
      - DATABASE_URL=mysql://user:password@db:3306/my_database
```

---

## Tester les variables d'environnement

### Afficher toutes les variables disponibles

Utilisez la commande suivante :

```bash
php bin/console debug:container --env-vars
```

---

## Bonnes pratiques

1. **Utilisez `.env.local` pour vos configurations locales.**
    
    - Ne versionnez jamais ce fichier pour éviter les conflits.
2. **Ne stockez pas de secrets directement dans `.env`.**
    
    - Utilisez des solutions comme **Symfony Secrets** pour gérer les informations sensibles :
        
        ```bash
        php bin/console secrets:set APP_SECRET
        php bin/console secrets:list
        ```
        
3. **Gérez vos environnements correctement.**
    
    - Définissez des valeurs spécifiques dans `.env.prod` ou `.env.test` pour les environnements appropriés.

---

## Liens connexes

- [[configuration-overview]] : Vue d’ensemble de la configuration Symfony.
- [[services-configuration]] : Injecter des variables d’environnement dans les services.
- [[get-parameter]] : Accéder aux paramètres dans Symfony.
- [[security-configuration]] : Gestion des secrets avec Symfony.

---

## Ressources supplémentaires

- [Documentation officielle : Variables d'environnement](https://symfony.com/doc/current/configuration.html#overriding-environment-values)
- [Documentation Symfony Secrets](https://symfony.com/doc/current/configuration/secrets.html)

---

## Tags

#symfony #configuration #environment-variables #env #secrets #parameters