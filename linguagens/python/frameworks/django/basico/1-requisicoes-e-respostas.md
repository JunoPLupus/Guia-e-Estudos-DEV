---
title: Requisições e Respostas
tipo: guia
linguagem: python
versao_linguagem: "3.12+"
tags:
  - guia
  - django-5-2
nivel: basico
status: pronto
atualizado: 2026-07-08
links:
  - https://docs.djangoproject.com/pt-br/5.2/intro/tutorial01/
---
[< Parte 0 - Criação e Configuração](0-criacao-e-configuracao.md)
# Requisições e Respostas

## Criando a aplicação de enquetes: Polls

Ao criar uma aplicação no projeto, o _Django_ gera automaticamente a estrutura básica de diretório dela para não precisar se preocupar em criar diretórios.

> [!NOTE] Diferença entre Projetos e Aplicações
> Um app é uma aplicação web que executa algo, por exemplo: um sistema de blog, uma base de dados de registros públicos ou uma pequena aplicação de enquete. Um projeto é uma coleção de configurações e apps para um website específico. Um projeto pode conter múltiplos apps. Um app pode estar em múltiplos projetos.

As aplicações podem viver em qualquer lugar do _Python Path_. Neste tutorial, vamos criar nossa aplicação `polls` dentro da pasta `django_tutorial`.

``` bash
$ python manage.py startapp polls
```

---
## Escrevendo a primeira view

``` python
# polls/views.py

from django.http import HttpResponse  
  
def index(request) -> HttpResponse:  
    return HttpResponse("Olá, mundo! Você está no index do 'polls'.")
```

Para acessar no navegador, será necessário mapear a url. Essas configurações de url são definidas dentro de cada aplicação _Django_ do projeto e são arquivos nomeados `urls.py`.

``` python
# polls/urls.py

from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

Próximo passo é configurar as rotas definidas em `polls.urls` dentro de `mysite`.

``` python
# mysite/urls.py

from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```


O `path()` espera pelo menos 2 argumentos: **rota** e **view**. A ideia por trás do `include()` é facilitar plugar URLs, sempre que o _Django_ encontrar este método, ele importa todas as urls daquele arquivo com o pré-fixo definido, neste caso, `polls/`. Não é obrigatório o uso do _include_, mas é altamente recomendado para melhor modularização e organização das rotas do projeto.


> [!NOTE] Quando usar `include()`
> Você deve sempre usar `include()` quando quiser incluir outras urls. A única exceção é `admin.site.urls` que é uma URLconf pré-configurada pelo Django para ser o site admin padrão.

Agora com as rotas devidamente configuradas, só rodar o servidor novamente e acessar http://localhost:8000/polls/.

``` bash
$ python manage.py runserver
```

[> Parte 2 - Modelos e o site Admin](2-modelos-e-site-admin.md)
