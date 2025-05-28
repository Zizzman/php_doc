### Documentation Symfony : Le Routing

#### Description

Le **routing** dans Symfony est le mécanisme qui permet d'associer une URL à une action spécifique dans le contrôleur de l'application. Symfony utilise un système flexible de routes pour définir les chemins d'accès aux différentes pages de l'application et les associer à des méthodes de contrôleur.

---

### Fichiers de configuration de routage

1. **Fichier `config/routes.yaml` (par défaut)**  
    C'est le fichier principal pour définir les routes de l'application. Les routes sont définies avec des chemins d'URL, et peuvent être associées à des méthodes de contrôleur.

```yaml
# config/routes.yaml
homepage:
  path: /
  controller: App\Controller\HomeController::index
```

2. **Annotation de routage dans les contrôleurs**  
    Symfony permet de définir des routes directement dans les contrôleurs avec des annotations. Pour utiliser cette fonctionnalité, il faut activer le package `symfony/annotations`.

```php
// src/Controller/HomeController.php
namespace App\Controller;

use Symfony\Component\Routing\Annotation\Route;

class HomeController
{
    /**
     * @Route("/", name="homepage")
     */
    public function index()
    {
        return new Response('Homepage');
    }
}
```

3. **Fichier `config/routes/` (routes spécifiques par dossier)**  
    Ce répertoire permet de définir des routes spécifiques dans différents fichiers YAML, pour organiser les routes par fonctionnalité.

```yaml
# config/routes/product.yaml
product_show:
  path: /product/{id}
  controller: App\Controller\ProductController::show
  requirements:
    id: \d+
```

---

### Paramètres de routes

1. **Paramètres dynamiques**  
    Vous pouvez définir des paramètres dynamiques dans le chemin de la route en utilisant des accolades `{}`. Ces paramètres seront passés à la méthode du contrôleur.

```yaml
# config/routes.yaml
product_show:
  path: /product/{id}
  controller: App\Controller\ProductController::show
```

2. **Exemple de contrôleur utilisant un paramètre**

```php
// src/Controller/ProductController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class ProductController
{
    /**
     * @Route("/product/{id}", name="product_show")
     */
    public function show($id)
    {
        // Logique pour afficher le produit avec l'ID donné
        return new Response('Product ID: ' . $id);
    }
}
```

3. **Exigences de paramètres**  
    Vous pouvez définir des exigences de format pour les paramètres dynamiques (par exemple, un identifiant qui doit être un entier).

```yaml
# config/routes.yaml
product_show:
  path: /product/{id}
  controller: App\Controller\ProductController::show
  requirements:
    id: \d+  # Le paramètre doit être un nombre entier
```

---

### Générer des URLs avec `generateUrl()`

Vous pouvez générer des URLs dynamiquement dans votre application à l'aide de la méthode [[generateUrl()]], généralement utilisée dans les contrôleurs ou les templates.

```php
// Générer l'URL pour la route "product_show"
$url = $this->generateUrl('product_show', ['id' => 42]);
```

---

### Cas d'erreurs courants

1. **Route non définie**  
    Si une route n'est pas définie, Symfony lèvera une erreur de type "No route found for...".

**Solution :** Vérifiez si la route est correctement définie dans le fichier de configuration des routes et que son nom est correct.

2. **Paramètre manquant**  
    Lorsque vous générez une URL avec `generateUrl()` et qu'un paramètre obligatoire est manquant, Symfony renverra une erreur.

**Solution :** Assurez-vous de fournir tous les paramètres requis pour la route.

---

### Conseils pratiques

1. **Nom des routes**  
    Assurez-vous de nommer vos routes de manière significative, cela facilitera la génération d'URLs et l'utilisation des redirections.
    
2. **Sécurisation des routes**  
    Vous pouvez sécuriser les routes en utilisant des restrictions ou des paramètres `requirements`. Par exemple, vous pouvez limiter les accès en fonction du rôle de l'utilisateur.
    

```yaml
# Exemple de route sécurisée
admin_dashboard:
  path: /admin/dashboard
  controller: App\Controller\AdminController::dashboard
  requirements:
    _role: ROLE_ADMIN
```

3. **Utilisation des redirections**  
    Utilisez les redirections de manière judicieuse dans vos contrôleurs pour améliorer l'expérience utilisateur.

[[redirectToRoute()]]

```php
// Exemple de redirection
return $this->redirectToRoute('homepage');
```

4. **Utiliser des sous-routes pour organiser les URL**  
    Les sous-routes permettent de mieux organiser les URL complexes.

```yaml
# config/routes.yaml
product:
  path: /product
  controller: App\Controller\ProductController::index
  prefix: /product/{category}
```

---

### Ressources complémentaires

- [Symfony Routing Documentation](https://symfony.com/doc/current/routing.html)
- [Annotations de Routage](https://symfony.com/doc/current/routing/annotations.html)
- [Génération d'URLs en Symfony](https://symfony.com/doc/current/routing.html#generate-urls)