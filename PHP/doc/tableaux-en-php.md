

Les tableaux sont une structure de données essentielle en PHP, permettant de stocker et de manipuler des collections de valeurs. Voici une documentation complète des fonctions de gestion des tableaux les plus importantes, avec des exemples et des explications.

---

## 1. `array_push()`

Ajoute un ou plusieurs éléments à la fin d'un tableau.

### Syntaxe :

```php
int array_push ( array &$array , mixed ...$values )
```

### Exemple :

```php
<?php
$fruits = ["pomme", "banane"];
array_push($fruits, "orange", "ananas");
print_r($fruits);
?>
```

**Sortie :**

```
Array
(
    [0] => pomme
    [1] => banane
    [2] => orange
    [3] => ananas
)
```

---

## 2. `array_pop()`

Supprime et retourne le dernier élément d'un tableau.

### Syntaxe :

```php
mixed array_pop ( array &$array )
```

### Exemple :

```php
<?php
$fruits = ["pomme", "banane", "orange"];
$lastFruit = array_pop($fruits);
echo "Fruit supprimé : $lastFruit\n";
print_r($fruits);
?>
```

**Sortie :**

```
Fruit supprimé : orange
Array
(
    [0] => pomme
    [1] => banane
)
```

---

## 3. `array_shift()`

Supprime et retourne le premier élément d'un tableau.

### Syntaxe :

```php
mixed array_shift ( array &$array )
```

### Exemple :

```php
<?php
$fruits = ["pomme", "banane", "orange"];
$firstFruit = array_shift($fruits);
echo "Fruit supprimé : $firstFruit\n";
print_r($fruits);
?>
```

**Sortie :**

```
Fruit supprimé : pomme
Array
(
    [0] => banane
    [1] => orange
)
```

---

## 4. `array_unshift()`

Ajoute un ou plusieurs éléments au début d'un tableau.

### Syntaxe :

```php
int array_unshift ( array &$array , mixed ...$values )
```

### Exemple :

```php
<?php
$fruits = ["banane", "orange"];
array_unshift($fruits, "pomme", "fraise");
print_r($fruits);
?>
```

**Sortie :**

```
Array
(
    [0] => pomme
    [1] => fraise
    [2] => banane
    [3] => orange
)
```

---

## 5. `array_merge()`

Fusionne un ou plusieurs tableaux.

### Syntaxe :

```php
array array_merge ( array ...$arrays )
```

### Exemple :

```php
<?php
$array1 = ["rouge", "vert"];
$array2 = ["bleu", "jaune"];
$result = array_merge($array1, $array2);
print_r($result);
?>
```

**Sortie :**

```
Array
(
    [0] => rouge
    [1] => vert
    [2] => bleu
    [3] => jaune
)
```

---

## 6. `array_keys()`

Retourne toutes les clés d'un tableau.

### Syntaxe :

```php
array array_keys ( array $array [, mixed $search_value [, bool $strict = false ]] )
```

### Exemple :

```php
<?php
$person = ["nom" => "Alice", "age" => 25, "ville" => "Paris"];
$keys = array_keys($person);
print_r($keys);
?>
```

**Sortie :**

```
Array
(
    [0] => nom
    [1] => age
    [2] => ville
)
```

---

## 7. `array_values()`

Retourne toutes les valeurs d'un tableau.

### Syntaxe :

```php
array array_values ( array $array )
```

### Exemple :

```php
<?php
$person = ["nom" => "Alice", "age" => 25, "ville" => "Paris"];
$values = array_values($person);
print_r($values);
?>
```

**Sortie :**

```
Array
(
    [0] => Alice
    [1] => 25
    [2] => Paris
)
```

---

## 8. `array_slice()`

Extrait une portion d'un tableau.

### Syntaxe :

```php
array array_slice ( array $array , int $offset [, int $length = null [, bool $preserve_keys = false ]] )
```

### Exemple :

```php
<?php
$numbers = [0, 1, 2, 3, 4, 5];
$slice = array_slice($numbers, 2, 3);
print_r($slice);
?>
```

**Sortie :**

```
Array
(
    [0] => 2
    [1] => 3
    [2] => 4
)
```

---

## 9. `array_filter()`

Filtre les éléments d'un tableau en utilisant une fonction de rappel.

### Syntaxe :

```php
array array_filter ( array $array [, callable|null $callback = null , int $mode = 0 ] )
```

### Exemple :

```php
<?php
$numbers = [1, 2, 3, 4, 5];
$even = array_filter($numbers, function($num) {
    return $num % 2 === 0;
});
print_r($even);
?>
```

**Sortie :**

```
Array
(
    [1] => 2
    [3] => 4
)
```

---

## 10. `array_map()`

Applique une fonction à tous les éléments d'un tableau.

### Syntaxe :

```php
array array_map ( callable $callback , array $array [, array ...$arrays ] )
```

### Exemple :

```php
<?php
$numbers = [1, 2, 3, 4, 5];
squared = array_map(function($num) {
    return $num * $num;
}, $numbers);
print_r($squared);
?>
```

**Sortie :**

```
Array
(
    [0] => 1
    [1] => 4
    [2] => 9
    [3] => 16
    [4] => 25
)
```

---

### Conclusion

Ces fonctions permettent de manipuler les tableaux de manière flexible et puissante en PHP. Elles couvrent les opérations de base telles que l'ajout, la suppression, la fusion, et la transformation des données.

Pratiquez ces fonctions avec des cas d'utilisation concrets pour vous familiariser pleinement avec leur comportement !