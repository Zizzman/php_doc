### Documentation Symfony : Migrations

#### Description

Les migrations dans Symfony permettent de gérer l'évolution de la base de données au fil du temps, de manière structurée et contrôlée. En utilisant Doctrine Migrations, vous pouvez versionner les changements de schéma de la base de données, les appliquer et les annuler de manière fiable et répétable.

---

### Installation et Configuration

#### Installation du bundle Doctrine Migrations

Si vous utilisez Symfony Flex, Doctrine Migrations est installé automatiquement. Sinon, vous pouvez l'ajouter à votre projet avec la commande suivante :

```bash
composer require doctrine/doctrine-migrations-bundle
```

Après installation, vous devez vous assurer que le fichier de configuration `config/packages/doctrine_migrations.yaml` existe et est correctement configuré.

Exemple de configuration dans `doctrine_migrations.yaml` :

```yaml
doctrine_migrations:
    migration_paths:
        'DoctrineMigrations': '%kernel.project_dir%/migrations'
    storage:
        table_storage:
            table_name: 'migration_versions'
```

Le répertoire des migrations par défaut est `migrations/` dans votre projet.

---

### Création d'une migration

Une migration est un fichier PHP généré automatiquement à partir des changements apportés à votre schéma de base de données. Pour créer une migration, utilisez la commande suivante :

```bash
php bin/console doctrine:migrations:diff
```

Cette commande compare l'état actuel de votre base de données avec les entités Doctrine et génère un fichier de migration qui contient les instructions SQL nécessaires pour mettre à jour la base de données.

Exemple de fichier de migration généré :

```php
<?php

namespace DoctrineMigrations;

use Doctrine\DBAL\Schema\Schema;
use Doctrine\Migrations\AbstractMigration;

class Version20231215000000 extends AbstractMigration
{
    public function up(Schema $schema): void
    {
        // Créer une table
        $this->addSql('CREATE TABLE user (id INT AUTO_INCREMENT NOT NULL, name VARCHAR(255) NOT NULL, PRIMARY KEY(id))');
    }

    public function down(Schema $schema): void
    {
        // Annuler la migration
        $this->addSql('DROP TABLE user');
    }
}
```

---

### Exécution des Migrations

Une fois la migration générée, vous pouvez l'exécuter avec la commande suivante :

```bash
php bin/console doctrine:migrations:migrate
```

Cela appliquera les migrations non exécutées et mettra à jour la base de données en conséquence. Il est recommandé d'utiliser cette commande sur votre environnement de production avec précaution, car elle modifie directement la base de données.

#### Option pour forcer l'exécution sans confirmation :

```bash
php bin/console doctrine:migrations:migrate --no-interaction
```

---

### Annulation d'une Migration

Si vous souhaitez annuler une migration ou revenir à une version précédente de la base de données, vous pouvez utiliser la commande suivante :

```bash
php bin/console doctrine:migrations:rollback
```

Cela annule la dernière migration exécutée et revient à l'état précédent de la base de données.

---

### Affichage de l'état des Migrations

Vous pouvez afficher l'état des migrations (qu'elles ont été appliquées ou non) en utilisant la commande suivante :

```bash
php bin/console doctrine:migrations:status
```

Cela fournit une liste de toutes les migrations avec leurs versions et leur état (appliqué ou non).

---

### Conseils et Bonnes Pratiques

1. **Utiliser les migrations sur tous les environnements** : Les migrations doivent être utilisées de manière cohérente sur tous les environnements (développement, test, production) pour garantir la synchronisation du schéma de la base de données.
2. **Ne pas modifier manuellement les migrations générées** : Évitez de modifier manuellement les fichiers de migration générés, sauf si vous avez une bonne raison. Mieux vaut générer une nouvelle migration pour chaque changement de schéma.
3. **Versionner les fichiers de migration** : Les fichiers de migration doivent être versionnés dans votre système de contrôle de version (par exemple, Git) pour assurer la traçabilité des changements de la base de données.
4. **Effectuer des sauvegardes avant les migrations** : Avant d'appliquer des migrations sur des bases de données sensibles (par exemple, en production), il est conseillé de faire des sauvegardes pour pouvoir revenir en arrière en cas d'erreur.
5. **Tests des migrations dans un environnement de développement** : Toujours tester vos migrations sur un environnement local ou de développement avant de les appliquer en production.

---

### Commandes Utiles

- `doctrine:migrations:diff` : Crée une nouvelle migration basée sur les différences entre les entités et la base de données.
- `doctrine:migrations:migrate` : Exécute les migrations non appliquées.
- `doctrine:migrations:rollback` : Annule la dernière migration.
- `doctrine:migrations:status` : Affiche l'état actuel des migrations.
- `doctrine:migrations:generate` : Génére un squelette de migration vide (utile pour les migrations manuelles).

---

### Ressources Complémentaires

- [Symfony Doctrine Migrations Documentation](https://symfony.com/doc/current/doctrine.html)
- [Doctrine Migrations Documentation](https://www.doctrine-project.org/projects/doctrine-migrations/en/latest/)