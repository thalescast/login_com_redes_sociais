# Login com redes sociais em Django
Utilizando o Django-allauth para login com redes sociais

## Instalando
 
```pip install django-allauth```


## Ir no settings.py

```python
INSTALLED_APPS = [
    # ...
    # Required
    'django.contrib.sites',  # new
    
    # Django-Allauth
    'allauth',  # new
    'allauth.account',  # new
    'allauth.socialaccount',  # new
    
    # Providers you want enable. In this case, I just want two: Google and Facebook
    'allauth.socialaccount.providers.google',  # new
    'allauth.socialaccount.providers.facebook',  # new
    # My Apps
    'app',
]

SITE_ID = 2  # Be carreful with ID site. Look always at the Django admin 
LOGIN_REDIRECT_URL = 'url_after_success_login'
```

Deve-se, depois, acrescentar também o backend de autenticação:

```python
AUTHENTICATION_BACKENDS = (  # new
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
)
```

## Criando as URLs
Nessa etapa, é necessário que se vá no arquivo do projeto de urls e insira a respectiva rota:

```python
  # ...
  path('accounts/', include(allauth.urls)),
  # ...
```

Para que se tenha acesso a seguinte lista:

```
- accounts/signup/ [name='account_signup']
- accounts/login/ [name='account_login']
- accounts/logout/ [name='account_logout']
- accounts/password/change/ [name='account_change_password']
- accounts/password/set/ [name='account_set_password']
- accounts/inactive/ [name='account_inactive']
- accounts/email/ [name='account_email']
- etc.

```
## Criando templates/allauth/accounts
Neste meu exemplo, eu quis fazer override em alguns templates. Dessa forma, tive que criar na raiz do meu projeto o caminho templates/allauth/accounts e adicionar os que eu queria, como, por exemplo, o login.html.

```bash
$
├──login_com_redes_sociais
  ├── app
  ├── myenv
  ├── myproject
  ├── templates
  ├    ├── allauth
  ├       └── account
  ├           └── login.html     
  ├──static
```

Depois disso, ir no settings.py e setar a varíavel DIR, em TEMPLATES:
```python
'DIRS': [os.path.join(BASE_DIR, 'templates'), os.path.join(BASE_DIR, 'templates', 'allauth')],
```
## Migrando
```
python manage.py migrate

```
## Ir no localhost:8000/admin e verificar:

- Aplicação Contas (Endereço de e-mail)

- Aplicação Contas Sociais (Aplicações Sociais, Contas Sociais, Tokens de Aplicativo Social)

- Aplicação Sites (Sites)

## Adicionar novo site no Admins

É necessário para quando for obter as crendenciais do Google, Facebook, etc., inserindo-se o endereço do respectivo site. Por exemplo, 127.0.0.1:8000

## Providers adicionados no settings.py

Se no INSTALLED_APPS tem ```allauth.socialaccount.providers.google``` e ```allauth.socialaccount.providers.facebook```, então deve-se acrescentar em Aplicativos Sociais, no Admin, as respectivas redes sociais.

## Adicionando uma Aplicação socialaccount

Para cada OAuth based provider, adicionar uma rede social (aplicação socialaccount):
- Para o Google, tem que ir na [console developers](https://console.developers.google.com)
- Criar uma aplicação (web application) e adquirir as credenciais: Client ID e a senha.
- Inserir o endereço do site (127.0.0.1:8000)
- Inserir o endereço re callback (127.0.0.1:8000/accounts/nome_rede_social/login/callback)

### Ir no Admin e criar um Aplicativo social com as credenciais obtidas no passo anterior.
