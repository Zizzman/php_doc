
### Documentation Symfony : Templates (Twig)

#### Description

Twig est le moteur de templates par défaut de Symfony. Il permet de séparer la logique de l'affichage et de la gestion des données, facilitant ainsi le développement d'applications web bien structurées. Twig est un moteur de templates puissant, flexible et sécurisé.

---

### Intégration de Twig dans Symfony

Symfony utilise Twig pour rendre les vues. Un template Twig est généralement un fichier avec l'extension `.html.twig`, situé dans le répertoire `templates/` de votre projet Symfony.

---

### Structure de base d'un template

Un template Twig contient du code HTML et des balises Twig pour l'insertion de données dynamiques.

1. **Exemple d'un template Twig simple**

```twig
{# templates/default/index.html.twig #}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{{ title }}</title>
</head>
<body>
    <h1>{{ message }}</h1>
</body>
</html>
```

Dans l'exemple ci-dessus, `{{ title }}` et `{{ message }}` sont des variables Twig qui seront remplies par des données provenant du contrôleur.

---

### Passer des données à un template

Dans Symfony, les données sont transmises aux templates via le contrôleur. Vous utilisez la méthode `render()` pour envoyer un template à l'utilisateur avec les données.

2. **Exemple de passage de données au template**

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/", name="home")
     */
    public function index(): Response
    {
        $title = "Page d'accueil";
        $message = "Bienvenue sur notre site Symfony!";
        
        return $this->render('default/index.html.twig', [
            'title' => $title,
            'message' => $message,
        ]);
    }
}
```

Ici, les variables `$title` et `$message` sont envoyées au template Twig.

---

### Structures conditionnelles et boucles

Twig prend en charge les structures conditionnelles et les boucles pour rendre les templates plus dynamiques.

3. **Conditionnelle dans Twig**

```twig
{# templates/default/index.html.twig #}
{% if user is defined %}
    <p>Bonjour, {{ user.name }}!</p>
{% else %}
    <p>Bienvenue, invité!</p>
{% endif %}
```

4. **Boucle dans Twig**

```twig
{# templates/default/products.html.twig #}
<ul>
    {% for product in products %}
        <li>{{ product.name }} - {{ product.price }}€</li>
    {% else %}
        <li>Aucun produit disponible.</li>
    {% endfor %}
</ul>
```

---

### Héritage de templates

L'héritage de templates permet de réutiliser des parties communes dans plusieurs pages, ce qui simplifie la gestion des mises à jour de l'interface utilisateur.

5. **Exemple d'héritage de template**

```twig
{# templates/base.html.twig #}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Mon Application Symfony{% endblock %}</title>
</head>
<body>
    <header>
        <h1>{% block header %}Bienvenue sur mon site{% endblock %}</h1>
    </header>

    <div>
        {% block content %}{% endblock %}
    </div>
</body>
</html>
```

Dans ce fichier de base, les blocs `title`, `header` et `content` peuvent être redéfinis dans des templates enfants.

```twig
{# templates/default/index.html.twig #}
{% extends 'base.html.twig' %}

{% block title %}Accueil - Mon Application{% endblock %}

{% block content %}
    <h2>Page d'accueil</h2>
    <p>Bienvenue sur la page d'accueil.</p>
{% endblock %}
```

---

### Filtres Twig

Twig offre de nombreux filtres pour manipuler les variables avant de les afficher.

6. **Exemples de filtres**

- **`|lower`** : Convertir une chaîne en minuscules

```twig
{{ 'HELLO WORLD'|lower }}  {# Affiche: hello world #}
```

- **`|date`** : Formater une date

```twig
{{ "now"|date("Y-m-d H:i:s") }}  {# Affiche: 2024-12-16 10:45:00 #}
```

- **`|escape`** : Échapper une chaîne pour éviter les attaques XSS

```twig
{{ user_input|escape }}
```

---

### Inclusions et macros

Twig permet d'inclure des fragments de templates ou d'utiliser des macros pour encapsuler du code réutilisable.

7. **Inclusion d'un template**

```twig
{# templates/default/header.html.twig #}
<header>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
    </nav>
</header>
```

```twig
{# templates/default/index.html.twig #}
{% include 'default/header.html.twig' %}
```

8. **Macro dans Twig**

```twig
{# templates/macros.html.twig #}
{% macro input(name, value) %}
    <input type="text" name="{{ name }}" value="{{ value }}">
{% endmacro %}
```

```twig
{# templates/default/index.html.twig #}
{% import 'macros.html.twig' as macros %}
{{ macros.input('username', 'John Doe') }}
```

---

### Conseils pratiques

- **Organisation des templates** : Placez tous vos fichiers Twig dans le dossier `templates/` pour bien organiser votre code.
- **Performance** : Utilisez les filtres Twig avec parcimonie dans les boucles pour éviter les ralentissements.
- **Sécurité** : Toujours échapper les données venant de l'utilisateur avec le filtre `escape` pour prévenir les attaques XSS.
- **Modularité** : Profitez de l'héritage et de l'inclusion pour maintenir vos templates DRY (Don't Repeat Yourself).

---

### Ressources complémentaires

- [Symfony - Template et Twig](https://symfony.com/doc/current/templates.html)
- [Documentation Twig](https://twig.symfony.com/doc/3.x/)