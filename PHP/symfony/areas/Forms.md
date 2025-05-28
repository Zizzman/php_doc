---
tags: 
cssclasses:
---
### Documentation Symfony : Les Forms


[[createForm()]]
#### Description

Le composant **Forms** de Symfony permet de gérer la création, la validation et l'affichage des formulaires dans une application web. Il fournit des outils puissants pour simplifier le processus de traitement des formulaires en combinant la gestion des données, des erreurs et de l'affichage.

---

### Création d'un formulaire

1. **Création d'une classe de formulaire** Symfony utilise des classes de formulaire pour définir les champs et leurs options. Vous devez créer une classe de formulaire, généralement dans le répertoire `src/Form/`.

```php
// src/Form/ContactType.php
namespace App\Form;

use App\Entity\Contact;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\EmailType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class ContactType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('name', TextType::class)
            ->add('email', EmailType::class)
            ->add('submit', SubmitType::class);
    }

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => Contact::class,
        ]);
    }
}
```

Dans cet exemple, nous avons défini un formulaire `ContactType` avec un champ `name`, un champ `email`, et un bouton `submit`.

2. **Création du formulaire dans un contrôleur**

```php
// src/Controller/ContactController.php
namespace App\Controller;

use App\Form\ContactType;
use App\Entity\Contact;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class ContactController extends AbstractController
{
    /**
     * @Route("/contact", name="contact")
     */
    public function contact(Request $request): Response
    {
        $contact = new Contact();

        $form = $this->createForm(ContactType::class, $contact);

        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            // Traitement des données du formulaire
            // Exemple: Enregistrer les données dans la base de données

            return $this->redirectToRoute('success');
        }

        return $this->render('contact/index.html.twig', [
            'form' => $form->createView(),
        ]);
    }
}
```

Ici, le formulaire est créé et manipulé dans le contrôleur. Nous passons également la vue du formulaire à notre template.

---

### Affichage d'un formulaire

Une fois le formulaire créé et passé au contrôleur, vous pouvez afficher le formulaire dans le template.

```twig
{# templates/contact/index.html.twig #}
<form method="post">
    {{ form_start(form) }}
    {{ form_row(form.name) }}
    {{ form_row(form.email) }}
    {{ form_row(form.submit) }}
    {{ form_end(form) }}
</form>
```

Les fonctions [[form_start()]], [[form_row()]], et [[form_end()]] sont utilisées pour rendre le formulaire dans le template.

---

### Validation des formulaires

Symfony fournit un système de validation des formulaires basé sur les contraintes.

1. **Exemple d'ajout de contraintes de validation**

```php
// src/Entity/Contact.php
namespace App\Entity;

use Symfony\Component\Validator\Constraints as Assert;

class Contact
{
    /**
     * @Assert\NotBlank()
     * @Assert\Length(min=2)
     */
    private $name;

    /**
     * @Assert\NotBlank()
     * @Assert\Email()
     */
    private $email;

    // getters and setters
}
```

Les annotations `@Assert\NotBlank()` et `@Assert\Length(min=2)` définissent des contraintes de validation pour le champ `name`, et `@Assert\Email()` valide le format de l'email.

2. **Afficher les erreurs de validation dans le template**

```twig
{# templates/contact/index.html.twig #}
<form method="post">
    {{ form_start(form) }}
    {{ form_row(form.name) }}
    {% if form.name.vars.errors|length > 0 %}
        <div class="errors">
            {% for error in form.name.vars.errors %}
                <p>{{ error.message }}</p>
            {% endfor %}
        </div>
    {% endif %}
    {{ form_row(form.email) }}
    {{ form_row(form.submit) }}
    {{ form_end(form) }}
</form>
```

---

### Types de champs de formulaire courants

Symfony offre de nombreux types de champs pour vos formulaires. Voici quelques-uns des types les plus utilisés :

- **TextType** : Champ de texte simple.
- **EmailType** : Champ pour une adresse email.
- **PasswordType** : Champ pour un mot de passe.
- **IntegerType** : Champ pour un entier.
- **ChoiceType** : Liste déroulante.
- **DateType** : Champ de sélection de date.

Exemple avec le type `ChoiceType` :

```php
$builder->add('category', ChoiceType::class, [
    'choices' => [
        'Option 1' => 1,
        'Option 2' => 2,
        'Option 3' => 3,
    ],
]);
```

---

### Gestion de la soumission du formulaire

Pour gérer la soumission du formulaire, vous devez utiliser la méthode `handleRequest()` et vérifier si le formulaire a été soumis et s'il est valide.

```php
$form->handleRequest($request);

if ($form->isSubmitted() && $form->isValid()) {
    // Traitement des données du formulaire
}
```

- **isSubmitted()** : Vérifie si le formulaire a été soumis.
- **isValid()** : Vérifie si le formulaire est valide en fonction des contraintes de validation.

---

### Conseils pratiques

- **Utilisation des Form Types** : Créez des classes de formulaire séparées pour chaque entité afin d'améliorer la réutilisabilité.
- **Validation** : Utilisez les annotations de validation directement dans les entités pour garantir la validité des données.
- **Rendu personnalisé** : Utilisez `form_row()` et `form_widget()` pour personnaliser le rendu de chaque champ si nécessaire.

---

### Ressources complémentaires

- [Symfony - Formulaires](https://symfony.com/doc/current/forms.html)
- [Symfony - Form Types](https://symfony.com/doc/current/reference/forms/types.html)