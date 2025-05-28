# Vue d'ensemble de la configuration dans Symfony

## Description
La configuration est essentielle pour adapter une application Symfony à différents environnements et besoins. Symfony offre une structure claire et des outils puissants pour gérer la configuration, notamment à travers des fichiers YAML, des variables d'environnement, et des paramètres globaux.

---

## Structure de base des fichiers de configuration

Symfony stocke ses fichiers de configuration dans le dossier `config/`. Voici les principaux sous-dossiers et fichiers que vous trouverez :
- **`config/packages/`** : Contient les fichiers de configuration des bundles.
- **`config/routes/`** : Contient les fichiers de définition des routes.
- **`config/services.yaml`** : Définit et configure les services de l'application.
- **`config/packages/*.yaml`** : Configuration spécifique à chaque bundle (par exemple, `doctrine.yaml`, `framework.yaml`).
- **`.env`** : Définit les variables d'environnement.

---

## Configurer des paramètres globaux

### Exemple dans `config/services.yaml` :
```yaml
parameters:
    app.name: "My Symfony App"
    app.version: "1.0.0"
    app.default_language: "en"
````

### Utiliser ces paramètres dans votre application :

- **Dans un contrôleur :**
    
    ```php
    $appName = $this->getParameter('app.name');
    ```
    
- **Dans un service :**
    
    ```yaml
    services:
        App\Service\MyService:
            arguments:
                $appName: '%app.name%'
    ```
    

---

## Variables d'environnement

Symfony utilise des variables d'environnement pour adapter la configuration en fonction des environnements (local, production, test).

### Exemple dans `.env` :

```dotenv
APP_ENV=dev
DATABASE_URL=mysql://user:password@127.0.0.1:3306/my_database
```

### Exemple d’utilisation dans un fichier de configuration :

```yaml
doctrine:
    dbal:
        url: '%env(DATABASE_URL)%'
```

> Voir [[variable-environment]] pour plus de détails.

---

## Configuration des bundles

Les bundles tiers et internes sont configurés via des fichiers YAML dans `config/packages/`.

### Exemple : Configurer Doctrine dans `config/packages/doctrine.yaml` :

```yaml
doctrine:
    dbal:
        url: '%env(resolve:DATABASE_URL)%'
    orm:
        auto_generate_proxy_classes: true
        naming_strategy: doctrine.orm.naming_strategy.underscore
```

### Exemple : Configurer le framework Symfony dans `config/packages/framework.yaml` :

```yaml
framework:
    secret: '%env(APP_SECRET)%'
    csrf_protection: true
    session:
        handler_id: null
```

---

## Gestion des routes

Les routes peuvent être définies dans :

1. **Fichiers YAML** (dans `config/routes/`).
    
    ```yaml
    app_home:
        path: /
        controller: App\Controller\HomeController::index
    ```
    
2. **Annotations** (dans le contrôleur).
    
    ```php
    use Symfony\Component\Routing\Annotation\Route;
    
    class HomeController {
        #[Route('/', name: 'app_home')]
        public function index() { ... }
    }
    ```
    

> Voir [[routing-configuration]] pour plus de détails.

---

## Surcharge de la configuration par environnement

Symfony permet de définir des fichiers spécifiques à un environnement :

- **`config/packages/prod/doctrine.yaml`** : Pour la production.
- **`config/packages/test/framework.yaml`** : Pour les tests.

---

## Tester la configuration

### Commande pour vérifier la configuration :

```bash
php bin/console config:dump-reference <nom_du_bundle>
```

#### Exemple pour Doctrine :

```bash
php bin/console config:dump-reference doctrine
```

---

## Bonnes pratiques

1. **Centralisez les paramètres globaux** dans `parameters` pour une gestion plus simple.
2. **Utilisez des variables d'environnement** pour les données sensibles (voir [[environment-variables]]).
3. **Divisez la configuration par environnement** pour une maintenance simplifiée.

---

## Liens connexes

- [[services-configuration]] : Configuration des services dans `services.yaml`.
- [[variable-environment]] : Gestion des variables d'environnement.
- [[routing-configuration]] : Configuration des routes.
- [[parameters-definition]] : Utiliser des paramètres globaux dans Symfony.
- [[security-configuration]] : Configurer les aspects de sécurité dans `security.yaml`.

---

## Ressources supplémentaires

- [Documentation officielle : Configuration](https://symfony.com/doc/current/configuration.html)
- [Documentation Symfony : Paramètres](https://symfony.com/doc/current/service_container.html#parameters)

---

## Tags

#symfony #configuration #parameters #services #routing #environment