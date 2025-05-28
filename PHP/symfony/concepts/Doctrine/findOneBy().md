### Documentation : `findOneBy()` en Symfony / Doctrine

#### Description

La méthode `findOneBy()` permet de récupérer une seule entité à partir de critères spécifiques. Contrairement à `find()`, elle ne se limite pas à la recherche par identifiant, mais utilise des paires clé-valeur représentant les champs de l’entité.

---

### Syntaxe

```php
findOneBy(array $criteria, array $orderBy = null): ?object
```

---

### Paramètres

1. **`$criteria`** _(array)_ :
    
    - Un tableau associatif où les clés sont les noms des champs et les valeurs sont les critères de recherche.
    - Exemple : `['email' => 'example@example.com']`.
2. **`$orderBy`** _(array|null)_ _(optionnel)_ :
    
    - Un tableau associatif pour trier les résultats avant de retourner le premier.
    - Exemple : `['createdAt' => 'DESC']`.

---

### Retour

- Retourne l'objet de l'entité correspondant aux critères si trouvé.
- Retourne `null` si aucune entité ne correspond aux critères.

---

### Exemples

#### Exemple 1 : Trouver une entité par un critère simple

```php
public function findUserByEmail(string $email): Response
{
    $repository = $this->getDoctrine()->getRepository(User::class);
    $user = $repository->findOneBy(['email' => $email]);

    if (!$user) {
        throw $this->createNotFoundException('No user found with email ' . $email);
    }

    return $this->render('user/show.html.twig', [
        'user' => $user,
    ]);
}
```

---

#### Exemple 2 : Trouver une entité avec plusieurs critères

```php
$criteria = [
    'status' => 'active',
    'role' => 'admin'
];

$repository = $this->getDoctrine()->getRepository(User::class);
$user = $repository->findOneBy($criteria);

if (!$user) {
    echo "No active admin found.";
}
```

---

#### Exemple 3 : Utiliser `orderBy` pour trier les résultats

```php
$criteria = ['status' => 'active'];
$orderBy = ['createdAt' => 'DESC'];

$repository = $this->getDoctrine()->getRepository(Post::class);
$post = $repository->findOneBy($criteria, $orderBy);

if ($post) {
    echo "Most recent active post: " . $post->getTitle();
}
```

---

#### Exemple 4 : Trouver une entité avec une valeur partielle (non supporté directement)

Pour une recherche partielle comme un champ contenant une sous-chaîne, utilisez le QueryBuilder :

```php
$repository = $this->getDoctrine()->getRepository(User::class);
$queryBuilder = $repository->createQueryBuilder('u')
    ->where('u.email LIKE :email')
    ->setParameter('email', '%example%')
    ->setMaxResults(1);

$user = $queryBuilder->getQuery()->getOneOrNullResult();
```

---

### Gestion des erreurs courantes

#### 1. **Erreur : Aucun résultat trouvé (`null`)**

**Cause :** Aucun enregistrement dans la base ne correspond aux critères.  
**Solution :** Toujours vérifier le retour et gérer le cas où la méthode retourne `null`.

```php
$user = $repository->findOneBy(['email' => 'nonexistent@example.com']);
if (!$user) {
    throw $this->createNotFoundException('No user found.');
}
```

---

#### 2. **Erreur : Critère incorrect**

**Cause :** Les noms de colonnes dans `$criteria` ne correspondent pas aux champs de l'entité.  
**Solution :** Vérifiez que les clés utilisées dans `$criteria` correspondent bien aux propriétés de l'entité Doctrine.

---

#### 3. **Erreur : Problème de typage**

**Cause :** Si vous passez une valeur mal typée dans les critères (ex. : une chaîne pour un champ attendu comme un entier).  
**Solution :** Assurez-vous que les types des valeurs correspondent à ceux définis dans l'entité.

---

### Conseils

1. **Préférez `findBy()` pour plusieurs résultats** : Si vous attendez plusieurs correspondances, utilisez `findBy()` au lieu de `findOneBy()`.  
    Exemple :
    
    ```php
    $users = $repository->findBy(['status' => 'active']);
    ```
    
2. **Limiter l'ordre** : Utilisez `orderBy` uniquement si le champ de tri est pertinent pour votre recherche.
    
3. **Valider les entrées utilisateur** : Si les critères proviennent de l'utilisateur (ex. : via un formulaire), nettoyez et validez les données avant de les utiliser dans `findOneBy()`.
    
4. **Utilisez QueryBuilder pour des requêtes complexes** : Pour des recherches avancées (ex. : jointures, sous-chaînes, calculs), utilisez le QueryBuilder.
    

---

### Ressources complémentaires

- [Doctrine : Working with Objects](https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/working-with-objects.html)
- [Symfony Doctrine : Repositories](https://symfony.com/doc/current/doctrine.html#fetching-objects-from-the-database)