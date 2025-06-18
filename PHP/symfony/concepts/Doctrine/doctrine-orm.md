### Documentation Symfony : Doctrine ORM

#### Description

**Doctrine ORM** (Object-Relational Mapping) est une bibliothèque de gestion de bases de données relationnelles dans Symfony. Elle permet de mapper les entités PHP aux tables de base de données, de gérer la persistance des données, et de faciliter les interactions avec la base de données via des objets PHP.

---

### Installation et Configuration

1. **Installation**
    
    Vous pouvez installer Doctrine ORM via Composer avec la commande suivante :
    
    ```bash
    composer require symfony/orm-pack
    ```
    
2. **Configuration de la base de données**
    
    Dans votre fichier `.env` ou `.env.local`, configurez la connexion à la base de données :
    
    ```bash
    DATABASE_URL="mysql://username:password@127.0.0.1:3306/db_name"
    ```
    
    N'oubliez pas de créer la base de données si nécessaire avec la commande suivante :
    
    ```bash
    php bin/console doctrine:database:create
    ```
    

---

### Création d'une Entité

1. **Génération d'une Entité**
    
    Symfony facilite la création d'entités via la commande suivante :
    
    ```bash
    php bin/console make:entity
    ```
    
    Cela vous guidera à travers la création des champs de l'entité et leur type.
    
2. **Exemple d'entité** Voici un exemple simple d'une entité `Product` :
    
    ```php
    // src/Entity/Product.php
    namespace App\Entity;
    
    use Doctrine\ORM\Mapping as ORM;
    
    /**
     * @ORM\Entity(repositoryClass="App\Repository\ProductRepository")
     */
    class Product
    {
        /**
         * @ORM\Id()
         * @ORM\GeneratedValue()
         * @ORM\Column(type="integer")
         */
        private $id;
    
        /**
         * @ORM\Column(type="string", length=255)
         */
        private $name;
    
        /**
         * @ORM\Column(type="float")
         */
        private $price;
    
        // Getters and setters
    }
    ```
    
3. **Exécution de la migration**
    
    Une fois l'entité créée, vous devez générer et exécuter la migration pour mettre à jour la base de données :
    
    ```bash
    php bin/console make:migration
    php bin/console doctrine:migrations:migrate
    ```
    

---

### Interactions avec la Base de Données

1. **Récupération d'entités avec le Repository**
    
    Doctrine fournit un système de repository pour interagir avec la base de données. Vous pouvez récupérer une entité en utilisant les méthodes du repository.
    
    Exemple de récupération d'une entité :
    
    ```php
    $productRepository = $entityManager->getRepository(Product::class);
    $product = $productRepository->find($id); // Recherche par ID
    ```
    
2. **Requête avec QueryBuilder**
    
    Vous pouvez également utiliser le **QueryBuilder** pour construire des requêtes plus complexes.
    
    Exemple de requête avec QueryBuilder :
    
    ```php
    $queryBuilder = $entityManager->createQueryBuilder();
    $query = $queryBuilder
        ->select('p')
        ->from(Product::class, 'p')
        ->where('p.price > :price')
        ->setParameter('price', 50)
        ->getQuery();
    
    $products = $query->getResult();
    ```
    
3. **Insertion d'une Entité**
    
    Pour persister une entité dans la base de données, vous devez utiliser l'EntityManager :
    
    ```php
    // Création d'une nouvelle entité
    $product = new Product();
    $product->setName('Product 1');
    $product->setPrice(100.0);
    
    // Persistance et enregistrement
    $entityManager->persist($product);
    $entityManager->flush();
    ```
    

---

### Opérations de mise à jour et suppression

1. **Mise à jour d'une entité**
    
    Pour mettre à jour une entité, vous devez d'abord la récupérer, modifier ses propriétés, puis utiliser `flush()` pour appliquer les changements.
    
    Exemple de mise à jour :
    
    ```php
    $product = $entityManager->getRepository(Product::class)->find($id);
    if ($product) {
        $product->setPrice(120.0);
        $entityManager->flush(); // Sauvegarde la mise à jour
    }
    ```
    
2. **Suppression d'une entité**
    
    Pour supprimer une entité, vous devez la récupérer et appeler `remove()` sur l'EntityManager avant de faire un `flush()`.
    
    Exemple de suppression :
    
    ```php
    $product = $entityManager->getRepository(Product::class)->find($id);
    if ($product) {
        $entityManager->remove($product);
        $entityManager->flush(); // Applique la suppression
    }
    ```
    

---

### Transactions

Doctrine ORM prend en charge les transactions pour garantir la cohérence des données. Vous pouvez utiliser la méthode `beginTransaction()`, `commit()` et `rollback()`.

Exemple de transaction :

```php
$entityManager->beginTransaction();

try {
    // Effectuer des opérations sur la base de données
    $entityManager->flush();
    $entityManager->commit();
} catch (\Exception $e) {
    $entityManager->rollback();
    throw $e;
}
```

---

### Requêtes DQL (Doctrine Query Language)

Doctrine ORM offre un langage de requête orienté objet appelé **DQL**. Il est similaire à SQL mais fonctionne avec les entités et non directement avec les tables.

Exemple de requête DQL pour récupérer tous les produits :

```php
$query = $entityManager->createQuery('SELECT p FROM App\Entity\Product p');
$products = $query->getResult();
```

---

### Conseils pratiques

- **Utilisation des Repositories** : Créez un repository personnalisé pour chaque entité afin de centraliser les requêtes complexes.
- **Migrations** : Assurez-vous d'exécuter les migrations pour garder la base de données synchronisée avec les entités.
- **Utilisation des transactions** : Utilisez les transactions lorsque vous effectuez plusieurs opérations qui doivent être traitées atomiquement.
- **Optimisation** : Utilisez le **DQL** ou **QueryBuilder** pour des requêtes complexes, mais pour des requêtes simples, privilégiez les méthodes de base comme `find()`, `findAll()`, ou `findBy()`.

---

### Ressources complémentaires

- [Doctrine ORM Documentation](https://www.doctrine-project.org/projects/doctrine-orm.html)
- [Symfony - Doctrine ORM](https://symfony.com/doc/current/doctrine.html)