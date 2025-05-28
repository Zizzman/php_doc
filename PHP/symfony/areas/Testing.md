### Documentation Symfony : Testing

#### Description

Les tests dans Symfony sont essentiels pour garantir que votre application fonctionne correctement. Symfony supporte divers types de tests, comme les tests unitaires, fonctionnels et d'intégration. Il fournit des outils comme PHPUnit pour effectuer les tests, et le framework Symfony propose des fonctionnalités pratiques pour faciliter la rédaction et l'exécution des tests.

---

### Types de Tests dans Symfony

1. **Tests Unitaires**  
    Les tests unitaires sont utilisés pour tester des parties spécifiques du code, comme les méthodes d'une classe.
    
2. **Tests Fonctionnels**  
    Les tests fonctionnels simulent une requête HTTP et testent le comportement global de votre application, y compris l'interaction entre les composants.
    
3. **Tests d'Intégration**  
    Les tests d'intégration vérifient l'interaction entre plusieurs composants ou services de votre application.
    

---

### Outils de Test dans Symfony

- **PHPUnit** : Symfony utilise PHPUnit comme framework de test principal. PHPUnit est un framework pour effectuer des tests unitaires et fonctionnels en PHP.
- **WebTestCase** : Pour les tests fonctionnels, Symfony fournit la classe `WebTestCase`, qui permet de simuler des requêtes HTTP et de vérifier les réponses.
- **Mocking** : Utilisation de "mocks" pour simuler les objets dans les tests unitaires.

---

### Configurer PHPUnit

#### Exemple de configuration PHPUnit (`phpunit.xml`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit bootstrap="vendor/autoload.php">
    <testsuites>
        <testsuite name="Symfony Project Test Suite">
            <directory>./tests</directory>
        </testsuite>
    </testsuites>
</phpunit>
```

- **bootstrap** : Inclut l'autoload de Composer pour charger les classes de l'application.
- **testsuites** : Définit le répertoire où PHPUnit recherche les tests.

---

### Écrire des Tests Unitaires

Les tests unitaires vérifient que chaque méthode ou fonction se comporte comme prévu. Pour écrire un test unitaire, il est essentiel de comprendre comment les dépendances sont gérées et comment les mocker si nécessaire.

#### Exemple de test unitaire

```php
// tests/Service/MyServiceTest.php

use PHPUnit\Framework\TestCase;
use App\Service\MyService;

class MyServiceTest extends TestCase
{
    public function testAddition()
    {
        $service = new MyService();
        $result = $service->add(2, 3);
        $this->assertEquals(5, $result);
    }
}
```

- **TestCase** : Classe de base pour les tests.
- **assertEquals** : Vérifie que les valeurs passées en argument sont égales.

---

### Écrire des Tests Fonctionnels

Les tests fonctionnels permettent de tester des fonctionnalités complètes de l'application, y compris l'interaction avec le serveur HTTP.

#### Exemple de test fonctionnel

```php
// tests/Controller/HomeControllerTest.php

use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class HomeControllerTest extends WebTestCase
{
    public function testIndex()
    {
        $client = static::createClient();
        $crawler = $client->request('GET', '/');
        
        $this->assertResponseIsSuccessful();
        $this->assertSelectorTextContains('h1', 'Welcome to Symfony');
    }
}
```

- **createClient** : Crée un client HTTP simulé pour envoyer des requêtes.
- **request** : Envoie une requête HTTP.
- **assertResponseIsSuccessful** : Vérifie que la réponse a un statut HTTP 2xx.
- **assertSelectorTextContains** : Vérifie que le texte spécifié est présent dans le sélecteur CSS.

---

### Tests d'Intégration

Les tests d'intégration vérifient que les différents composants de votre application interagissent correctement entre eux.

#### Exemple de test d'intégration

```php
// tests/Service/UserServiceTest.php

use Symfony\Bundle\FrameworkBundle\Test\KernelTestCase;
use App\Service\UserService;

class UserServiceTest extends KernelTestCase
{
    public function testCreateUser()
    {
        self::bootKernel();
        $container = self::$container;
        
        $userService = $container->get(UserService::class);
        $user = $userService->createUser('John Doe', 'john@example.com');
        
        $this->assertInstanceOf(User::class, $user);
        $this->assertEquals('John Doe', $user->getName());
    }
}
```

- **bootKernel** : Démarre le noyau de Symfony pour accéder aux services.
- **self::$container** : Accède au conteneur de services pour récupérer un service spécifique.

---

### Tester les Formulaires

Symfony offre des méthodes pour tester facilement les formulaires et les soumissions de données.

#### Exemple de test de formulaire

```php
// tests/Form/ContactFormTest.php

use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class ContactFormTest extends WebTestCase
{
    public function testSubmitForm()
    {
        $client = static::createClient();
        $crawler = $client->request('GET', '/contact');
        
        $form = $crawler->selectButton('Submit')->form();
        $form['contact[name]'] = 'John Doe';
        $form['contact[email]'] = 'john@example.com';
        
        $client->submit($form);
        
        $this->assertResponseRedirects('/thank-you');
    }
}
```

- **selectButton** : Sélectionne un bouton de formulaire pour l'interaction.
- **form** : Récupère le formulaire pour y insérer des données.
- **submit** : Soumet le formulaire et vérifie la redirection.

---

### Conseils et Bonnes Pratiques

1. **Organisez vos tests** : Placez vos tests dans des répertoires comme `tests/Controller`, `tests/Service`, ou `tests/Entity`.
2. **Isoler les tests** : Essayez de rendre vos tests indépendants. Utilisez des mocks et des stubs pour simuler les dépendances externes.
3. **Utilisez les assertions adaptées** : Symfony et PHPUnit offrent une large gamme de méthodes d'assertion pour valider les résultats (par exemple, `assertResponseIsSuccessful`, `assertSelectorTextContains`, etc.).
4. **Exécutez les tests régulièrement** : Assurez-vous d'exécuter vos tests fréquemment pour vérifier que les changements n'introduisent pas de régressions.
5. **Utilisez les tests fonctionnels pour tester des comportements complets** : Les tests fonctionnels sont particulièrement utiles pour valider des flux utilisateurs, comme des soumissions de formulaire ou des connexions.

---

### Ressources Complémentaires

- [Documentation officielle Symfony : Testing](https://symfony.com/doc/current/testing.html)
- [PHPUnit](https://phpunit.de/)
- [Symfony WebTestCase](https://symfony.com/doc/current/test/web_test_case.html)