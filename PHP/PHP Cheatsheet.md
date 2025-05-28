

---

## PHP Cheatsheet

### Chapitre 1 : PHP

#### 1.1 Syntaxe de base

```php
<?php
echo "Hello, World!";
?>
```

#### 1.2 Variables

```php
<?php
$name = "John";
$age = 30;
$is_active = true;
?>
```

#### 1.3 Types de données

```php
<?php
$string = "Hello";
$integer = 42;
$float = 3.14;
$boolean = true;
$array = array("apple", "banana", "cherry");
$null = null;
?>
```

#### 1.4 Constantes

```php
<?php
define("SITE_NAME", "My Website");
echo SITE_NAME;
?>
```

#### 1.5 Structures de contrôle

- **If-Else**

```php
<?php
$age = 20;
if ($age >= 18) {
    echo "Adult";
} else {
    echo "Minor";
}
?>
```

- **Switch**

```php
<?php
$day = "Monday";
switch ($day) {
    case "Monday":
        echo "Start of the week";
        break;
    case "Friday":
        echo "End of the week";
        break;
    default:
        echo "Midweek";
}
?>
```

- **Boucles**

```php
<?php
// For Loop
for ($i = 0; $i < 5; $i++) {
    echo $i;
}

// While Loop
$i = 0;
while ($i < 5) {
    echo $i;
    $i++;
}

// Do-While Loop
$i = 0;
do {
    echo $i;
    $i++;
} while ($i < 5);

// Foreach Loop
$fruits = array("apple", "banana", "cherry");
foreach ($fruits as $fruit) {
    echo $fruit;
}
?>
```

#### 1.6 Fonctions

```php
<?php
function greet($name) {
    return "Hello, " . $name;
}
echo greet("Alice");
?>
```

#### 1.7 Tableaux

- **Tableaux indexés**

```php
<?php
$fruits = array("apple", "banana", "cherry");
echo $fruits[1]; // banana
?>
```

- **Tableaux associatifs**

```php
<?php
$person = array("name" => "John", "age" => 30);
echo $person["name"]; // John
?>
```

- **Tableaux multidimensionnels**

```php
<?php
$people = array(
    array("name" => "John", "age" => 30),
    array("name" => "Alice", "age" => 25)
);
echo $people[1]["name"]; // Alice
?>
```

#### 1.8 Superglobales

```php
<?php
// $_GET
echo $_GET['name'];

// $_POST
echo $_POST['name'];

// $_SERVER
echo $_SERVER['HTTP_HOST'];

// $_SESSION
session_start();
$_SESSION['user'] = "John";

// $_COOKIE
setcookie("user", "John", time() + (86400 * 30), "/"); // 86400 = 1 day
echo $_COOKIE['user'];
?>
```

#### 1.9 Gestion des erreurs

```php
<?php
// Try-Catch
try {
    $num = 2 / 0;
} catch (Exception $e) {
    echo 'Caught exception: ',  $e->getMessage(), "\n";
}

// Custom Error Handler
function customError($errno, $errstr) {
    echo "Error: [$errno] $errstr";
}
set_error_handler("customError");
echo($test);
?>
```


### Conclusion

En suivant ces étapes, vous pouvez intégrer un backend PHP avec MongoDB à un frontend React.js. Utilisez ce document comme référence rapide et guide pratique pour vos projets.

docker compose exec -it web php bin/console c:c

## print
dd($user);

## show routes
docker compose exec -it web php bin/console debug:router
