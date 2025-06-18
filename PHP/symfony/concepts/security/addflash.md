### Documentation : `addFlash()` en Symfony

#### Description

La méthode `addFlash()` permet d'ajouter des messages flash (temporairement stockés dans la session) qui peuvent être affichés à l'utilisateur après une redirection ou une action. Ces messages sont souvent utilisés pour notifier des succès, des erreurs, ou des informations.

---

### Syntaxe

```php
addFlash(string $type, string $message): void
```

---

### Paramètres

1. **`$type`** _(string)_ :  
    La catégorie du message flash, souvent utilisée pour définir son style ou son importance.  
    Exemples : `"success"`, `"error"`, `"warning"`, `"info"`.
    
2. **`$message`** _(string)_ :  
    Le contenu du message à afficher à l'utilisateur.  
    Exemple : `"Votre compte a été créé avec succès."`.
    

---

### Exemples

#### Ajouter un message flash simple

```php
public function userCreated(): RedirectResponse
{
    $this->addFlash('success', 'Utilisateur créé avec succès.');
    return $this->redirectToRoute('user_list');
}
```

---

#### Ajouter plusieurs messages flash

```php
public function multipleFlashes(): RedirectResponse
{
    $this->addFlash('success', 'Premier message de succès.');
    $this->addFlash('info', 'Deuxième message d’information.');
    return $this->redirectToRoute('home');
}
```

---

#### Afficher les messages flash dans Twig

Dans le contrôleur :

```php
public function deleteUser(): RedirectResponse
{
    $this->addFlash('error', 'Utilisateur supprimé.');
    return $this->redirectToRoute('user_list');
}
```

Dans le template Twig :

```twig
{% for type, messages in app.flashes %}
    <div class="flash-{{ type }}">
        {% for message in messages %}
            <p>{{ message }}</p>
        {% endfor %}
    </div>
{% endfor %}
```

---

#### Ajouter un message flash conditionnel

```php
public function conditionalFlash(bool $isSuccess): RedirectResponse
{
    if ($isSuccess) {
        $this->addFlash('success', 'Opération réussie.');
    } else {
        $this->addFlash('error', 'Une erreur est survenue.');
    }
    return $this->redirectToRoute('dashboard');
}
```

---

### Gestion des erreurs courantes

#### 1. **Erreur : Aucun message n'apparaît**

**Cause possible :** Les messages flash ne sont pas affichés dans Twig.  
**Solution :** Assurez-vous d'utiliser une boucle pour afficher les messages flash dans vos templates :

```twig
{% for type, messages in app.flashes %}
    <div class="flash-{{ type }}">
        {% for message in messages %}
            <p>{{ message }}</p>
        {% endfor %}
    </div>
{% endfor %}
```

---

#### 2. **Erreur : Session non initialisée**

**Cause possible :** La session Symfony n'est pas activée.  
**Solution :** Vérifiez que la session est bien configurée dans `framework.yaml` :

```yaml
framework:
    session:
        enabled: true
```

---

### Conseils

- **Types standardisés** : Utilisez des types de messages cohérents (`success`, `error`, `info`, `warning`) pour faciliter leur gestion dans Twig.
- **Stylisation** : Associez chaque type de message à une classe CSS (`.flash-success`, `.flash-error`, etc.) pour une présentation visuelle claire.
- **Messages multilingues** : Intégrez les messages avec le composant de traduction pour gérer plusieurs langues.  
    Exemple :
    
    ```php
    $this->addFlash('success', $translator->trans('message.account_created'));
    ```
    

---

### Ressources complémentaires

- [Documentation officielle : Symfony addFlash()](https://symfony.com/doc/current/controller.html#flash-messages)
- [Composant Session Symfony](https://symfony.com/doc/current/components/http_foundation/sessions.html)