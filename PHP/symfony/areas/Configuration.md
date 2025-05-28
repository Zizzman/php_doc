### Documentation Symfony : Configuration

#### Description

La **configuration** dans Symfony est un aspect clé du framework qui permet de personnaliser et d'adapter le comportement de l'application en fonction des besoins spécifiques. La configuration peut être définie dans plusieurs formats, dont YAML, XML et PHP, et elle s'intègre principalement dans les fichiers de configuration du dossier `config/`.

---

### Fichiers de Configuration

Les fichiers de configuration sont utilisés pour définir les services, les routes, la sécurité, les paramètres et plus encore. Voici les principaux fichiers de configuration utilisés dans Symfony :

- **config/packages/** : Contient la configuration des différents paquets installés.
- **config/routes/** : Définit les configurations des routes.
- **config/services.yaml** : Contient la configuration des services (injection de dépendances).
- **config/parameters.yaml** : Contient les paramètres globaux de l'application.

---

### Configuration des Services (services.yaml)

Les services sont des objets que vous pouvez configurer et injecter dans les contrôleurs, commandes, ou autres services. Voici un exemple de configuration de service :

#### Exemple de configuration de service

```yaml
# config/services.yaml
services:
    App\Service\MyService:
        arguments:
            $param1: 'value'
            $param2: '@service_id'
```

- **App\Service\MyService** : C'est la classe que vous voulez configurer.
- **arguments** : Les paramètres passés au constructeur de la classe. Les paramètres peuvent être des valeurs ou des services existants (préfixés par `@`).

---

### Configuration des Routes (routes.yaml)

Les routes permettent de définir les URL qui pointent vers les actions des contrôleurs. Voici un exemple de configuration d'une route :

#### Exemple de configuration de route

```yaml
# config/routes.yaml
home:
    path: /home
    controller: App\Controller\HomeController::index
```

- **home** : Le nom de la route.
- **path** : L'URL associée à la route.
- **controller** : Le contrôleur et la méthode qui traitent la requête.

---

### Paramètres (parameters.yaml)

Les paramètres sont utilisés pour stocker des valeurs qui peuvent être réutilisées dans plusieurs parties de la configuration (par exemple, les informations de base de données, les clés API, etc.).

#### Exemple de configuration de paramètres

```yaml
# config/parameters.yaml
parameters:
    app_name: 'Mon Application'
    app_env: 'prod'
```

Vous pouvez ensuite récupérer ces paramètres dans votre code comme suit :

```php
$param = $this->getParameter('app_name');
```

---

### Configuration de la Sécurité (security.yaml)

La configuration de la sécurité permet de définir les règles d'accès et d'authentification dans l'application.

#### Exemple de configuration de sécurité

```yaml
# config/packages/security.yaml
security:
    encoders:
        App\Entity\User:
            algorithm: bcrypt
    providers:
        in_memory: { memory: ~ }
    firewalls:
        main:
            pattern: ^/
            form_login:
                login_path: login
                check_path: login
            logout:
                path: /logout
```

- **encoders** : Définit l'algorithme de hachage pour le mot de passe.
- **providers** : Définit la source des utilisateurs (par exemple, en mémoire ou une base de données).
- **firewalls** : Définit les règles de sécurité des différentes parties de l'application.

---

### Configuration des Paramètres d'Environnement

Symfony permet d'utiliser des variables d'environnement définies dans un fichier `.env` ou directement dans le système d'exploitation.

#### Exemple de fichier `.env`

```env
APP_ENV=dev
APP_SECRET=your_secret_key
```

#### Exemple de récupération d'une variable d'environnement

```yaml
# config/services.yaml
parameters:
    app_secret: '%env(APP_SECRET)%'
```

- **%env(APP_SECRET)%** : Récupère la valeur de la variable d'environnement `APP_SECRET`.

---

### Conseils et Bonnes Pratiques

- **Organisation claire** : Organisez vos fichiers de configuration dans des répertoires logiques comme `config/packages/` pour les paquets, `config/services.yaml` pour les services, et `config/routes.yaml` pour les routes.
- **Utilisation des paramètres** : Utilisez les paramètres pour éviter de répéter des valeurs dans plusieurs fichiers de configuration.
- **Variables d'environnement** : Utilisez les variables d'environnement pour gérer les configurations sensibles et les configurations spécifiques à l'environnement (dev, prod, test).
- **Nouveaux services** : Pour ajouter de nouveaux services ou de nouvelles dépendances, assurez-vous de les configurer dans le fichier `services.yaml` afin qu'ils soient correctement injectés dans vos contrôleurs et autres services.

---

### Ressources Complémentaires

- [Documentation officielle Symfony : Configuration](https://symfony.com/doc/current/configuration.html)
- [Symfony Configuration Cheat Sheet](https://symfony.com/doc/current/reference/configuration.html)