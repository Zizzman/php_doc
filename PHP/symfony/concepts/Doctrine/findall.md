### Documentation : `findAll()` en Symfony / Doctrine

#### Description

La méthode `findAll()` permet de récupérer toutes les entités d’un type donné depuis la base de données. Elle est utile pour afficher l’ensemble des enregistrements sans critères spécifiques.

---

### Syntaxe

```php
findAll(): array
```

---

### Paramètres

Aucun paramètre requis.

---

### Retour

- **Retourne un tableau d'objets** de l'entité correspondante.
- Si aucune entité n'est trouvée, retourne un tableau vide `[]`.

---

### Exemples

#### Exemple 1 : Récupérer toutes les entités

```php
public function listUsers(): Response
{
    $repository = $this->getDoctrine()->getRepository(User::class);
    $users = $repository->findAll();

    return $this->render('user/list.html.twig', [
        'users' => $users,
    ]);
}
```

---

#### Exemple 2 : Vérifier si la liste est vide

```php
$repository = $this->getDoctrine()->getRepository(Product::class);
$products = $repository->findAll();

if (empty($products)) {
    echo "No products found in the database.";
} else {
    foreach ($products as $product) {
        echo $product->getName() . PHP_EOL;
    }
}
```

---

#### Exemple 3 : Utilisation dans une API

```php
public function getAllPosts(): JsonResponse
{
    $repository = $this->getDoctrine()->getRepository(Post::class);
    $posts = $repository->findAll();

    return $this->json([
        'data' => $posts,
    ]);
}
```

---

### Gestion des erreurs courantes

#### 1. **Erreur : Aucun résultat (tableau vide)**

**Cause :** La table associée à l'entité est vide.  
**Solution :** Gérez ce cas en vérifiant si le tableau retourné est vide.

```php
$entities = $repository->findAll();
if (empty($entities)) {
    echo "No entities found.";
}
```

#### 2. **Erreur : Repository non trouvé**

**Cause :** Le repository n’est pas configuré ou l'entité est mal référencée.  
**Solution :** Vérifiez la déclaration de l’entité et du repository dans votre configuration Doctrine.

---

### Conseils

1. **Limiter l’utilisation dans les bases volumineuses** :  
    Si la table contient beaucoup de lignes, utiliser `findAll()` peut entraîner des problèmes de performance et de mémoire. Privilégiez des requêtes avec des critères (`findBy()`) ou une pagination.
    
2. **Filtrage et tri** :  
    Si vous devez appliquer un tri ou un filtre, utilisez `findBy()` ou le QueryBuilder à la place.  
    Exemple avec tri :
    
    ```php
    $users = $repository->findBy([], ['createdAt' => 'DESC']);
    ```
    
3. **Pagination recommandée** :  
    Si vous devez traiter un grand nombre de résultats, utilisez un package comme [KnpPaginatorBundle](https://github.com/KnpLabs/KnpPaginatorBundle) pour implémenter une pagination.
    

---

### Ressources complémentaires

- [Symfony Doctrine : Fetching Objects](https://symfony.com/doc/current/doctrine.html#fetching-objects-from-the-database)
- [Doctrine : Basic Mapping](https://www.doctrine-project.org/projects/doctrine-orm/en/current/tutorials/getting-started.html#basic-mapping)