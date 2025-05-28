### Documentation : `getParameter()` en Symfony

#### Description

La méthode `getParameter()` est utilisée pour récupérer la valeur d'un paramètre de configuration dans Symfony. Ces paramètres peuvent être définis dans les fichiers de configuration, comme `services.yaml`, ou via des variables d'environnement.

---

### Syntaxe

```php
public function getParameter(string $name): mixed
```

---

### Paramètres

- **`$name`** _(string)_ :  
    Le nom du paramètre à récupérer. Cela doit correspondre à une clé définie dans les fichiers de configuration ou dans les variables d'environnement.

---

### Retour

- **Retourne la valeur du paramètre** associé au nom fourni. Le type retourné peut varier (string, array, bool, etc.), selon le type du paramètre configuré.

---

### Exemples

#### Exemple 1 : Récupérer un paramètre simple

```php
public function index(): Response
{
    $param = $this->getParameter('app.site_name');
    
    return new Response('Site Name: ' . $param);
}
```

Dans cet exemple, on suppose que le paramètre `app.site_name` a été défini dans le fichier `services.yaml` :

```yaml
parameters:
    app.site_name: 'Mon Site Web'
```

---

#### Exemple 2 : Récupérer un paramètre dans un service

```php
class SomeService
{
    private $siteName;
    
    public function __construct(private readonly ContainerInterface $container)
    {
        $this->siteName = $this->container->getParameter('app.site_name');
    }
    
    public function getSiteName(): string
    {
        return $this->siteName;
    }
}
```

---

#### Exemple 3 : Récupérer un paramètre dans un contrôleur

```php
public function show(): Response
{
    $timezone = $this->getParameter('app.timezone');
    
    return new Response('Timezone: ' . $timezone);
}
```

Dans ce cas, le paramètre `app.timezone` pourrait être défini dans le fichier de configuration comme suit :

```yaml
parameters:
    app.timezone: 'Europe/Paris'
```

---

#### Exemple 4 : Récupérer une valeur d'environnement

Si un paramètre est défini via une variable d'environnement dans `.env` ou dans le fichier `services.yaml`, vous pouvez le récupérer de la même manière :

```yaml
parameters:
    app.api_key: '%env(API_KEY)%'
```

```php
$apiKey = $this->getParameter('app.api_key');
```

Dans cet exemple, la valeur de `API_KEY` provient d'une variable d'environnement définie.

---

### Gestion des erreurs courantes

#### 1. **Erreur : Le paramètre n'existe pas**

**Cause :** Si vous tentez de récupérer un paramètre qui n'existe pas, Symfony générera une exception `ParameterNotFoundException`.  
**Solution :** Avant de récupérer un paramètre, assurez-vous qu'il est défini. Vous pouvez utiliser `getParameterBag()->has()` pour vérifier la présence du paramètre.

```php
if ($this->container->hasParameter('app.site_name')) {
    $siteName = $this->getParameter('app.site_name');
} else {
    throw new \Exception('Le paramètre "app.site_name" n\'existe pas.');
}
```

#### 2. **Erreur : Mauvais type ou format du paramètre**

**Cause :** Si vous vous attendez à un paramètre sous un certain format ou type (ex : tableau, chaîne de caractères), et que le paramètre est mal configuré, cela peut générer des erreurs dans votre application.  
**Solution :** Validez et gérez le type de paramètre avant de l'utiliser.

```php
$param = $this->getParameter('app.some_param');
if (!is_string($param)) {
    throw new \InvalidArgumentException('Le paramètre app.some_param doit être une chaîne.');
}
```

---

### Conseils

1. **Centraliser la configuration** :  
    Utilisez `getParameter()` pour récupérer des paramètres qui sont centralisés dans des fichiers de configuration (comme `services.yaml`). Cela vous permet de maintenir un code propre et réutilisable.
    
2. **Gérer les paramètres sensibles** :  
    Utilisez les variables d'environnement pour les paramètres sensibles (comme les clés API) et assurez-vous qu'ils sont bien définis dans votre fichier `.env`.
    
3. **Utiliser les paramètres de manière cohérente** :  
    Il est préférable de stocker les paramètres de configuration comme des constantes ou dans un fichier centralisé, plutôt que d'en avoir des versions éparpillées dans tout le code.
    
4. **Vérifications préalables** :  
    Utilisez des vérifications pour vous assurer qu'un paramètre est bien défini avant de l'utiliser, afin d'éviter des erreurs liées à des paramètres manquants.
    
5. **Optimisation des performances** :  
    Les paramètres sont généralement mis en cache. Il n'y a pas besoin de vous inquiéter des performances de `getParameter()`, car la récupération des paramètres depuis le conteneur de services est rapide, mais assurez-vous de ne pas appeler `getParameter()` dans des boucles fréquentes si ce n'est pas nécessaire.
    

---

### Ressources complémentaires

- [Symfony : Paramètres dans les fichiers de configuration](https://symfony.com/doc/current/services.html#parameters)
- [Symfony : Gestion des variables d'environnement](https://symfony.com/doc/current/components/dotenv.html)