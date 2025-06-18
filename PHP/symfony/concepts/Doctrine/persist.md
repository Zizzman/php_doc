### Documentation : `persist()` en Symfony / Doctrine

#### Description

La méthode `persist()` de Doctrine est utilisée pour marquer une entité comme devant être ajoutée ou mise à jour dans la base de données lors de l'exécution de l'opération `flush()`. Elle ne modifie pas immédiatement la base de données mais prépare l'entité pour l'ajout ou la mise à jour à la prochaine synchronisation avec la base de données.

---

### Syntaxe

```php
public function persist(object $entity): void
```

---

### Paramètres

- **`$entity`** _(object)_ :  
    L'entité à persister, qui doit être une instance d'une entité Doctrine (ex. : une classe annotée avec `@Entity`).

---

### Retour

- Cette méthode ne retourne rien (`void`).

---

### Exemples

#### Exemple 1 : Persister une nouvelle entité

```php
public function createUser(Request $request): Response
{
    $entityManager = $this->getDoctrine()->getManager();
    
    $user = new User();
    $user->setName('John Doe');
    $user->setEmail('johndoe@example.com');
    
    // Persiste l'entité
    $entityManager->persist($user);

    // Sauvegarde les changements dans la base de données
    $entityManager->flush();

    return $this->redirectToRoute('user_show', ['id' => $user->getId()]);
}
```

---

#### Exemple 2 : Mettre à jour une entité existante

```php
public function updateUser(User $user): Response
{
    $entityManager = $this->getDoctrine()->getManager();
    
    // Mise à jour des propriétés de l'entité
    $user->setEmail('newemail@example.com');
    
    // Persiste l'entité (mise à jour)
    $entityManager->persist($user);
    
    // Sauvegarde les changements dans la base de données
    $entityManager->flush();

    return $this->redirectToRoute('user_show', ['id' => $user->getId()]);
}
```

---

#### Exemple 3 : Persister plusieurs entités

```php
public function createMultipleUsers(Request $request): Response
{
    $entityManager = $this->getDoctrine()->getManager();

    // Créer plusieurs entités
    $user1 = new User();
    $user1->setName('Alice');
    $user1->setEmail('alice@example.com');

    $user2 = new User();
    $user2->setName('Bob');
    $user2->setEmail('bob@example.com');

    // Persiste les entités
    $entityManager->persist($user1);
    $entityManager->persist($user2);
    
    // Sauvegarde toutes les entités en une seule fois
    $entityManager->flush();

    return $this->redirectToRoute('user_list');
}
```

---

### Gestion des erreurs courantes

#### 1. **Erreur : L'entité ne fait pas partie du gestionnaire**

**Cause :** L'entité n'a pas été correctement initialisée ou n'est pas correctement associée au gestionnaire d'entités.  
**Solution :** Assurez-vous que l'entité a été correctement instanciée et que vous utilisez l'instance correcte du gestionnaire d'entités.

```php
$user = new User();
$entityManager->persist($user);
$entityManager->flush(); // Toujours appeler flush après persist pour enregistrer les données
```

#### 2. **Erreur : Tentative de persistance sur une entité non valide**

**Cause :** Les entités Doctrine doivent être valides avant d'être persistées (via des validations définies par exemple avec des annotations).  
**Solution :** Utilisez les contraintes de validation avant de persister l'entité pour éviter des erreurs en base de données.

```php
use Symfony\Component\Validator\Validator\ValidatorInterface;

public function createUser(Request $request, ValidatorInterface $validator): Response
{
    $user = new User();
    $user->setEmail('invalid-email'); // Email invalide
    
    $errors = $validator->validate($user);
    if (count($errors) > 0) {
        // Gestion des erreurs de validation
        return new Response((string) $errors);
    }

    $entityManager = $this->getDoctrine()->getManager();
    $entityManager->persist($user);
    $entityManager->flush();

    return $this->redirectToRoute('user_show', ['id' => $user->getId()]);
}
```

---

### Conseils

1. **Ne pas appeler `flush()` trop souvent** :  
    Appeler `persist()` marque simplement l'entité pour être persistée. C'est `flush()` qui effectue la mise à jour réelle dans la base de données. Il est plus performant de grouper plusieurs appels `persist()` et de les synchroniser en une seule fois via `flush()`.
    
2. **Utilisation d'objets partiellement modifiés** :  
    Si une entité est déjà persistée et modifiée, vous n'avez pas besoin d'appeler `persist()` à nouveau. Doctrine détectera les changements automatiquement lors de l'appel de `flush()`.
    
3. **Utiliser les transactions pour des opérations critiques** :  
    Si vous effectuez plusieurs `persist()` sur des entités liées, il est conseillé d'encapsuler l'opération dans une transaction pour garantir la cohérence des données. Exemple :
    
    ```php
    $entityManager->beginTransaction();
    try {
        $entityManager->persist($user1);
        $entityManager->persist($user2);
        $entityManager->flush();
        $entityManager->commit();
    } catch (\Exception $e) {
        $entityManager->rollback();
        throw $e;
    }
    ```
    
4. **Optimiser les appels `flush()`** :  
    Ne persistez pas trop souvent dans une boucle, car chaque appel à `flush()` peut être coûteux en termes de performance.
    

---

### Ressources complémentaires

- [Doctrine : Persist and Flush](https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/unitofwork.html#persist)
- [Symfony : Working with Doctrine](https://symfony.com/doc/current/doctrine.html#persisting-and-flushing)