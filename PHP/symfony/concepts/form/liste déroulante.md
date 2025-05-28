#php #twig #dev 
# Ajouter une liste déroulante dans un formulaire Symfony avec Twig

## Introduction
Dans Symfony, les listes déroulantes (select) sont gérées via le composant `Form`. Cette fiche explique comment ajouter une liste déroulante dans un formulaire Symfony et l'afficher avec Twig.

---

## 1. Création du formulaire avec une liste déroulante
Symfony propose plusieurs manières d'ajouter une liste déroulante (`ChoiceType` est recommandé).

### Exemple de formulaire :

```php
// src/Form/ExampleType.php

namespace App\Form;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\Extension\Core\Type\ChoiceType;
use Symfony\Component\OptionsResolver\OptionsResolver;
use App\Entity\ExampleEntity;

class ExampleType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('category', ChoiceType::class, [
                'choices' => [
                    'Option 1' => 'option1',
                    'Option 2' => 'option2',
                    'Option 3' => 'option3',
                ],
                'placeholder' => 'Sélectionnez une option',
                'required' => true,
            ]);
    }

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => ExampleEntity::class,
        ]);
    }
}
````

---

## 2. Ajouter le formulaire dans le contrôleur

```php
// src/Controller/ExampleController.php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use App\Form\ExampleType;
use App\Entity\ExampleEntity;
use Doctrine\ORM\EntityManagerInterface;

class ExampleController extends AbstractController
{
    #[Route('/form', name: 'app_form')]
    public function index(Request $request, EntityManagerInterface $entityManager): Response
    {
        $example = new ExampleEntity();
        $form = $this->createForm(ExampleType::class, $example);

        $form->handleRequest($request);
        if ($form->isSubmitted() && $form->isValid()) {
            $entityManager->persist($example);
            $entityManager->flush();

            $this->addFlash('success', 'Formulaire soumis avec succès !');
            return $this->redirectToRoute('app_form');
        }

        return $this->render('form/index.html.twig', [
            'form' => $form->createView(),
        ]);
    }
}
```

---

## 3. Affichage du formulaire avec Twig

```twig
{# templates/form/index.html.twig #}

{% extends 'base.html.twig' %}

{% block body %}
    <h1>Formulaire avec liste déroulante</h1>

    {{ form_start(form) }}
        <div>
            {{ form_label(form.category) }}
            {{ form_widget(form.category) }}
        </div>
        <button type="submit">Soumettre</button>
    {{ form_end(form) }}
{% endblock %}
```

---

## 4. Cas particuliers

### a) Utilisation de données dynamiques (ex: depuis une base de données)

Si vous voulez générer la liste déroulante à partir d'une entité stockée en base de données, utilisez `EntityType` :

```php
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use App\Entity\Category;

$builder->add('category', EntityType::class, [
    'class' => Category::class,
    'choice_label' => 'name', // Utilise le champ "name" comme label
    'placeholder' => 'Sélectionnez une catégorie',
]);
```

### b) Liste déroulante avec valeurs pré-sélectionnées

Si vous voulez pré-sélectionner une valeur spécifique, assurez-vous que l'entité associée a bien une valeur par défaut.

```php
$builder->add('category', ChoiceType::class, [
    'choices' => [
        'Option 1' => 'option1',
        'Option 2' => 'option2',
    ],
    'data' => 'option2', // Définit 'Option 2' comme valeur par défaut
]);
```

---

## 5. Erreurs fréquentes et solutions

| Problème                                                | Solution                                                                               |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Le formulaire ne s'affiche pas                          | Vérifiez que le contrôleur passe bien la variable `form` à Twig                        |
| Les options ne s'affichent pas dans la liste            | Assurez-vous que les valeurs de `choices` sont bien définies et cohérentes             |
| Erreur "Expected argument of type string, object given" | Si vous utilisez `EntityType`, assurez-vous que `choice_label` est défini correctement |

---

## Conclusion

Cette fiche vous permet d'ajouter une liste déroulante dans un formulaire Symfony avec Twig. Pour aller plus loin, explorez l'ajout d'Ajax pour charger dynamiquement les options.

---

## Ressources supplémentaires

- [Documentation officielle Symfony - Forms](https://symfony.com/doc/current/forms.html)
- [Symfony Form ChoiceType](https://symfony.com/doc/current/reference/forms/types/choice.html)

```

Tu peux copier-coller ce fichier Markdown pour l’utiliser comme référence. Si tu veux approfondir un point ou adapter le formulaire à un besoin spécifique, dis-moi ! 🚀
```