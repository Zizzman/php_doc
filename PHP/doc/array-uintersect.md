
#### **Description**

La fonction `array_uintersect` renvoie un tableau contenant les valeurs communes à deux ou plusieurs tableaux, comparées à l'aide d'une fonction de rappel (callback) personnalisée.

---

#### **Syntaxe**

```php
array array_uintersect(array $array1, array $array2, callable $callback, ...);
```

---

#### **Paramètres**

1. **`$array1`** (obligatoire)
    
    - Le premier tableau d'entrée.
    - C'est le tableau principal à comparer avec les autres tableaux.
2. **`$array2`** (obligatoire)
    
    - Le second tableau d'entrée.
    - Ses valeurs seront comparées à celles de `$array1`.
3. **`$callback`** (obligatoire)
    
    - Une fonction de rappel (callback) définissant la logique de comparaison entre les éléments.
    - La fonction doit accepter deux paramètres et retourner un entier :
        - 0 si les éléments sont considérés égaux,
        - un entier négatif si le premier élément est "plus petit",
        - un entier positif si le premier élément est "plus grand".
4. **`...`** (optionnel)

    - Vous pouvez ajouter plus de tableaux à comparer.

---

#### **Valeur de retour**

Un tableau contenant les valeurs communes entre les tableaux, comparées en utilisant la fonction de rappel.

- Les clés associatives des éléments du tableau retourné sont héritées de `$array1`.

---

#### **Exemples**

##### **Exemple 1 : Intersection simple avec comparaison personnalisée**

```php
function case_insensitive_compare($a, $b) {
    return strcasecmp($a, $b); // Comparaison insensible à la casse
}

$array1 = ["Apple", "Banana", "Cherry"];
$array2 = ["banana", "apple", "Date"];

$result = array_uintersect($array1, $array2, "case_insensitive_compare");

print_r($result);
```

**Sortie :**

```php
Array
(
    [0] => Apple
    [1] => Banana
)
```

---

##### **Exemple 2 : Comparaison numérique personnalisée**

```php
function numerical_compare($a, $b) {
    return $a - $b;
}

$array1 = [1, 2, 3, 4];
$array2 = [3, 4, 5, 6];

$result = array_uintersect($array1, $array2, "numerical_compare");

print_r($result);
```

**Sortie :**

```php
Array
(
    [2] => 3
    [3] => 4
)
```

---

#### **Notes importantes**

- Contrairement à `array_intersect`, qui utilise une comparaison stricte ou standard, `array_uintersect` permet d'utiliser une logique de comparaison définie par l'utilisateur via un callback.
- La fonction ne préserve pas les clés des tableaux `$array2`, `$array3`, etc., mais les clés du premier tableau `$array1` sont conservées dans le résultat.

---

#### **Voir aussi**

- [array_intersect()](https://www.php.net/manual/fr/function.array-intersect.php) : Intersection des tableaux avec une comparaison stricte.
- [array_uintersect_assoc()](https://www.php.net/manual/fr/function.array-uintersect-assoc.php) : Comme `array_uintersect`, mais inclut aussi une comparaison des clés.
- [usort()](https://www.php.net/manual/fr/function.usort.php) : Permet de trier un tableau avec une fonction de comparaison utilisateur.

---

Ceci vous aidera-t-il pour votre projet ou un besoin spécifique ? 😊