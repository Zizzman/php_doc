### Documentation : `redirectToRoute()` en Symfony

#### Description

La méthode `redirectToRoute()` est utilisée dans un contrôleur Symfony pour rediriger l'utilisateur vers une route définie, en générant automatiquement l'URL basée sur les paramètres de la route.

---

### Syntaxe

```php
redirectToRoute(string $route, array $parameters = [], int $status = 302): RedirectResponse
```

---

### Paramètres

1. **`$route`** _(string)_ :  
    Nom de la route définie dans les fichiers de configuration des routes (`annotations`, `yaml`, ou `php`).  
    Exemple : `"home"`, `"user_profile"`, `"product_list"`.
    
2. **`$parameters`** _(array)_ :  
    Paramètres dynamiques nécessaires pour la génération de l'URL, correspondant aux variables définies dans la route.  
    Exemple : `['id' => 123]`.
    
3. **`$status`** _(int)_ :  
    Code HTTP de la redirection. Par défaut, une redirection temporaire (`302`).  
    Autres valeurs courantes :
    
    - `301` : Redirection permanente.
    - `303` : Redirection pour changer de méthode HTTP (ex. POST → GET).

---

### Exemples

#### Exemple simple : Redirection vers une route

```php
public function homeRedirect(): RedirectResponse
{
    return $this->redirectToRoute('home');
}
```

---

#### Passer des paramètres à la route

```php
public function viewUser(int $userId): RedirectResponse
{
    return $this->redirectToRoute('user_profile', ['id' => $userId]);
}
```

---

#### Utiliser un code de redirection personnalisé

```php
public function permanentRedirect(): RedirectResponse
{
    return $this->redirectToRoute('old_route', [], 301); // Redirection permanente
}
```

---

#### Redirection conditionnelle

```php
public function conditionalRedirect(bool $isAdmin): RedirectResponse
{
    if ($isAdmin) {
        return $this->redirectToRoute('admin_dashboard');
    }

    return $this->redirectToRoute('user_dashboard');
}
```

---

### Gestion des erreurs courantes

#### 1. **Route introuvable**

**Erreur :**

```
Unable to generate a URL for the named route "nonexistent_route" as such route does not exist.
```

**Solution :**

- Vérifiez que la route existe dans vos fichiers de configuration (`routes.yaml`, annotations, etc.).
- Exemple :
    
    ```yaml
    user_profile:
        path: /user/{id}
        controller: App\Controller\UserController::profile
    ```
    

---

#### 2. **Paramètres de route manquants**

**Erreur :**

```
Some mandatory parameters are missing ("id") to generate a URL for route "user_profile".
```

**Solution :**

- Passez les paramètres requis pour la route dans le tableau `$parameters`.
- Exemple :
    
    ```php
    return $this->redirectToRoute('user_profile', ['id' => 123]);
    ```
    

---

#### 3. **Code HTTP non valide**

**Erreur :**

```
InvalidArgumentException: The HTTP status code "600" is not valid.
```

**Solution :**

- Utilisez un code HTTP valide, comme `301`, `302`, ou `303`.

---

### Conseils

- **Nommez vos routes de manière explicite** : Utilisez des noms descriptifs (`user_profile`, `admin_dashboard`) pour faciliter leur utilisation.
- **Redirection permanente (`301`)** : Utilisez avec précaution, car les navigateurs mettent en cache cette redirection.
- **Vérifiez les paramètres de la route** : Utilisez `bin/console debug:router` pour lister les routes disponibles et leurs paramètres.
- **Tests et Debugging** : Vérifiez les redirections via le Web Profiler pour s'assurer qu'elles fonctionnent correctement.

---

### Ressources complémentaires

- [Documentation officielle : Symfony redirectToRoute()](https://symfony.com/doc/current/controller.html#redirecting)
- [Liste des codes HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)