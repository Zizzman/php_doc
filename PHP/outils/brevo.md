# Documentation : Utiliser Brevo (ex-SendinBlue) pour l'envoi d'e-mails avec PHP Symfony

## Prérequis

1. **Compte Brevo** : Assurez-vous d'avoir un compte actif sur Brevo.
2. **Clé API** : Générez une clé API sur votre tableau de bord Brevo.
3. **Projet Symfony** : Ayez un projet Symfony configuré (version 5.4 ou ultérieure recommandée).

---

## Installation

### 1. Installer le SDK Brevo

Brevo fournit un SDK officiel pour PHP. Installez-le en utilisant Composer :

```bash
composer require sendinblue/api-v3-sdk
```

### 2. Installer la bibliothèque Symfony Mailer

Symfony Mailer simplifie l'envoi d'e-mails. Ajoutez-le si ce n'est pas déjà fait :

```bash
composer require symfony/mailer
```

---

## Configuration

### 1. Ajouter la clé API dans les variables d'environnement

Dans le fichier `.env`, ajoutez votre clé API Brevo :

```env
BREVO_API_KEY=your_api_key_here
```

### 2. Configurer Symfony Mailer

Dans le fichier `config/packages/mailer.yaml`, configurez Symfony Mailer pour utiliser Brevo via SMTP :

```yaml
framework:
    mailer:
        dsn: 'smtp://smtp-relay.sendinblue.com:587?encryption=tls&auth_mode=login&username=your_email@domain.com&password=%env(BREVO_API_KEY)%'
```

Remplacez `your_email@domain.com` par l'adresse e-mail associée à votre compte Brevo.

---

## Envoi d'e-mails avec Symfony Mailer

### 1. Créer un service pour l'envoi d'e-mails

Dans le dossier `src/Service`, créez un fichier `EmailService.php` :

```php
<?php

namespace App\Service;

use Symfony\Component\Mailer\MailerInterface;
use Symfony\Component\Mime\Email;

class EmailService
{
    private $mailer;

    public function __construct(MailerInterface $mailer)
    {
        $this->mailer = $mailer;
    }

    public function sendEmail(string $to, string $subject, string $content): void
    {
        $email = (new Email())
            ->from('your_email@domain.com')
            ->to($to)
            ->subject($subject)
            ->html($content);

        $this->mailer->send($email);
    }
}
```

Remplacez `your_email@domain.com` par votre adresse e-mail d'envoi.

### 2. Utiliser le service dans un contrôleur

Dans un contrôleur, injectez et utilisez le service :

```php
<?php

namespace App\Controller;

use App\Service\EmailService;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class EmailController extends AbstractController
{
    private $emailService;

    public function __construct(EmailService $emailService)
    {
        $this->emailService = $emailService;
    }

    #[Route('/send-email', name: 'send_email')]
    public function sendEmail(): Response
    {
        $this->emailService->sendEmail(
            'recipient@example.com',
            'Test Subject',
            '<h1>Hello from Symfony and Brevo!</h1>'
        );

        return new Response('Email sent successfully!');
    }
}
```

---

## Envoi d'e-mails transactionnels avec le SDK Brevo

Si vous préférez utiliser directement le SDK Brevo pour envoyer des e-mails transactionnels :

### 1. Configuration

Ajoutez la clé API dans votre service :

```php
<?php

namespace App\Service;

use SendinBlue\Client\Configuration;
use SendinBlue\Client\Api\TransactionalEmailsApi;

class BrevoEmailService
{
    private $apiInstance;

    public function __construct(string $brevoApiKey)
    {
        $config = Configuration::getDefaultConfiguration()->setApiKey('api-key', $brevoApiKey);
        $this->apiInstance = new TransactionalEmailsApi(null, $config);
    }

    public function sendTransactionalEmail(string $to, string $subject, string $content): void
    {
        $sendSmtpEmail = new \SendinBlue\Client\Model\SendSmtpEmail([
            'to' => [['email' => $to]],
            'subject' => $subject,
            'htmlContent' => $content,
            'sender' => ['name' => 'Your Name', 'email' => 'your_email@domain.com'],
        ]);

        $this->apiInstance->sendTransacEmail($sendSmtpEmail);
    }
}
```

### 2. Utilisation

Injectez et utilisez `BrevoEmailService` dans vos contrôleurs ou services.

---

## Conclusion

Vous pouvez maintenant utiliser Symfony avec Brevo pour envoyer des e-mails via SMTP ou leur SDK API. Adaptez les exemples fournis à vos besoins et assurez-vous de tester vos configurations avant de passer en production.