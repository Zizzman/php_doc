### Documentation : `denyAccessUnlessGranted()` en Symfony

#### Description

La méthode `denyAccessUnlessGranted()` est utilisée dans Symfony pour empêcher l'accès à une page ou à une ressource si l'utilisateur n'a pas l'autorisation nécessaire. Elle est généralement utilisée dans les contrôleurs pour appliquer des règles d'accès basées sur des rôles ou des permissions.

---

### Syntaxe

```php
public function denyAccessUnlessGranted(string $attribute, mixed $subject = null): void
```

---

### Paramètres

- **`$attribute`** _(string)_ :  
    Le nom de l'attribut ou de la permission à vérifier. Cela peut être un rôle (par exemple `ROLE_ADMIN`) ou une action spécifique (comme `EDIT`, `VIEW`, etc.).
    
- **`$subject`** _(mixed, optionnel)_ :  
    L'entité ou l'objet sur lequel appliquer la vérification d'accès. Ce paramètre est optionnel et permet de vérifier l'autorisation pour un objet spécifique. Par défaut, ce paramètre est `null` et la vérification se fait sans objet spécifique.
    

---

### Retour

Cette méthode ne retourne rien. Si l'utilisateur n'a pas l'autorisation nécessaire, une exception `AccessDeniedException` est levée, empêchant ainsi l'accès à la ressource.

---

### Exemples

#### Exemple 1 : Vérifier un rôle spécifique

```php
// Vérifier si l'utilisateur possède le rôle ROLE_ADMIN
public function someAction()
{
    $this->denyAccessUnlessGranted('ROLE_ADMIN');

    // Code de l'action si l'utilisateur a le rôle
}
```

#### Exemple 2 : Vérifier une permission sur une entité spécifique

```php
// Vérifier si l'utilisateur a la permission d'éditer une entité Article
public function editAction($id)
{
    $article = $this->getDoctrine()->getRepository(Article::class)->find($id);

    $this->denyAccessUnlessGranted('EDIT', $article);

    // Code pour éditer l'article
}
```

#### Exemple 3 : Vérifier un rôle dans une action de contrôleur

```php
// Vérifier si l'utilisateur est un utilisateur authentifié avec le rôle ROLE_USER
public function userAction()
{
    $this->denyAccessUnlessGranted('ROLE_USER');

    // Code à exécuter si l'utilisateur est authentifié avec ROLE_USER
}
```

---

### Cas d'erreur courants

#### 1. **Erreur : Utilisation d'un rôle ou d'un attribut inexistant**

**Cause :** Si vous essayez de vérifier un rôle ou un attribut qui n'existe pas dans votre configuration de sécurité, Symfony renverra une erreur.  
**Solution :** Assurez-vous que le rôle ou l'attribut que vous vérifiez est bien défini dans la configuration de sécurité.

```php
// Erreur si 'ROLE_UNKNOWN' n'existe pas dans la configuration de sécurité
$this->denyAccessUnlessGranted('ROLE_UNKNOWN');
```

#### 2. **Erreur : Le sujet n'est pas du bon type**

**Cause :** Si l'objet passé en paramètre `$subject` n'est pas celui attendu, la vérification d'accès pourrait échouer ou générer des erreurs inattendues.  
**Solution :** Vérifiez que l'objet passé est du type attendu par la règle de sécurité.

```php
$article = $this->getDoctrine()->getRepository(Article::class)->find($id);

// Erreur si l'attribut 'EDIT' n'est pas défini pour l'entité Article
$this->denyAccessUnlessGranted('EDIT', 'SomeNonExistentEntity');
```

---

### Conseils

1. **Utiliser avec précaution dans les contrôleurs** :  
    `denyAccessUnlessGranted()` est une méthode puissante pour appliquer des contrôles d'accès au niveau du contrôleur. Toutefois, elle ne doit être utilisée que pour des contrôles d'accès simples. Pour des contrôles plus complexes, vous devriez envisager des mécanismes de sécurité supplémentaires comme des règles personnalisées ou des voter.
    
2. **Gérer les exceptions de manière appropriée** :  
    Lorsque cette méthode échoue, elle lance une exception `AccessDeniedException`. Assurez-vous que vous gérez cette exception de manière appropriée (par exemple, en redirigeant l'utilisateur vers une page d'erreur ou de connexion).
    
3. **Utiliser des permissions sur des entités spécifiques** :  
    Si vous vérifiez une permission sur un objet, assurez-vous que la permission (`EDIT`, `VIEW`, etc.) est correctement configurée dans votre système de sécurité pour l'entité ou l'objet concerné.
    
4. **Vérifications dans les actions sensibles** :  
    Appliquez des vérifications d'accès dans les actions sensibles de vos contrôleurs où l'accès doit être strictement contrôlé. Par exemple, des actions telles que la modification ou la suppression d'une ressource doivent toujours être protégées par des contrôles d'accès appropriés.
    
5. **Tests de rôles et de permissions** :  
    Assurez-vous de tester vos rôles et permissions avant de déployer votre application en production. Vous pouvez également utiliser des outils comme PHPUnit ou Symfony Panther pour automatiser ces tests.
    

---

### Ressources complémentaires

- [Symfony Security](https://symfony.com/doc/current/security.html)
- [Symfony Access Control](https://symfony.com/doc/current/security/access_control.html)
- [Symfony Roles and Permissions](https://symfony.com/doc/current/security/roles.html)
- [Symfony Voter](https://symfony.com/doc/current/security/voters.html)