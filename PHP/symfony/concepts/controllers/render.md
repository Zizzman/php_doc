### Documentation : `render()` en Symfony

#### Description

La méthode `render()` est utilisée dans un contrôleur Symfony pour retourner une réponse HTML en rendant un template Twig avec des variables passées en contexte.

---

### Syntaxe

```php
render(string $view, array $parameters = [], Response $response = null): Response
```

---

### Paramètres

1. **`$view`** _(string)_ :  
    Le chemin du fichier template Twig, relatif au répertoire `templates/`.  
    Exemple : `"base.html.twig"`, `"user/profile.html.twig"`
    
2. **`$parameters`** _(array)_ :  
    Les variables passées au template. Clés (string) → Noms des variables accessibles dans Twig.  
    Exemple : `['name' => 'John', 'age' => 30]`
    
3. **`$response`** _(Response|null)_ :  
    Une instance optionnelle de `Symfony\Component\HttpFoundation\Response` pour personnaliser la réponse HTTP.
    

---

### Exemples

#### Exemple simple : Rendre un template

```php
public function index(): Response
{
    return $this->render('home/index.html.twig');
}
```

---

#### Passer des variables au template

```php
public function userProfile(): Response
{
    $user = [
        'name' => 'John Doe',
        'email' => 'john.doe@example.com',
    ];
    return $this->render('user/profile.html.twig', [
        'user' => $user,
    ]);
}
```

---

#### Utiliser un `Response` personnalisé

```php
public function customResponse(): Response
{
    $response = new Response();
    $response->headers->set('X-Custom-Header', 'Value');
    return $this->render('custom/page.html.twig', [], $response);
}
```

---

### Gestion des erreurs courantes

#### 1. **Template introuvable**

**Erreur :**

```
Unable to find template "nonexistent.html.twig".
```

**Solution :**

- Vérifiez le chemin du fichier. Il doit être relatif à `templates/`.
- Assurez-vous que le fichier existe.

---

#### 2. **Variable non définie dans Twig**

**Erreur :**

```
Variable "user" does not exist.
```

**Solution :**

- Vérifiez que la variable est bien passée dans le tableau `$parameters`.
- Exemple :
    
    ```php
    return $this->render('user/profile.html.twig', ['user' => $user]);
    ```
    

---

#### 3. **Problèmes de syntaxe Twig**

**Erreur :**

```
Unexpected token "endblock" of value "endblock".
```

**Solution :**

- Vérifiez les balises ouvertes/fermées dans votre fichier Twig.
- Exemple correct :
    
    ```twig
    {% block content %}
        <p>Hello, {{ name }}</p>
    {% endblock %}
    ```
    

---

### Conseils

- **Organisation des templates** : Placez les templates dans des dossiers bien structurés (`templates/home`, `templates/user`, etc.).
- **Réutilisation des fragments** : Utilisez `{% include %}` ou `{% extends %}` pour factoriser le code Twig.
- **Debugging** : Activez le Web Profiler pour afficher les templates rendus et le contexte des variables.
- **Performance** : Activez le cache Twig en production.

---

### Ressources complémentaires

- [Documentation officielle : Symfony render()](https://symfony.com/doc/current/templates.html#rendering-templates)
- [Twig Documentation](https://twig.symfony.com/doc/3.x/)