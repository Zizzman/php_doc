### Documentation Symfony : Internationalization (i18n)

#### Description

L'internationalisation (i18n) dans Symfony permet de rendre votre application accessible dans plusieurs langues. Symfony fournit des outils pour gérer les traductions, la gestion des locales et l'adaptation des formats de date, heure, monnaie, etc.

---

### Configuration de la locale

La locale définit la langue et les paramètres culturels de votre application (comme le format de date, la devise, etc.).

#### Configuration de la locale par défaut

Dans le fichier `config/packages/framework.yaml`, vous pouvez configurer la locale par défaut :

```yaml
framework:
    default_locale: en
```

Cela définit la langue par défaut sur l'anglais (en).

#### Changer la locale dynamiquement

Vous pouvez modifier la locale en fonction des préférences de l'utilisateur ou de l'URL. Pour cela, vous pouvez utiliser un `LocaleListener` ou un service personnalisé.

##### Exemple avec un contrôleur :

```php
// src/Controller/LanguageController.php

namespace App\Controller;

use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\RedirectResponse;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\HttpFoundation\Session\SessionInterface;

class LanguageController
{
    /**
     * @Route("/language/{locale}", name="set_language")
     */
    public function setLanguage($locale, SessionInterface $session)
    {
        $session->set('_locale', $locale);
        return new RedirectResponse($session->get('_previous', '/'));
    }
}
```

Ici, nous définissons un contrôleur pour changer la langue et la stocker dans la session.

---

### Traductions avec Symfony

Symfony utilise des fichiers de traduction pour stocker les traductions des chaînes de texte dans l'application. Ces fichiers sont placés dans le répertoire `translations/` et sont utilisés selon la locale active.

#### Structure des fichiers de traduction

Les fichiers de traduction sont organisés par langue et peuvent être de type `.yaml`, `.xlf`, `.php`, etc.

Par exemple, un fichier de traduction pour l'anglais (`messages.en.yaml`) :

```yaml
# translations/messages.en.yaml
greeting: "Hello"
```

Et un fichier pour le français (`messages.fr.yaml`) :

```yaml
# translations/messages.fr.yaml
greeting: "Bonjour"
```

---

### Utilisation des traductions dans le code

Vous pouvez utiliser les traductions dans vos contrôleurs et templates en utilisant la fonction `trans` de Symfony.

#### Dans un contrôleur :

```php
// src/Controller/HelloController.php

namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Contracts\Translation\TranslatorInterface;

class HelloController
{
    public function greet(TranslatorInterface $translator)
    {
        $greeting = $translator->trans('greeting');
        return new Response($greeting);
    }
}
```

#### Dans un template Twig :

```twig
{# templates/hello/greet.html.twig #}

<h1>{{ 'greeting'|trans }}</h1>
```

---

### Gestion des formats spécifiques à la locale

Symfony permet également d'adapter le format des dates, heures, nombres et devises selon la locale active.

#### Exemple avec des dates :

```php
use Symfony\Component\Intl\DateFormatter\IntlDateFormatter;

$date = new \DateTime();
$formatter = new IntlDateFormatter('fr_FR', IntlDateFormatter::LONG, IntlDateFormatter::NONE);
echo $formatter->format($date);  // Affiche la date formatée en français
```

#### Exemple avec des devises :

```php
use Symfony\Component\Intl\NumberFormatter\NumberFormatter;

$currencyFormatter = new NumberFormatter('fr_FR', NumberFormatter::CURRENCY);
echo $currencyFormatter->formatCurrency(1234.56, 'EUR');  // Affiche 1 234,56 €
```

---

### Routage avec locale

Symfony vous permet de gérer les routes avec ou sans la locale dans l'URL.

#### Exemple de route avec locale dans l'URL :

```yaml
# config/routes.yaml
home:
    path: /{_locale}/home
    controller: App\Controller\HomeController::index
    requirements:
        _locale: en|fr|de
```

Cette configuration rend l'URL dynamique selon la locale choisie, par exemple `/en/home` ou `/fr/home`.

#### Définir la locale à partir de l'URL

Dans votre contrôleur, vous pouvez récupérer la locale de l'URL via le paramètre `_locale`.

```php
public function index($_locale)
{
    // $_locale est la locale extraite de l'URL
    // Vous pouvez l'utiliser pour changer dynamiquement la locale de l'application
}
```

---

### Conseils et Bonnes Pratiques

1. **Stocker la locale dans la session** : Si vous souhaitez que l'utilisateur garde sa langue préférée entre les différentes pages de votre application, stockez la locale dans la session ou dans un cookie.
2. **Utiliser des fallback** : Lorsque vous ajoutez des traductions, assurez-vous de prévoir une langue par défaut en cas de traduction manquante.
3. **Utiliser des fichiers YAML ou XLIFF** : Les fichiers de traduction en format YAML ou XLIFF sont très pratiques pour gérer plusieurs langues. L'option XLIFF est particulièrement utile pour une traduction professionnelle.
4. **Traduction de format de données** : Utilisez la fonction `trans` pour tout texte que vous souhaitez traduire, y compris les messages d'erreur, les messages de validation, etc.
5. **Optimisation des traductions** : Si vous avez un grand nombre de traductions, pensez à utiliser le cache de traduction pour améliorer les performances de votre application.

---

### Ressources Complémentaires

- [Documentation officielle Symfony i18n](https://symfony.com/doc/current/translation.html)
- [Symfony Localization](https://symfony.com/doc/current/intl.html)
- [Traduction avec Symfony et Twig](https://symfony.com/doc/current/translation/translation.html)