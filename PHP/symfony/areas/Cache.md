### Documentation Symfony : Le Cache

#### Description

Le cache dans Symfony est utilisé pour stocker temporairement des données qui ne changent pas fréquemment, permettant ainsi de réduire les coûts de calcul et de rendre l'application plus rapide. Symfony offre un système de cache flexible et puissant, qui peut être utilisé pour le cache des données, des vues, des requêtes, et plus encore.

---

### Types de Cache en Symfony

1. **Cache de données** : Utilisé pour stocker des données spécifiques comme des résultats de requêtes, des objets, etc.
2. **Cache HTTP** : Utilisé pour mettre en cache les réponses HTTP, comme les pages HTML.
3. **Cache des métadonnées** : Utilisé pour stocker des informations supplémentaires comme les entités Doctrine ou la configuration de l'application.T

---

### Installation du Cache

Symfony utilise le composant `Cache` qui peut être installé via Composer :

```bash
composer require symfony/cache
```

---

### Utilisation du Cache

#### Service Cache

Le service `CacheInterface` est le point d'accès pour manipuler les différents caches dans Symfony.

```php
use Symfony\Contracts\Cache\CacheInterface;

class MyService
{
    private $cache;

    public function __construct(CacheInterface $cache)
    {
        $this->cache = $cache;
    }

    public function getData()
    {
        // Exemple d'utilisation du cache
        $data = $this->cache->get('key', function (ItemInterface $item) {
            // Si la donnée n'est pas en cache, calculer et stocker la donnée
            $item->expiresAfter(3600); // Expiration après 1 heure
            return 'Données calculées ou récupérées';
        });

        return $data;
    }
}
```

Dans cet exemple, la méthode `get()` vérifie d'abord si la donnée est présente dans le cache. Si ce n’est pas le cas, elle exécute la fonction de rappel pour calculer ou récupérer la donnée avant de la stocker dans le cache.

#### Cache des Réponses HTTP

Symfony permet de mettre en cache les réponses HTTP à l'aide des annotations ou des contrôleurs. Par exemple :

```php
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class PageController
{
    /**
     * @Route("/cache-test", name="cache_test")
     */
    public function index()
    {
        $response = new Response('Page en cache');
        $response->setCache([
            'public' => true,
            'max_age' => 3600,
            's_maxage' => 3600,
            'must_revalidate' => true,
        ]);

        return $response;
    }
}
```

Dans cet exemple, le cache HTTP est configuré avec une durée de vie de 1 heure pour les utilisateurs et les proxys.

---

### Cache des Données avec `CachePool`

Symfony offre des pools de cache comme `FilesystemCache`, `ApcuCache`, `RedisCache`, etc. Ces pools peuvent être utilisés pour stocker et récupérer des données dans différentes solutions de stockage.

Exemple avec un cache basé sur le système de fichiers :

```php
use Symfony\Component\Cache\Adapter\FilesystemAdapter;

$cache = new FilesystemAdapter();

// Mise en cache d'une donnée
$cache->get('key', function (ItemInterface $item) {
    $item->expiresAfter(3600); // Expiration après 1 heure
    return 'Données mises en cache sur le système de fichiers';
});
```

---

### Configuration du Cache

La configuration du cache se fait principalement dans le fichier `config/packages/cache.yaml`. Voici un exemple de configuration pour utiliser Redis comme cache de données :

```yaml
framework:
    cache:
        pools:
            app.cache.redis:
                adapter: 'cache.adapter.redis'
                default_lifetime: 3600
                provider: '%env(REDIS_URL)%'
```

Dans cet exemple, Redis est configuré comme un adaptateur pour le cache des données. Vous pouvez également utiliser `cache.adapter.filesystem` ou d'autres solutions comme `cache.adapter.memcached`.

---

### Gestion du Cache

#### Vider le Cache

Pour vider le cache d'une application Symfony, utilisez la commande suivante :

```bash
php bin/console cache:clear
```

Cette commande supprime tout le cache de l'application (y compris le cache des routes, du framework, etc.) et le régénère selon la configuration actuelle.

#### Expiration du Cache

Les caches peuvent expirer après un certain délai en utilisant la méthode `expiresAfter()`.

```php
$item->expiresAfter(3600); // Expiration après 1 heure
```

Cela garantit que les données seront stockées pendant une période déterminée avant que le cache ne soit automatiquement invalidé.

---

### Conseils et Bonnes Pratiques

1. **Utiliser les pools de cache pour une gestion fine** : Utilisez des pools de cache pour différencier les types de données ou d'utilisateurs (par exemple, cache pour les résultats de base de données, cache pour les pages HTML).
2. **Cache des requêtes Doctrine** : Symfony offre des mécanismes pour mettre en cache les résultats des requêtes Doctrine en utilisant `DoctrineCache` ou en configurant directement des entités Doctrine avec le cache.
3. **Sécuriser le cache** : Lorsque vous travaillez avec des caches sensibles, assurez-vous qu'ils ne contiennent pas d'informations privées. Utilisez des mécanismes comme `cache.adapter.filesystem` et cryptez les données sensibles.
4. **Réglage du cache pour les environnements de production** : Assurez-vous que le cache est bien configuré pour les environnements de production en activant le cache optimisé dans `config/packages/prod/cache.yaml`.
5. **Surveiller les performances** : Surveillez la taille du cache et les performances de vos adaptateurs (par exemple, Redis ou Memcached) pour éviter la surcharge et garantir une efficacité maximale.

---

### Commandes Utiles

- `php bin/console cache:clear` : Vide le cache de l'application.
- `php bin/console cache:warmup` : Préchauffe le cache en préchargeant des données ou des ressources.
- `php bin/console debug:container cache` : Affiche les services liés au cache.

---

### Ressources Complémentaires

- [Symfony Cache Component Documentation](https://symfony.com/doc/current/components/cache.html)
- [Doctrine Cache Documentation](https://www.doctrine-project.org/projects/doctrine-cache.html)
- [Redis Cache Adapter Documentation](https://symfony.com/doc/current/cache/adapters/redis.html)