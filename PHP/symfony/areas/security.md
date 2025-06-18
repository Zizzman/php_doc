### Documentation Symfony : Security

#### Description

Le composant **Security** de Symfony fournit une infrastructure complète pour la gestion de la sécurité dans une application web, notamment l'authentification, l'autorisation et la gestion des rôles. Il permet de protéger les ressources sensibles et d'assurer que seuls les utilisateurs autorisés peuvent y accéder.

---

### Concepts de Base

1. **Authentification** : Le processus qui permet d'identifier un utilisateur. Cela peut être réalisé via des formulaires de connexion, des jetons API, des cookies, etc.
    
2. **Autorisation** : La vérification des permissions d'un utilisateur après son identification, afin de déterminer si l'utilisateur peut accéder à une ressource ou effectuer une action.
    
3. **Rôles** : Les rôles sont des attributs attribués aux utilisateurs pour déterminer leurs privilèges dans l'application. Par exemple, `ROLE_USER` ou `ROLE_ADMIN`.
    
4. **Firewalls** : Ils servent à protéger différentes parties de l'application en fonction de l'URL, du type de requête ou du mécanisme d'authentification (ex : formulaire de connexion, API REST, etc.).
    
5. **Access Control** : La gestion des permissions pour les utilisateurs selon leurs rôles et les ressources auxquelles ils accèdent.
    

---

### Configuration de la Sécurité

La configuration de la sécurité se fait dans le fichier `config/packages/security.yaml`. Ce fichier contient les règles relatives aux **firewalls**, **access control**, et les **providers**.

#### Exemple de Configuration de Base

```yaml
# config/packages/security.yaml
security:
    # Firewall pour l'authentification via formulaire
    firewalls:
        # Sécurisation du login
        login:
            pattern: ^/login$
            security: false

        # Sécurisation des pages protégées
        main:
            pattern: ^/(?!login).*$
            form_login:
                login_path: login
                check_path: login
            logout:
                path: /logout
            security: true

    # Contrôle d'accès
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
        - { path: ^/profile, roles: ROLE_USER }
```

---

### Authentification par Formulaire

L'authentification par formulaire est l'une des méthodes les plus courantes pour authentifier les utilisateurs. Elle permet à l'utilisateur de se connecter en envoyant son nom d'utilisateur et son mot de passe via un formulaire.

#### Exemple d'Authentification via Formulaire

```yaml
# config/packages/security.yaml
security:
    firewalls:
        main:
            form_login:
                login_path: login
                check_path: login
                default_target_path: /dashboard
```

1. **login_path** : Le chemin de la page de connexion.
2. **check_path** : Le chemin auquel les données du formulaire sont envoyées pour vérifier l'authentification.

#### Exemple de Contrôleur pour la Connexion

```php
// src/Controller/SecurityController.php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class SecurityController extends AbstractController
{
    /**
     * @Route("/login", name="login")
     */
    public function login()
    {
        // La vue du formulaire de connexion
        return $this->render('security/login.html.twig');
    }
}
```

---

### Access Control (Contrôle d'Accès)

Le contrôle d'accès permet de restreindre l'accès aux pages selon les rôles d'un utilisateur. Il se fait dans la section `access_control` de `security.yaml`.

#### Exemple de Contrôle d'Accès

```yaml
# config/packages/security.yaml
security:
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
        - { path: ^/profile, roles: ROLE_USER }
```

Dans cet exemple, seules les personnes ayant le rôle `ROLE_ADMIN` peuvent accéder aux pages sous `/admin`, et celles ayant le rôle `ROLE_USER` peuvent accéder à `/profile`.

---

### Rôles Utilisateurs

Les rôles sont utilisés pour définir les permissions des utilisateurs. Ils sont généralement associés à un utilisateur lors de l'authentification.

#### Exemple d'Assignation de Rôles

```php
// src/Entity/User.php
namespace App\Entity;

use Symfony\Component\Security\Core\User\UserInterface;

class User implements UserInterface
{
    private $roles = [];

    public function getRoles()
    {
        return $this->roles;
    }

    public function setRoles(array $roles)
    {
        $this->roles = $roles;
    }
}
```

Un utilisateur peut se voir attribuer plusieurs rôles, comme `ROLE_USER`, `ROLE_ADMIN`, ou d'autres rôles personnalisés.

---

### Gestion de la Session

Le composant **Security** gère également la session des utilisateurs après leur authentification. Le session handler est automatiquement configuré, mais vous pouvez personnaliser son comportement.

#### Exemple de Récupération de l'Utilisateur Actuel

```php
// Dans un contrôleur
$user = $this->getUser();  // Récupère l'utilisateur actuellement authentifié
```

---

### Méthodes Utiles dans le Composant Security

- **getUser()** : Récupère l'utilisateur actuellement authentifié.
- **[[isGranted()]]** : Vérifie si l'utilisateur a les droits d'accéder à une ressource.
- **[[denyAccessUnlessGranted()]]** : Déclenche une exception si l'utilisateur n'a pas les permissions nécessaires.

#### Exemple d'Utilisation de `isGranted()` et `denyAccessUnlessGranted()`

```php
// Dans un contrôleur
if ($this->isGranted('ROLE_ADMIN')) {
    // L'utilisateur est un administrateur
} else {
    // L'utilisateur n'est pas autorisé
    $this->denyAccessUnlessGranted('ROLE_ADMIN');
}
```

---

### Conseils et Bonnes Pratiques

- **Sécurisez toujours les pages sensibles** : Utilisez des **firewalls** pour protéger les pages et les ressources sensibles.
- **Utilisez des rôles clairs et précis** : Ne vous contentez pas de `ROLE_ADMIN` ou `ROLE_USER`, définissez des rôles plus fins pour des contrôles d'accès plus détaillés.
- **Vérifiez les permissions dans les contrôleurs** : Utilisez `isGranted()` ou `denyAccessUnlessGranted()` pour ajouter des vérifications de sécurité dans vos contrôleurs.
- **Mise en œuvre de l'authentification multi-facteurs (MFA)** : Symfony permet d'intégrer facilement des mécanismes de sécurité avancés comme l'authentification par code SMS, email, ou autre méthode supplémentaire.

---

### Ressources Complémentaires

- [Documentation Symfony : Security](https://symfony.com/doc/current/security.html)
- [Composant Security sur GitHub](https://github.com/symfony/security-core)