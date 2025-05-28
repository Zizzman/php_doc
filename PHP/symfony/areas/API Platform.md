### Documentation Symfony : API Platform

#### Description

**API Platform** est un framework basé sur Symfony pour développer des API REST et GraphQL. Il permet de créer rapidement des API conformes aux meilleures pratiques de l'industrie, avec une configuration minimale. API Platform inclut des outils pour la création de ressources, la gestion de la pagination, la validation des données, et l'authentification.

---

### Installation de API Platform

Pour installer API Platform dans un projet Symfony, vous pouvez utiliser Composer :

```bash
composer require api
```

Cela ajoute API Platform et ses dépendances à votre projet Symfony.

---

### Configuration de base

API Platform suit une approche convention-over-configuration. Les ressources sont définies par des entités Doctrine annotées ou des ressources personnalisées avec des contrôleurs.

#### Exemple de ressource simple

```php
// src/Entity/Book.php

namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ApiResource
 * @ORM\Entity()
 */
class Book
{
    /**
     * @ORM\Id()
     * @ORM\GeneratedValue()
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column(type="string")
     */
    private $title;

    // getters et setters...
}
```

- **@ApiResource** : Annotation qui permet à API Platform de considérer cette entité comme une ressource API.
- **@ORM\Entity** : Indique que `Book` est une entité Doctrine.

---

### Routes et Endpoints Automatiques

API Platform génère automatiquement des routes pour toutes les ressources annotées. Par exemple, pour l'entité `Book`, API Platform crée des routes pour :

- `GET /api/books` pour récupérer la liste des livres
- `POST /api/books` pour créer un livre
- `GET /api/books/{id}` pour récupérer un livre par son identifiant
- `PUT /api/books/{id}` pour modifier un livre
- `DELETE /api/books/{id}` pour supprimer un livre

---

### Personnalisation des Endpoints

Vous pouvez personnaliser les comportements de vos ressources API à l'aide de l'annotation `@ApiResource`.

#### Exemple de personnalisation d'un endpoint

```php
// src/Entity/Book.php

/**
 * @ApiResource(
 *     collectionOperations={"get"={"path"="/books"}},
 *     itemOperations={"get"={"path"="/books/{id}"}},
 *     attributes={"pagination_enabled"=false}
 * )
 */
class Book
{
    // ...
}
```

- **collectionOperations** : Définit les opérations disponibles pour la collection de ressources.
- **itemOperations** : Définit les opérations disponibles pour un élément spécifique.
- **pagination_enabled** : Active ou désactive la pagination.

---

### Filtrage et Recherche

API Platform prend en charge le filtrage, la recherche et la pagination des ressources de manière native. Vous pouvez ajouter des filtres en utilisant l'annotation `@ApiFilter`.

#### Exemple de filtrage

```php
// src/Entity/Book.php

use ApiPlatform\Core\Annotation\ApiFilter;
use ApiPlatform\Core\Bridge\Doctrine\Orm\Filter\SearchFilter;

/**
 * @ApiResource
 * @ApiFilter(SearchFilter::class, properties={"title": "partial"})
 * @ORM\Entity()
 */
class Book
{
    // ...
}
```

- **SearchFilter** : Permet de filtrer les ressources par un champ spécifique. Ici, la recherche est partielle sur le champ `title`.

---

### Validation des Données

API Platform intègre le composant de validation de Symfony pour valider les données envoyées dans les requêtes. Vous pouvez utiliser des annotations comme `@Assert\NotBlank`, `@Assert\Length`, etc.

#### Exemple de validation

```php
// src/Entity/Book.php

use Symfony\Component\Validator\Constraints as Assert;

/**
 * @ApiResource
 * @ORM\Entity()
 */
class Book
{
    /**
     * @ORM\Column(type="string")
     * @Assert\NotBlank
     * @Assert\Length(min=5)
     */
    private $title;

    // ...
}
```

- **@Assert\NotBlank** : Assure que le champ `title` n'est pas vide.
- **@Assert\Length(min=5)** : Vérifie que le titre a une longueur minimale de 5 caractères.

---

### Authentification et Sécurité

API Platform s'intègre avec le système de sécurité de Symfony pour gérer l'authentification et les autorisations. Vous pouvez utiliser des tokens JWT, OAuth2, ou des mécanismes d'authentification personnalisés.

#### Exemple d'authentification via JWT

1. Installer le bundle `lexik/jwt-authentication-bundle` :
    
    ```bash
    composer require lexik/jwt-authentication-bundle
    ```
    
2. Configurer les paramètres JWT dans `config/packages/lexik_jwt_authentication.yaml` :
    
    ```yaml
    lexik_jwt_authentication:
        secret_key: '%env(JWT_SECRET_KEY)%'
        public_key: '%env(JWT_PUBLIC_KEY)%'
        pass_phrase: '%env(JWT_PASSPHRASE)%'
        token_ttl: 3600
    ```
    

---

### Pagination

La pagination est activée par défaut pour les collections de ressources, mais elle peut être configurée ou désactivée selon vos besoins.

#### Exemple de désactivation de la pagination

```php
// src/Entity/Book.php

/**
 * @ApiResource(
 *     attributes={"pagination_enabled"=false}
 * )
 */
class Book
{
    // ...
}
```

---

### Support GraphQL

API Platform supporte GraphQL en plus de REST. Une fois configuré, vous pouvez effectuer des requêtes GraphQL pour accéder à vos ressources.

#### Exemple de requête GraphQL

```graphql
query {
  books {
    id
    title
  }
}
```

---

### Conseils et Bonnes Pratiques

1. **Utiliser des DTOs** : Pour une meilleure gestion des données, vous pouvez utiliser des **Data Transfer Objects (DTOs)** au lieu d'exposer directement vos entités.
2. **Gérer les relations** : API Platform gère les relations entre entités (par exemple, ManyToOne, OneToMany) et peut automatiquement inclure ou exclure ces relations dans les réponses.
3. **Sécuriser les API** : Toujours sécuriser les endpoints sensibles en utilisant les mécanismes d'authentification et d'autorisation de Symfony.
4. **Utiliser les filtres** : Exploitez les filtres comme `SearchFilter`, `DateFilter`, etc., pour ajouter des options de recherche et de filtrage sur vos API.

---

### Ressources Complémentaires

- [Documentation officielle d'API Platform](https://api-platform.com/docs/)
- [API Platform GitHub](https://github.com/api-platform/core)
- [GraphQL avec API Platform](https://api-platform.com/docs/core/graphql/)
- [Symfony Security et API Platform](https://api-platform.com/docs/core/security/)