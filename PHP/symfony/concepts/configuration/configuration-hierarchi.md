# Hiérarchie et ordre de priorité des fichiers de configuration

## Description

Symfony utilise une hiérarchie de fichiers de configuration pour déterminer comment votre application doit se comporter dans différents environnements (dev, prod, test). Comprendre l'ordre de priorité de ces fichiers est essentiel pour s'assurer que les paramètres corrects sont appliqués au bon moment.

---

## Ordre de priorité des fichiers de configuration

Symfony charge les fichiers de configuration dans un ordre spécifique. Voici l'ordre général :

1. **Fichiers globaux** :
   - `config/packages/*.yaml`
   - `config/services.yaml`
   - `config/routes.yaml`

2. **Fichiers spécifiques à l'environnement** :
   - `config/packages/<env>/*.yaml` (par exemple, `config/packages/prod/doctrine.yaml`)
   - `config/services_<env>.yaml` (par exemple, `config/services_prod.yaml`)
   - `config/routes_<env>.yaml`

3. **Variables d'environnement** :
   - Fichiers `.env`, `.env.local`, `.env.<env>`, etc.

---

## Détails des niveaux de configuration

### **1. Configuration globale**

Les fichiers situés dans `config/packages/` et `config/` définissent la configuration par défaut de votre application.

#### Exemple :

- `config/packages/framework.yaml`
- `config/packages/doctrine.yaml`

### **2. Configuration par environnement**

Les fichiers spécifiques à un environnement surchargent la configuration globale pour cet environnement particulier.

#### Exemple :

- `config/packages/prod/doctrine.yaml` : S'applique uniquement en production.
- `config/packages/dev/monolog.yaml` : S'applique uniquement en développement.

### **3. Variables d'environnement**

Les variables d'environnement définies dans les fichiers `.env` peuvent surcharger les configurations précédentes.

#### Exemple :

- `.env` : Variables d'environnement par défaut.
- `.env.local` : Variables spécifiques à votre machine locale.
- `.env.prod` : Variables pour l'environnement de production.

---

## Exemple pratique

Supposons que vous ayez les fichiers suivants :

### **1. `config/packages/doctrine.yaml`**

```yaml
doctrine:
  dbal:
    url: '%env(DATABASE_URL)%'
    driver: 'pdo_mysql'
    charset: utf8mb4
````

### **2. `config/packages/prod/doctrine.yaml`**

```yaml
doctrine:
  dbal:
    driver: 'pdo_pgsql'
    charset: utf8
```

### **3. `.env`**

```dotenv
DATABASE_URL=mysql://user:pass@127.0.0.1:3306/mydb
```

### **4. `.env.prod`**

```dotenv
DATABASE_URL=pgsql://user:pass@db.example.com:5432/proddb
```

---

### **Comportement attendu**

- **En environnement de développement (`APP_ENV=dev`)** :
    
    - Le fichier `config/packages/doctrine.yaml` est chargé.
    - Les variables de `.env` sont utilisées.
    - **Résultat** : Utilisation de MySQL avec les paramètres définis dans `.env`.
- **En environnement de production (`APP_ENV=prod`)** :
    
    - Le fichier `config/packages/doctrine.yaml` est chargé, puis surchargé par `config/packages/prod/doctrine.yaml`.
    - Les variables de `.env.prod` sont utilisées.
    - **Résultat** : Utilisation de PostgreSQL avec les paramètres définis dans `.env.prod`.

---

## Priorité des fichiers `.env`

Les fichiers `.env` sont chargés dans l'ordre suivant, le dernier surchargera les précédents :

1. `.env` : Fichier principal avec les valeurs par défaut.
2. `.env.local` : Surcharge les valeurs de `.env` (non versionné, spécifique à la machine).
3. `.env.<APP_ENV>.local` : Surcharge les valeurs précédentes pour un environnement spécifique (non versionné).
4. `.env.<APP_ENV>` : Surcharge les valeurs pour un environnement spécifique.

---

## Bonnes pratiques

1. **Utilisez les fichiers spécifiques à l'environnement** pour définir des configurations différentes selon l'environnement (dev, prod, test).
    
2. **Évitez de versionner les fichiers `.env.local`** qui contiennent des configurations spécifiques à votre machine ou des secrets.
    
3. **Centralisez les configurations communes** dans les fichiers globaux pour éviter les duplications.
    
4. **Testez vos configurations** en utilisant la commande :
    
    ```bash
    php bin/console config:dump-reference <nom_du_bundle>
    ```
    
    Par exemple :
    
    ```bash
    php bin/console config:dump-reference doctrine
    ```
    

---

## Liens connexes

- [[configuration-overview]] : Vue d'ensemble de la configuration dans Symfony.
- [[environment-variables]] : Gestion des variables d'environnement avec `.env`.
- [[services-configuration]] : Configurer les services dans `services.yaml`.
- [[parameters-definition]] : Définir et utiliser des paramètres globaux.
- [[security-configuration]] : Configurer les paramètres de sécurité.

---

## Ressources supplémentaires

- [Documentation officielle : Hiérarchie de configuration](https://symfony.com/doc/current/configuration.html#configuration-order-and-hierarchy)
- [Meilleures pratiques Symfony](https://symfony.com/doc/current/best_practices.html#configuration)

---

## Tags
#symfony #configuration #hierarchie #environnements #variablenvironnement 