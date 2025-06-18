### Documentation : `isGranted()` en Symfony

#### Description

La méthode `isGranted()` est utilisée dans Symfony pour vérifier si l'utilisateur actuel possède une certaine autorisation ou un rôle pour accéder à une ressource ou effectuer une action. Elle est généralement utilisée dans les contrôleurs, les services ou les templates pour appliquer des contrôles d'accès.

---

### Syntaxe

```php
public function isGranted(string $attribute, mixed $subject = null): bool
```

---

### Paramètres

- **`$attribute`** _(string)_ :  
    Le nom de l'attribut ou de la permission que vous souhaitez vérifier. Cela peut être un rôle (par exemple `ROLE_ADMIN`), ou une action spécifique comme `VIEW`, `EDIT`, etc.
    
- **`$subject`** _(mixed, optionnel)_ :  
    L'entité ou l'objet concerné par l'attribut. Ce paramètre est optionnel et permet de vérifier l'autorisation pour un objet spécifique (par exemple, une ressource que l'utilisateur souhaite modifier). Si vous ne spécifiez pas de sujet, la vérification se fait simplement sur l'attribut.
    

---

### Retour

La méthode retourne un **booléen** :

- `true` si l'utilisateur possède l'autorisation ou le rôle spécifié.
- `false` sinon.

---

### Exemples

#### Exemple 1 : Vérifier si l'utilisateur a un rôle spécifique

```php
// Vérifier si l'utilisateur a le rôle ROLE_ADMIN
if ($this->isGranted('ROLE_ADMIN')) {
    // L'utilisateur a le rôle ROLE_ADMIN, on peut lui accorder l'accès
} else {
    // L'utilisateur n'a pas le rôle, refuser l'accès
}
```

#### Exemple 2 : Vérifier une permission sur une entité spécifique

```php
// Vérifier si l'utilisateur peut éditer une entité Article
$article = $this->getDoctrine()->getRepository(Article::class)->find($id);

if ($this->isGranted('EDIT', $article)) {
    // L'utilisateur est autorisé à éditer cet article
} else {
    // L'utilisateur n'a pas la permission d'éditer cet article
}
```

#### Exemple 3 : Vérifier un rôle et un attribut dans une route ou un contrôleur

```php
// Vérifier si l'utilisateur est autorisé à accéder à la route
public function someAction()
{
    if (!$this->isGranted('VIEW', 'SomeEntity')) {
        throw $this->createAccessDeniedException('You do not have permission to view this page.');
    }
    
    // Code pour afficher la page
}
```

#### Exemple 4 : Vérification dans un template Twig

```twig
{% if is_granted('ROLE_ADMIN') %}
    <a href="{{ path('admin_dashboard') }}">Admin Dashboard</a>
{% else %}
    <p>You do not have access to the admin dashboard.</p>
{% endif %}
```

---

### Cas d'erreur courants

#### 1. **Erreur : Utilisation d'un attribut inexistant**

**Cause :** Si vous essayez de vérifier un rôle ou un attribut qui n'existe pas, Symfony renverra une erreur.  
**Solution :** Assurez-vous que l'attribut ou le rôle que vous utilisez est correctement défini et qu'il existe dans la configuration de votre système de sécurité.

```php
// Erreur si 'ROLE_UNKNOWN' n'existe pas dans la configuration de sécurité
if ($this->isGranted('ROLE_UNKNOWN')) {
    // Cela génère une erreur
}
```

#### 2. **Erreur : Le sujet n'est pas du bon type**

**Cause :** Si vous passez un objet incorrect dans le paramètre `$subject` (par exemple, une entité mal configurée), cela pourrait entraîner un comportement inattendu ou une erreur.  
**Solution :** Assurez-vous que le sujet passé à `isGranted()` correspond au type attendu par votre logique d'autorisation.

```php
$article = $this->getDoctrine()->getRepository(Article::class)->find($id);

// Erreur si l'attribut 'EDIT' n'est pas défini pour l'entité Article
if ($this->isGranted('EDIT', 'SomeNonExistentEntity')) {
    // Cela génère une erreur
}
```

---

### Conseils

1. **Utiliser des rôles standards** :  
    Lorsque vous gérez des rôles, il est conseillé d'utiliser des rôles standards comme `ROLE_USER`, `ROLE_ADMIN`, ou des rôles personnalisés que vous définissez dans la configuration de sécurité. Assurez-vous que ces rôles sont bien mappés dans votre système de sécurité.
    
2. **Vérification dans les contrôleurs et les templates** :  
    Utilisez `isGranted()` pour appliquer des contrôles d'accès dans les contrôleurs, mais également dans les templates Twig pour masquer des éléments d'interface (comme des liens, des boutons) en fonction des rôles de l'utilisateur.
    
3. **Créer des permissions personnalisées** :  
    Si vous avez besoin de vérifier des actions plus fines sur des entités spécifiques (comme `EDIT` ou `DELETE` sur une ressource particulière), vous pouvez définir des règles d'autorisation personnalisées dans la configuration de sécurité.
    
4. **Gérer les exceptions de sécurité** :  
    En cas d'accès non autorisé, vous pouvez lancer une exception comme `AccessDeniedException` pour gérer les erreurs d'accès et rediriger l'utilisateur vers une page d'erreur ou de connexion.
    
5. **Distinguer les rôles et les attributs** :  
    `isGranted()` permet de vérifier les rôles de l'utilisateur ou des actions spécifiques, mais il est important de bien comprendre la différence entre un rôle (comme `ROLE_USER`) et une permission sur une entité (comme `EDIT` sur une ressource).
    

---

### Ressources complémentaires

- [Symfony Security](https://symfony.com/doc/current/security.html)
- [Symfony Roles and Permissions](https://symfony.com/doc/current/security/roles.html)
- [Symfony Access Control](https://symfony.com/doc/current/security/access_control.html)