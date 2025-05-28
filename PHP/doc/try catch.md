# Documentation Complète sur les Blocs `try-catch` en PHP

## Introduction

Le bloc `try-catch` en PHP est un outil fondamental pour gérer les exceptions et assurer la robustesse de votre code. Il permet de capturer et traiter des erreurs ou situations exceptionnelles qui pourraient autrement interrompre l'exécution normale du programme.

---

## Structure de Base d'un `try-catch`

### Syntaxe

```php
try {
    // Code qui peut potentiellement lancer une exception
} catch (ExceptionType $e) {
    // Traitement de l'exception
}
```

### Exemple Simple

```php
try {
    // Diviser par zéro (erreur)
    $result = 10 / 0;
} catch (DivisionByZeroError $e) {
    echo 'Erreur : ' . $e->getMessage();
}
```

---

## Utilisation de Blocs `try-catch-finally`

Le mot-clé `finally` permet d'exécuter du code indépendamment du fait qu'une exception ait été levée ou non.

### Exemple

```php
try {
    $file = fopen("example.txt", "r");
    if (!$file) {
        throw new Exception("Impossible d'ouvrir le fichier.");
    }
} catch (Exception $e) {
    echo 'Erreur : ' . $e->getMessage();
} finally {
    if (isset($file) && is_resource($file)) {
        fclose($file);
        echo "Fichier fermé.";
    }
}
```

---

## Types d'Exceptions

En PHP, les exceptions peuvent provenir de plusieurs sources :

### Exceptions Standards

- `Exception`: Classe de base pour toutes les exceptions.
- `ErrorException`: Représente des erreurs PHP transformées en exceptions.

### Exceptions Spécifiques

- `PDOException`: Levée par PDO lors de problèmes avec la base de données.
- `InvalidArgumentException`: Pour des arguments invalides dans une fonction.

### Création d'Exceptions Personnalisées

```php
class CustomException extends Exception {}

try {
    throw new CustomException("Erreur personnalisée");
} catch (CustomException $e) {
    echo $e->getMessage();
}
```

---

## Bonnes Pratiques

### 1. **Ne Capturer que ce que vous Pouvez Gérer**

Ne capturez pas une exception si vous ne pouvez pas proposer une solution ou un traitement approprié.

#### Exemple à éviter :

```php
try {
    $result = 10 / 0;
} catch (Exception $e) {
    // Rien à faire ici...
}
```

#### Correct :

```php
try {
    $result = 10 / 0;
} catch (DivisionByZeroError $e) {
    echo 'Division par zéro interdite !';
}
```

---

### 2. **Précisez le Type d'Exception**

Précisez les types d'exceptions que vous voulez capturer pour éviter d'attraper des erreurs inattendues.

#### Exemple :

```php
try {
    // Code pouvant lever plusieurs types d'exceptions
} catch (PDOException $e) {
    echo 'Erreur de base de données : ' . $e->getMessage();
} catch (Exception $e) {
    echo 'Erreur générale : ' . $e->getMessage();
}
```

---

### 3. **Utilisez  Finally pour Libérer des Ressources**

Libérez toujours les ressources comme les fihiers ou les connexions à la base de données dans un bloc `finally`.

---

### 4. **Ne Cachez Pas les Exceptions Sans Logging**

Assurez-vous de journaliser les exceptions capturées.

#### Exemple :

```php
try {
    throw new Exception("Erreur critique");
} catch (Exception $e) {
    error_log($e->getMessage());
    echo "Une erreur s'est produite, veuillez contacter l'administrateur.";
}
```

---

## Points d'Attention

1. **Performance** : Les exceptions ne doivent pas remplacer une logique de validation. Utilisez-les uniquement pour les erreurs inattendues.
    
2. **Propagation des Exceptions** : Si une exception ne peut pas être gérée localement, relancez-la avec `throw`.
    

```php
try {
    // Code
} catch (Exception $e) {
    // Logger, puis relancer
    error_log($e->getMessage());
    throw $e;
}
```

3. **Exceptions vs Erreurs** : En PHP, les erreurs fatales (ég., division par zéro) peuvent être capturées depuis PHP 7 grâce à  `Throwable`.

---

## Exemples Avancés

### Propagation des Exceptions

```php
function divide($a, $b) {
    if ($b === 0) {
        throw new DivisionByZeroError("Impossible de diviser par zéro");
    }
    return $a / $b;
}

try {
    echo divide(10, 0);
} catch (DivisionByZeroError $e) {
    echo $e->getMessage();
}
```

### Exception et Validation

```php
function processInput($data) {
    if (!isset($data['name'])) {
        throw new InvalidArgumentException("Le nom est requis");
    }

    // Traitement
    return true;
}

try {
    processInput([]);
} catch (InvalidArgumentException $e) {
    echo 'Validation échouée : ' . $e->getMessage();
}
```

---

## Liste exception

### **1. Exceptions Standard**

#### **Exception**

- Classe de base pour toutes les exceptions.
- Elle peut être utilisée directement ou étendue pour créer des exceptions personnalisées.

#### **ErrorException**

- Transforme les erreurs PHP (comme `E_WARNING` ou `E_NOTICE`) en exceptions.
- Utilisée souvent avec `set_error_handler()` pour capturer des erreurs classiques sous forme d'exception.

---

### **2. Exceptions de Type SPL (Standard PHP Library)**

Les exceptions SPL sont des exceptions prédéfinies dans PHP pour des situations spécifiques.

#### **InvalidArgumentException**

- Levée lorsqu'un argument passé à une fonction ou méthode est invalide.

#### **LengthException**

- Utilisée lorsqu'une longueur ou une taille est invalide.

#### **OutOfRangeException**

- Levée lorsqu'une valeur est en dehors de la plage attendue.

#### **LogicException**

- Classe de base pour des exceptions dues à des erreurs dans la logique du programme.
    - **BadFunctionCallException** : Levée lorsqu'une fonction appelée est invalide.
    - **BadMethodCallException** : Variante pour des méthodes.
    - **DomainException** : Pour des erreurs liées à un domaine de valeur non valide.
    - **InvalidArgumentException** : Arguments invalides.
    - **LengthException** : Longueur incorrecte.

#### **RuntimeException**

- Classe de base pour des erreurs détectées uniquement à l'exécution.
    - **OutOfBoundsException** : Pour des erreurs liées à des indices invalides.
    - **OverflowException** : Pour des débordements.
    - **UnderflowException** : Lorsque des ressources ou données attendues manquent.
    - **UnexpectedValueException** : Pour des valeurs inattendues.

### **3. Exceptions de la Gestion des Fichiers**

#### **FileNotFoundException**

- Levée lorsque l'accès à un fichier spécifié échoue.

#### **DirectoryNotFoundException**

- Levée pour des répertoires inexistants (dans des frameworks spécifiques comme Symfony).

### **4. Exceptions de Base de Données**

####  **PDOException**

- Levée par PDO pour des erreurs SQL ou des problèmes de connexion.

#### **ConnectionException**

- Levée dans des bibliothèques spécifiques lorsqu'une connexion échoue.

### **5. Exceptions HTTP**

#### **HttpException**

- Utilisée pour signaler des erreurs liées à HTTP (par exemple, erreur 404, 500).

#### **ClientException**

- Levée pour des erreurs liées à des requêtes HTTP client.

### **6. Exceptions pour les APIs et Frameworks**

#### **Symfony\Component\HttpKernel\Exception\HttpException**

- Levée dans Symfony pour des erreurs HTTP spécifiques.

#### **Laravel\Validation\ValidationException**

- Levée lors d'une erreur de validation des données dans Laravel.

---

### **7. Exceptions Personnalisées**

#### Création d'une Exception Personnalisée

Vous pouvez étendre la classe `Exception` pour créer vos propres exceptions.

```php
class CustomException extends Exception
{
    public function errorMessage(): string
    {
        return "Erreur personnalisée : " . $this->getMessage();
    }
}

try {
    throw new CustomException("Une erreur est survenue.");
} catch (CustomException $e) {
    echo $e->errorMessage();
}
```

---

### **Conseils Pratiques**

1. **Utilisez des Exceptions Spécifiques** : Capturez des exceptions précises pour améliorer la lisibilité et la gestion des erreurs.
2. **Propagation des Exceptions** : Si une exception ne peut être gérée localement, relancez-la avec `throw $e`.
3. **Logging** : Journalisez toutes les exceptions pour faciliter le débogage.
4. **Evitez de Tout Attraper** : Ne capturez pas `Throwable` ou `Exception` à moins d'avoir une bonne raison.
5. **Utilisez des Messages Clairs** : Les messages d'exception doivent être explicites pour identifier rapidement les problèmes.

---

## **Exemple Pratique**

Un exemple combinant plusieurs types d'exceptions :

```php
try {
    $db = new PDO('mysql:host=localhost;dbname=test', 'user', 'password');
    if (!$db) {
        throw new PDOException("Connexion à la base de données échouée.");
    }
    $result = $db->query("SELECT * FROM users WHERE id = 1");
    if (!$result) {
        throw new UnexpectedValueException("Aucun résultat trouvé.");
    }
} catch (PDOException $e) {
    error_log($e->getMessage());
    echo "Erreur de base de données.";
} catch (UnexpectedValueException $e) {
    echo $e->getMessage();
} catch (Exception $e) {
    echo "Une erreur inattendue est survenue.";
}
```


## Conclusion

Les blocs `try-catch` sont indispensables pour gérer les exceptions et améliorer la résilience de vos applications PHP. En suivant les bonnes pratiques et en faisant attention aux pièges courants, vous pouvez écrire un code plus robuste et maintenable.