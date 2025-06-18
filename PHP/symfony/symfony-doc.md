
---

# Documentation PHP de A à Z

## 1. Introduction à PHP

PHP (Hypertext Preprocessor) est un langage de script côté serveur conçu pour le développement web.

```php
<?php
// Exemple simple de script PHP
echo "Hello, World!";
?>
```

## 2. Installation

1. Téléchargez [XAMPP](https://www.apachefriends.org/) ou [WAMP](https://www.wampserver.com/).
2. Installez et configurez un serveur local.
3. Placez vos fichiers PHP dans le dossier `htdocs` ou équivalent.

---

## 3. Syntaxe de Base

### 3.1 Variables

```php
<?php
$nom = "Léonard";
$age = 25;
echo "Bonjour $nom, vous avez $age ans.";
?>
```

### 3.2 Types de Données

```php
<?php
$entier = 42;          // Integer
$flottant = 3.14;      // Float
$texte = "Bonjour";    // String
$booleen = true;       // Boolean
$tableau = [1, 2, 3];  // Array
?>
```

---

## 4. Structures de Contrôle

### 4.1 Conditions

```php
<?php
if ($age >= 18) {
    echo "Adulte";
} else {
    echo "Mineur";
}
?>
```

### 4.2 Boucles

#### Boucle `for`

```php
<?php
for ($i = 0; $i < 5; $i++) {
    echo "Itération $i\n";
}
?>
```

#### Boucle `while`

```php
<?php
$i = 0;
while ($i < 5) {
    echo "Itération $i\n";
    $i++;
}
?>
```

---

## 5. Fonctions

```php
<?php
function addition($a, $b) {
    return $a + $b;
}
echo addition(3, 5); // 8
?>
```

---

## 6. Manipulation de Formulaires HTML

```php
<!-- formulaire.html -->
<form action="traitement.php" method="post">
    <input type="text" name="nom" placeholder="Votre nom">
    <button type="submit">Envoyer</button>
</form>
```

```php
<?php
// traitement.php
$nom = $_POST['nom'];
echo "Bonjour, $nom";
?>
```

---

## 7. Manipulation de Bases de Données

### Connexion à MySQL

```php
<?php
$conn = new mysqli("localhost", "root", "", "ma_base");

if ($conn->connect_error) {
    die("Erreur de connexion : " . $conn->connect_error);
}
echo "Connecté à la base de données";
?>
```

### Requêtes

```php
<?php
// Insérer des données
$conn->query("INSERT INTO utilisateurs (nom, email) VALUES ('Leonard', 'email@example.com')");

// Récupérer des données
$result = $conn->query("SELECT * FROM utilisateurs");
while ($row = $result->fetch_assoc()) {
    echo $row['nom'];
}
?>
```

---

# Documentation Symfony de A à Z

## 1. Introduction à Symfony

Symfony est un framework PHP basé sur le modèle MVC, utilisé pour développer des applications web robustes.

---

## 2. Installation

1. Installez [Composer](https://getcomposer.org/).
2. Installez Symfony CLI :
    
    ```bash
    curl -sS https://get.symfony.com/cli/installer | bash
    ```
    
3. Créez un projet :
    
    ```bash
    symfony new mon_projet --webapp
    ```
    

---

## 3. Structure de Symfony

### Dossiers principaux :

- `src/`: Code source (contrôleurs, entités, etc.).
- `templates/`: Templates Twig.
- `config/`: Configuration.

---

## 4. Création d'un Contrôleur

```bash
php bin/console make:controller HomeController
```

### Exemple de Contrôleur

```php
<?php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;

class HomeController extends AbstractController {
    public function index(): Response {
        return $this->render('home/index.html.twig', [
            'message' => 'Bienvenue sur Symfony !',
        ]);
    }
}
```

---

## 5. Routes

Les routes définissent les URL disponibles.

### Exemple de Route

```yaml
# config/routes.yaml
home:
    path: /
    controller: App\Controller\HomeController::index
```

---

## 6. Twig : Le moteur de templates

```twig
<!-- templates/home/index.html.twig -->
<!DOCTYPE html>
<html>
    <head>
        <title>{{ message }}</title>
    </head>
    <body>
        <h1>{{ message }}</h1>
    </body>
</html>
```

---

## 7. Doctrine : ORM intégré

### Configuration

```bash
php bin/console make:entity
```

### Exemple d'Entité

```php
<?php
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
class Utilisateur {
    #[ORM\Id, ORM\GeneratedValue, ORM\Column(type: "integer")]
    private $id;

    #[ORM\Column(type: "string", length: 100)]
    private $nom;
}
?>
```

### Migration

```bash
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

---

## 8. Formulaires

```bash
php bin/console make:form
```

### Exemple de Formulaire

```php
<?php
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class UtilisateurType extends AbstractType {
    public function buildForm(FormBuilderInterface $builder, array $options) {
        $builder
            ->add('nom');
    }
}
?>
```
