# Configuration des routes dans Symfony

## Description
Symfony utilise le composant **Routing** pour mapper les URLs aux contrôleurs. Les routes peuvent être configurées de plusieurs façons : dans des fichiers YAML (`routes.yaml`), en utilisant des annotations directement dans les contrôleurs, ou encore dans des fichiers XML ou PHP.

---

## Méthodes de configuration des routes

### **1. Configuration dans `routes.yaml`**
Le fichier `routes.yaml` est utilisé pour définir les routes de manière centralisée.

#### Exemple basique :
```yaml
home:
    path: /
    controller: App\Controller\HomeController::index
````

- **`home`** : Nom unique de la route.
- **`path`** : URL associée à la route.
- **`controller`** : Méthode du contrôleur appelée lorsque cette route est accédée.

---

### **2. Configuration par annotations**

Les annotations permettent de définir des routes directement dans les contrôleurs.

#### Exemple :

```php
namespace App\Controller;

use Symfony\Component\Routing\Annotation\Route;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class HomeController extends AbstractController {
    #[Route('/', name: 'home')]
    public function index() {
        return $this->render('home.html.twig');
    }
}
```

- **`#[Route]`** : Annotation définissant une route.
- **`name`** : Nom de la route.
- **`path`** : (optionnel) Si omis, Symfony utilise `name` comme chemin.

---

### **3. Configuration par fichiers PHP**

Les routes peuvent être définies dans un fichier PHP.

#### Exemple dans `config/routes.php` :

```php
use Symfony\Component\Routing\RouteCollection;
use Symfony\Component\Routing\Route;

$routes = new RouteCollection();
$routes->add('home', new Route('/', [
    '_controller' => 'App\Controller\HomeController::index',
]));

return $routes;
```

---

## Paramètres dans les routes

### **1. Routes dynamiques**

Les routes peuvent contenir des paramètres dynamiques.

#### Exemple dans `routes.yaml` :

```yaml
product_show:
    path: /product/{id}
    controller: App\Controller\ProductController::show
```

#### Exemple d’annotation :

```php
#[Route('/product/{id}', name: 'product_show')]
public function show(int $id) {
    return new Response("Produit ID : $id");
}
```

---

### **2. Contraintes sur les paramètres**

Vous pouvez définir des contraintes pour les paramètres dynamiques en utilisant des expressions régulières.

#### Exemple :

```yaml
product_show:
    path: /product/{id}
    controller: App\Controller\ProductController::show
    requirements:
        id: '\d+'
```

#### En annotations :

```php
#[Route('/product/{id}', name: 'product_show', requirements: ['id' => '\d+'])]
public function show(int $id) {
    return new Response("Produit ID : $id");
}
```

---

### **3. Valeurs par défaut**

Définissez des valeurs par défaut pour les paramètres.

#### Exemple :

```yaml
product_show:
    path: /product/{id}
    controller: App\Controller\ProductController::show
    defaults:
        id: 1
```

#### En annotations :

```php
#[Route('/product/{id}', name: 'product_show', defaults: ['id' => 1])]
```

---

## Routes de groupe (prefix)

Symfony permet de regrouper les routes partageant un même préfixe.

### **1. Dans `routes.yaml`**

```yaml
admin_:
    path: /admin
    controller: App\Controller\AdminController
    prefix:
        path: /dashboard
```

### **2. En annotations**

```php
#[Route('/admin', name: 'admin_')]
class AdminController extends AbstractController {
    #[Route('/dashboard', name: 'dashboard')]
    public function dashboard() {
        return new Response("Admin Dashboard");
    }
}
```

---

## Importer des fichiers de routes

Les routes peuvent être divisées en plusieurs fichiers et importées.

### Exemple dans `routes.yaml` :

```yaml
imports:
    - { resource: '../src/Controller/', type: annotation }
    - { resource: 'custom_routes.yaml' }
```

---

## Tester les routes

### Liste des routes disponibles :

```bash
php bin/console debug:router
```

### Vérifier une route spécifique :

```bash
php bin/console debug:router <nom_de_la_route>
```

---

## Bonnes pratiques

1. **Utilisez les annotations** pour des projets simples ou lorsque les routes sont directement liées aux contrôleurs.
2. **Centralisez les routes globales** dans `routes.yaml` pour les projets complexes.
3. **Utilisez des noms explicites pour vos routes** pour éviter les conflits.
4. **Testez vos routes régulièrement** en utilisant la commande `debug:router`.

---

## Liens connexes

- [[configuration-overview]] : Vue d'ensemble de la configuration dans Symfony.
- [[parameters-definition]] : Définir des paramètres globaux pour les routes.
- [[generateUrl()]] : Générer des URLs dynamiques avec des routes.
- [[redirectToRoute()]] : Rediriger vers une route existante.

---

## Ressources supplémentaires

- [Documentation officielle : Routing](https://symfony.com/doc/current/routing.html)
- [Annotations de routes](https://symfony.com/doc/current/routing.html#creating-routes-as-annotations)

---

## Tags

#symfony #routing #annotations #yaml #routes #parameters #best-practices