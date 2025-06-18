### Documentation Symfony : Console Commands

#### Description

Les **Console Commands** dans Symfony permettent de créer des commandes en ligne de commande qui sont exécutées dans le terminal. Ces commandes peuvent être utilisées pour automatiser des tâches récurrentes, exécuter des scripts ou interagir avec votre application depuis la ligne de commande.

Symfony fournit un outil de ligne de commande intégré, `bin/console`, qui permet d'exécuter des commandes système ou des commandes personnalisées.

---

### Créer une Commande Symfony

Les commandes Symfony sont créées en étendant la classe `Command` de Symfony.

#### Exemple de Commande Symfony

```php
// src/Command/HelloCommand.php
namespace App\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class HelloCommand extends Command
{
    protected static $defaultName = 'app:hello';

    protected function configure()
    {
        $this->setDescription('Affiche un message de bienvenue');
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $output->writeln('Bonjour Symfony!');
        return Command::SUCCESS;
    }
}
```

Dans cet exemple :

- La classe `HelloCommand` étend `Command`.
- Le nom de la commande est défini par la constante `protected static $defaultName = 'app:hello';`.
- La méthode `execute()` définit la logique qui sera exécutée lors de l'appel de la commande.

---

### Enregistrer la Commande

Les commandes personnalisées doivent être enregistrées pour être reconnues par Symfony. Ce processus se fait automatiquement avec la commande `make:command`, mais vous pouvez aussi enregistrer la commande manuellement en la déclarant dans les services.

#### Exemple d'Enregistrement Manuel

Si vous n'utilisez pas `make:command`, vous pouvez enregistrer la commande dans `config/services.yaml` :

```yaml
# config/services.yaml
services:
    App\Command\HelloCommand:
        tags: ['console.command']
```

Le tag `console.command` permet à Symfony d'enregistrer la commande pour qu'elle soit accessible via `bin/console`.

---

### Exécuter une Commande

Une fois la commande définie et enregistrée, vous pouvez l'exécuter via la ligne de commande :

```bash
bin/console app:hello
```

Cela exécutera la commande et affichera le message `Bonjour Symfony!` dans le terminal.

---

### Paramètres et Arguments

Les commandes peuvent accepter des paramètres et des arguments pour rendre les commandes dynamiques.

#### Exemple de Commande avec Argument

```php
// src/Command/GreetCommand.php
namespace App\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class GreetCommand extends Command
{
    protected static $defaultName = 'app:greet';

    protected function configure()
    {
        $this->setDescription('Affiche un message de salutation')
             ->addArgument('name', InputArgument::REQUIRED, 'Le nom de la personne');
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $name = $input->getArgument('name');
        $output->writeln('Bonjour, ' . $name . '!');
        return Command::SUCCESS;
    }
}
```

Exécution de la commande avec un argument :

```bash
bin/console app:greet John
```

Résultat :

```
Bonjour, John!
```

#### Types de Paramètres

- **InputArgument::REQUIRED** : L'argument est obligatoire.
- **InputArgument::OPTIONAL** : L'argument est facultatif.

---

### Options

Les options sont similaires aux arguments mais elles sont nommées et sont souvent facultatives.

#### Exemple de Commande avec Option

```php
// src/Command/OptionCommand.php
namespace App\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class OptionCommand extends Command
{
    protected static $defaultName = 'app:option';

    protected function configure()
    {
        $this->setDescription('Affiche un message avec une option')
             ->addOption('upper', null, InputOption::VALUE_NONE, 'Affiche en majuscules');
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $message = 'Bonjour Symfony!';
        
        if ($input->getOption('upper')) {
            $message = strtoupper($message);
        }

        $output->writeln($message);
        return Command::SUCCESS;
    }
}
```

Exécution de la commande avec une option :

```bash
bin/console app:option --upper
```

Résultat :

```
BONJOUR SYMFONY!
```

#### Types d'Options

- **InputOption::VALUE_NONE** : L'option ne prend pas de valeur.
- **InputOption::VALUE_REQUIRED** : L'option nécessite une valeur.
- **InputOption::VALUE_OPTIONAL** : L'option peut prendre une valeur, mais ce n'est pas obligatoire.

---

### Gestion des Erreurs

Lorsque des erreurs surviennent dans l'exécution d'une commande, vous pouvez gérer les exceptions ou valider les entrées.

#### Exemple de Gestion d'Erreur

```php
// src/Command/ErrorCommand.php
namespace App\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Exception\InvalidArgumentException;

class ErrorCommand extends Command
{
    protected static $defaultName = 'app:error';

    protected function configure()
    {
        $this->setDescription('Commande avec gestion d\'erreur')
             ->addArgument('age', InputArgument::REQUIRED, 'L\'âge de la personne');
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $age = $input->getArgument('age');
        
        if ($age < 0) {
            throw new InvalidArgumentException('L\'âge ne peut pas être négatif');
        }
        
        $output->writeln('Age: ' . $age);
        return Command::SUCCESS;
    }
}
```

Exécution avec erreur :

```bash
bin/console app:error -5
```

Cela lancera une exception `InvalidArgumentException` avec le message d'erreur.

---

### Conseils et Bonnes Pratiques

- **Nommez correctement vos commandes** : Utilisez des noms clairs et logiques, tels que `app:generate:report` ou `app:user:create`.
- **Documentez les commandes** : Ajoutez des descriptions et des exemples d'utilisation dans la méthode `configure()` pour aider les utilisateurs.
- **Validez les entrées** : Validez les arguments et options afin d’éviter des erreurs d'exécution.
- **Utilisez des services dans les commandes** : Injectez des services dans vos commandes pour effectuer des actions plus complexes.

---

### Ressources Complémentaires

- [Documentation officielle Symfony : Console](https://symfony.com/doc/current/console.html)
- [Liste des Commandes Symfony par défaut](https://symfony.com/doc/current/reference/console.html)