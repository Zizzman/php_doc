

PHP propose des fonctionnalités robustes pour manipuler les dates et heures. Cette documentation détaille les fonctions principales avec des exemples pratiques pour vous permettre de travailler efficacement avec des données temporelles.

---

## 1. `date()`

Retourne une chaîne formatée représentant la date et l'heure actuelles.

### Syntaxe :

```php
string date ( string $format [, int $timestamp = time() ] )
```

- `$format` : Le format de sortie (ex. `Y-m-d`, `H:i:s`).
- `$timestamp` : (Optionnel) L'horodatage Unix à formater.

### Exemple :

```php
<?php
echo date("Y-m-d H:i:s"); // Ex : 2024-12-22 15:30:45
?>
```

---

## 2. `time()`

Retourne l'horodatage Unix actuel (le nombre de secondes écoulées depuis le 1er janvier 1970).

### Syntaxe :

```php
int time ( void )
```

### Exemple :

```php
<?php
echo time(); // Ex : 1734895445
?>
```

---

## 3. `mktime()`

Crée un horodatage Unix pour une date spécifique.

### Syntaxe :

```php
int mktime ( int $hour , int $minute , int $second , int $month , int $day , int $year [, int $is_dst ] )
```

- Les paramètres spécifient les composantes de la date.

### Exemple :

```php
<?php
$timestamp = mktime(0, 0, 0, 12, 25, 2024);
echo date("Y-m-d", $timestamp); // Ex : 2024-12-25
?>
```

---

## 4. `strtotime()`

Convertit une chaîne de texte en un horodatage Unix.

### Syntaxe :

```php
int strtotime ( string $datetime [, int $baseTimestamp = time() ] )
```

### Exemple :

```php
<?php
$timestamp = strtotime("next Friday");
echo date("Y-m-d", $timestamp); // Ex : 2024-12-27 (si aujourd'hui est le 22 décembre 2024)
?>
```

---

## 5. Classe `DateTime`

Une classe orientée objet pour gérer les dates et heures.

### Instanciation :

```php
DateTime __construct ([ string $datetime = "now" [, DateTimeZone $timezone = NULL ]] )
```

### Exemple :

```php
<?php
$date = new DateTime("2024-12-25");
echo $date->format("Y-m-d"); // Ex : 2024-12-25
?>
```

---

## 6. `DateTime::modify()`

Modifie une date en utilisant des chaînes relatives (ex. "+1 day").

### Syntaxe :

```php
public DateTime DateTime::modify ( string $modifier )
```

### Exemple :

```php
<?php
$date = new DateTime("2024-12-25");
$date->modify("+1 week");
echo $date->format("Y-m-d"); // Ex : 2025-01-01
?>
```

---

## 7. `DateTimeZone`

Spécifie le fuseau horaire pour une date.

### Exemple :

```php
<?php
$timezone = new DateTimeZone("Europe/Paris");
$date = new DateTime("now", $timezone);
echo $date->format("Y-m-d H:i:s");
?>
```

---

## 8. `date_default_timezone_set()` et `date_default_timezone_get()`

Définit ou récupère le fuseau horaire par défaut.

### Exemple :

```php
<?php
date_default_timezone_set("Europe/Paris");
echo date("Y-m-d H:i:s"); // Ex : 2024-12-22 15:30:45
?>
```

---

## 9. Différences de Dates : `DateTime::diff()`

Calcule la différence entre deux dates.

### Exemple :

```php
<?php
$date1 = new DateTime("2024-12-25");
$date2 = new DateTime("2025-01-01");
$interval = $date1->diff($date2);
echo $interval->format("%a jours"); // Ex : 7 jours
?>
```

---

## 10. Formats de Dates Personnalisés

Les formats acceptés par `date()` et `DateTime::format()`.

### Exemple :

- `Y` : Année complète (ex. 2024).
- `m` : Mois (ex. 12).
- `d` : Jour (ex. 22).
- `H` : Heure (24 heures).
- `i` : Minutes.
- `s` : Secondes.

### Exemple :

```php
<?php
echo date("l, d F Y"); // Ex : Sunday, 22 December 2024
?>
```

---

## Conclusion

La gestion des dates et heures en PHP est flexible et puissante. En combinant des fonctions procédurales comme `date()` et des approches orientées objet avec `DateTime`, vous pouvez répondre à tous vos besoins liés au temps. Expérimentez ces fonctionnalités pour maîtriser leur usage !