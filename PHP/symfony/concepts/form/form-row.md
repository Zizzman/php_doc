
# Méthode `form_row()` en Symfony

## Vue d'ensemble

La méthode `form_row()` est un helper Twig qui génère un groupe de champ de formulaire complet, incluant le label, le champ de saisie et les potentielles erreurs de validation.

## Signature de la méthode

```twig
{{ form_row(form.champ, options = []) }}
```

## Paramètres

### Paramètre principal
- `form.champ` (obligatoire) : Le champ spécifique du formulaire à rendre

### Options disponibles
- `label`: Personnaliser le libellé du champ
- `attr`: Attributs HTML personnalisés
- `help`: Texte d'aide supplémentaire
- `row_attr`: Attributs sur la div conteneur
- `label_attr`: Attributs sur le label

## Exemples de code

### Exemple basique
```twig
{{ form_row(form.nom) }}
{{ form_row(form.email) }}
```

### Personnalisation complète
```twig
{{ form_row(form.username, {
    'label': 'Nom d\'utilisateur',
    'attr': {
        'class': 'form-control',
        'placeholder': 'Choisissez un pseudo'
    },
    'help': 'Le nom doit faire entre 3 et 20 caractères'
}) }}
```

### Modification des attributs de la ligne
```twig
{{ form_row(form.description, {
    'row_attr': {
        'class': 'mb-3 custom-description-row',
        'data-type': 'user-description'
    }
}) }}
```

## Cas d'erreur courants

### 1. Champ inexistant
```twig
{# Incorrect #}
{{ form_row(form.champInexistant) }}  {# Génèrera une erreur #}

{# Correct #}
{{ form_row(form.champDefini) }}
```

### 2. Options incorrectes
```twig
{# Incorrect #}
{{ form_row(form.email, {'invalid_option': true}) }}  {# Option non reconnue #}

{# Correct #}
{{ form_row(form.email, {'attr': {'class': 'email-input'}}) }}
```

## Conseils et bonnes pratiques

1. **Flexibilité maximale** : Utilisez les options pour personnaliser chaque champ
2. Gérez les erreurs de validation visuellement
3. Utilisez des classes CSS pour un rendu cohérent
4. Ajoutez des textes d'aide pour guider l'utilisateur

## Gestion des erreurs de validation

```twig
{{ form_row(form.email, {
    'attr': {
        'class': form.email.vars.errors|length > 0 ? 'is-invalid' : ''
    }
}) }}
```

## Exemple complet de formulaire

```twig
{{ form_start(form) }}
    {{ form_row(form.nom, {
        'label': 'Votre nom complet',
        'attr': {'class': 'form-control'}
    }) }}

    {{ form_row(form.email, {
        'label': 'Adresse email',
        'help': 'Nous ne partagerons jamais votre email',
        'attr': {'class': 'form-control'}
    }) }}

    {{ form_row(form.mot_de_passe, {
        'label': 'Mot de passe',
        'attr': {
            'class': 'form-control',
            'type': 'password'
        }
    }) }}

    <button type="submit" class="btn btn-primary">Envoyer</button>
{{ form_end(form) }}
```

## Intégration avec Bootstrap

```twig
{{ form_row(form.champ, {
    'attr': {'class': 'form-control'},
    'row_attr': {'class': 'mb-3'}
}) }}
```

## Notes importantes

- `form_row()` combine automatiquement `form_label()`, `form_widget()`, et `form_errors()`
- Hautement personnalisable via les options
- Supporte tous les types de champs (texte, email, checkbox, etc.)

## Cas spéciaux

### Champs booléens
```twig
{{ form_row(form.newsletter, {
    'label': 'Recevoir la newsletter',
    'attr': {'class': 'form-check-input'}
}) }}
```

### Champs de type select
```twig
{{ form_row(form.pays, {
    'label': 'Pays de résidence',
    'attr': {'class': 'form-select'}
}) }}
```

Cette documentation offre une vue d'ensemble détaillée de la méthode `form_row()` en Symfony, avec des exemples pratiques, des conseils et des cas d'utilisation courants.