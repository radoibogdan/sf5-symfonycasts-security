# Création Api

## Démarrer serveur en local
```bash
$ symfony serve -d
```
Lien pour accéder à l'interface: https://127.0.0.1:8000

## Git 
```bash
$ git init
$ git remote add origin https://github.com/radoibogdan/-sf5-symfonycasts-security.git
$ git add .
$ git commit -m "initial commit + create user"
```

## Lignes jouées:
### Installer API et dépendances
```bash
$ composer update
$ symfony console doctrine:database:create
$ symfony console doctrine:migrations:migrate
$ symfony console doctrine:fixtures:load
$ yarn install
$ composer require security
$ symfony console make:user
$ symfony console make:entity (add firstName property)
$ symfony console make:migration
$ symfony console d:m:m
$ symfony console make:factory
$ symfony console doctrine:fixtures:load
$ symfony console doctrine:query:sql 'Select * FROM user'
```

Pour le json des objets (voir UserController)
```bash
$ composer req serializer
```

# Registration form

```bash
$ composer require form validator
$ symfony console make:registration-form
```

Configure the Symfony application to use Bootstrap 5 styles when rendering forms
config/packages/twig.yaml  
 [Symfony Bootstrap 5 doc][1]  
```bash
    twig:  
        form_themes: ['bootstrap_5_layout.html.twig']
```

Pour l'auto enregistrement, modifie RegistrationController + services.yaml pour donner un alias a $formLoginAuthenticator
```bash
$ symfony console debug:container form_login
```
services.yaml
```bash
    bind:
        bool $isDebug: '%kernel.debug%'
        $formLoginAuthenticator: '@security.authenticator.form_login.main'
```

# Make QuestionVoter - Autorise l'acces à certaines sections
```bash
$ symfony console make:voter
```
Voir fichiers:

    Security/QuestionVoter
    src/Controller/QuestionController.php
    templates/question/show.html.twig

# Vérifie l'email
Crée route dans RegistrationController -> verify
```bash
$ composer req symfonycasts/verify-email-bundle
```

# Limiter le nb de tentatives de connexions via le formulaire de login
Edit security.yaml  
login_throttling: true
```bash
$ composer require symfony/rate-limiter
voir options
$ symfony console debug:config security
login_throttling:
    max_attempts: 5
    interval: '1 minute'
    lock_factory: null
```

# Events
Créer CheckVerifiedUserSubscriber
```bash
$ symfony console debug:event --dispatcher=security.event_dispatcher.main
```

# 2 Factor Authentification
https://symfony.com/bundles/SchebTwoFactorBundle/current/installation.html
https://symfony.com/bundles/SchebTwoFactorBundle/current/providers/totp.html
```bash
$ composer req 2fa
$ composer req endroid/qr-code
$ symfony console debug:config scheb_two_factor
```
## Etapes
- Edit User Rajoute totpSecret property
- Edit security.yaml et scheb_2fa.yaml
- Créé twig templates : 
  - security/enable2fa.html.twig 
  - security/2fa_form.html.twig
  - registration/resend_verify_email.html.twig
- Créé routes 
  - RegistrationController.php->app_verify_resend_email
  - SecurityController.php
    - app_2fa_authenticate
    - app_qr_code


# Tests (pas sur ce projet)
Jouer les tests (srs/tests):

```bash
$ php bin/phpunit
```

[1]: https://symfony.com/doc/current/form/bootstrap5.html
