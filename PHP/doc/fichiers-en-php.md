
La gestion des fichiers est une fonctionnalité essentielle pour de nombreuses applications. PHP fournit des fonctions puissantes pour lire, écrire, créer, et manipuler des fichiers et des répertoires. Voici une documentation complète avec des exemples pratiques pour comprendre et utiliser ces fonctionnalités.

---

## 1. `fopen()`

Ouvre un fichier ou crée un fichier pour être manipulé.

### Syntaxe :

```php
resource fopen ( string $filename , string $mode [, bool $use_include_path = false [, resource $context ]] )
```

- `$filename` : Chemin du fichier.
- `$mode` : Mode d'ouverture du fichier (ex. `r`, `w`, `a`, etc.).

### Exemple :

```php
<?php
$file = fopen("example.txt", "w");
if ($file) {
    echo "Fichier ouvert avec succès.";
    fclose($file);
} else {
    echo "Impossible d'ouvrir le fichier.";
}
?>
```

---

## 2. `fwrite()`

Écrit des données dans un fichier ouvert.

### Syntaxe :

```php
int fwrite ( resource $handle , string $data [, int $length ] )
```

- `$handle` : Le pointeur de fichier obtenu avec `fopen()`.
- `$data` : Les données à écrire.

### Exemple :

```php
<?php
$file = fopen("example.txt", "w");
fwrite($file, "Bonjour, monde !\n");

fclose($file);
?>
```

---

## 3. `fread()`

Lit le contenu d'un fichier ouvert.

### Syntaxe :

```php
string fread ( resource $handle , int $length )
```

- `$length` : Nombre d'octets à lire.

### Exemple :

```php
<?php
$file = fopen("example.txt", "r");
$content = fread($file, filesize("example.txt"));
echo $content;

fclose($file);
?>
```

---

## 4. `fclose()`

Ferme un fichier ouvert.

### Syntaxe :

```php
bool fclose ( resource $handle )
```

### Exemple :

```php
<?php
$file = fopen("example.txt", "r");
// Lecture ou écriture
fclose($file);
?>
```

---

## 5. `file_get_contents()`

Lit tout le contenu d'un fichier en une seule opération.

### Syntaxe :

```php
string file_get_contents ( string $filename [, bool $use_include_path = false [, resource $context [, int $offset = 0 [, int $maxlen ]]]] )
```

### Exemple :

```php
<?php
$content = file_get_contents("example.txt");
echo $content;
?>
```

---

## 6. `file_put_contents()`

Écrit des données dans un fichier en une seule opération.

### Syntaxe :

```php
int file_put_contents ( string $filename , mixed $data [, int $flags = 0 [, resource $context ]] )
```

### Exemple :

```php
<?php
file_put_contents("example.txt", "Nouveau contenu\n");
?>
```

---

## 7. `unlink()`

Supprime un fichier.

### Syntaxe :

```php
bool unlink ( string $filename [, resource $context ] )
```

### Exemple :

```php
<?php
if (unlink("example.txt")) {
    echo "Fichier supprimé.";
} else {
    echo "Erreur lors de la suppression.";
}
?>
```

---

## 8. `fgets()`

Lit une ligne d'un fichier ouvert.

### Syntaxe :

```php
string fgets ( resource $handle [, int $length ] )
```

### Exemple :

```php
<?php
$file = fopen("example.txt", "r");
while (($line = fgets($file)) !== false) {
    echo $line;
}

fclose($file);
?>
```

---

## 9. `feof()`

Teste si la fin d'un fichier est atteinte.

### Syntaxe :

```php
bool feof ( resource $handle )
```

### Exemple :

```php
<?php
$file = fopen("example.txt", "r");
while (!feof($file)) {
    $line = fgets($file);
    echo $line;
}

fclose($file);
?>
```

---

## 10. `is_file()`

Vérifie si un chemin correspond à un fichier.

### Syntaxe :

```php
bool is_file ( string $filename )
```

### Exemple :

```php
<?php
if (is_file("example.txt")) {
    echo "C'est un fichier.";
} else {
    echo "Ce n'est pas un fichier.";
}
?>
```

---

## Conclusion

La gestion des fichiers en PHP est riche en fonctionnalités et permet de manipuler efficacement des données stockées. En combinant ces fonctions, vous pouvez créer des systèmes complexes comme des gestionnaires de fichiers ou des systèmes de stockage de données.

Pratiquez chaque fonction pour bien comprendre ses particularités et son comportement dans différents scénarios.