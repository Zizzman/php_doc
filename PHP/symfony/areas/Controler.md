### Documentation Symfony : Controller

#### Description

Les **contrôleurs** dans Symfony sont responsables de traiter les requêtes HTTP entrantes, d'exécuter la logique métier nécessaire, puis de renvoyer une réponse. Les contrôleurs sont des classes PHP qui contiennent des méthodes exécutées lorsque les utilisateurs accèdent à des URL spécifiques via des routes définies.

---

### Définition d'un contrôleur

Un contrôleur Symfony est une classe qui contient des méthodes appelées actions. Chaque action est associée à une route spécifique. Les actions peuvent renvoyer des réponses simples ou des objets plus complexes, comme des vues ou des redirections.

1. **Structure basique d'un contrôleur**

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/", name="homepage")
     */
    public function index(): Response
    {
        return new Response('Bienvenue sur la page d\'accueil!');
    }
}
```

---

### Méthodes des contrôleurs

Les méthodes des contrôleurs sont généralement annotées avec des annotations de routage pour associer l'action à une route spécifique. Elles peuvent aussi recevoir des paramètres provenant de la route.

2. **Exemple de méthode avec paramètre dans la route**

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
    public function show($id): Response
    {
        // Logique pour récupérer le produit par ID
        return new Response("Produit ID: $id");
    }
}
```

---

### Retourner une réponse dans un contrôleur

1. **Retourner un objet `Response`**

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/about", name="about")
     */
    public function about(): Response
    {
        return new Response('Page à propos');
    }
}
```

2. **Retourner une vue avec `render()`**

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/home", name="home")
     */
    public function home(): Response
    {
        return $this->render('home/index.html.twig');
    }
}
```

3. **Retourner une redirection avec `redirectToRoute()`**

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\RedirectResponse;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/redirect", name="redirect")
     */
    public function redirectToHome(): RedirectResponse
    {
        return $this->redirectToRoute('homepage');
    }
}
```

---

### Paramètres de méthode

Les paramètres de méthode peuvent être passés automatiquement depuis la route. Si un paramètre de route est défini, il sera injecté directement dans la méthode.

4. **Exemple de paramètre de route**

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/user/{username}", name="user_profile")
     */
    public function profile($username): Response
    {
        return new Response("Profil de l'utilisateur : $username");
    }
}
```

---

### Utilisation de services dans les contrôleurs

Les services Symfony, comme le gestionnaire de session ou le service Doctrine, peuvent être injectés dans les contrôleurs pour effectuer des tâches complexes.

5. **Exemple d'injection de service (Doctrine)**

```php
// src/Controller/ProductController.php
namespace App\Controller;

use App\Repository\ProductRepository;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class ProductController
{
    /**
     * @Route("/products", name="product_list")
     */
    public function list(ProductRepository $productRepository): Response
    {
        $products = $productRepository->findAll();
        return $this->render('product/list.html.twig', [
            'products' => $products
        ]);
    }
}
```

---

### Gestion des erreurs

Symfony permet de gérer les erreurs directement depuis les contrôleurs ou via des gestionnaires d'événements personnalisés. Vous pouvez également rediriger ou afficher des messages d'erreur lorsque nécessaire.

6. **Exemple de gestion d'erreur avec `try-catch`**

```php
// src/Controller/ProductController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\HttpKernel\Exception\NotFoundHttpException;

class ProductController
{
    /**
     * @Route("/product/{id}", name="product_show")
     */
    public function show($id): Response
    {
        try {
            // Logique pour récupérer le produit
            throw new NotFoundHttpException('Produit non trouvé');
        } catch (NotFoundHttpException $e) {
            return new Response('Erreur: ' . $e->getMessage());
        }
    }
}
```

---

### Conseils pratiques

- **Nommer les méthodes des contrôleurs de manière descriptive** : Choisissez des noms qui expliquent clairement ce que fait chaque action.
- **Utiliser les annotations de routage pour simplifier les routes** : Elles permettent de lier directement une URL à une méthode du contrôleur.
- **Exploiter les services Symfony** : Utilisez les services (comme le `Doctrine`, la gestion des sessions, ou des services personnalisés) pour ajouter des fonctionnalités à vos actions.
- **Sécuriser les actions** : Utilisez `@IsGranted()` ou `denyAccessUnlessGranted()` pour contrôler l'accès à certaines actions.

---

### Ressources complémentaires

- [Symfony Controllers Documentation](https://symfony.com/doc/current/controller.html)
- [Routing and Controllers](https://symfony.com/doc/current/routing.html)
- [Symfony HTTP Foundation](https://symfony.com/doc/current/components/http_foundation.html)