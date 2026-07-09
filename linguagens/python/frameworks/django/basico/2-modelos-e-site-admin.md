---
title: Modelos e o site Admin
aliases:
  - Sem título
tipo: guia
linguagem: python
versao_linguagem: 3.12+
tags:
  - guia
nivel: basico
status: em desenvolvimento
atualizado: 2026-07-08
links:
  - https://docs.djangoproject.com/pt-br/5.2/intro/tutorial02/
---
[< Parte 1 - Requisições e Respostas](1-requisicoes-e-respostas.md)
# Modelos e o Site Admin

## Configurando o banco de dados

### settings.py

Agora, abra o `mysite/settings.py`. Ele é um módulo normal do _Python_ com variáveis de módulo representando as configurações do _Django_.

#### LANGUAGE_CODE

- Define a língua na qual os menus e as ferramentas nativas do painel administrativo do _Django_ são exibidos.

- Ajuda a formatar datas, números e moedas de acordo com o padrão cultural daquele país.

- Atua como a linguagem base para os seus próprios arquivos de tradução, caso seu projeto suporte múltiplos idiomas.

Mude a `LANGUAGE_CODE` para o desejado:

``` python
LANGUAGE_CODE = 'pt-BR' # Configura o idioma do sistema para o Português do Brasil.
```

#### TIME_ZONE

Por padrão o _Django_ grava todos os dados de data/hora no banco de dados no formato UTC, o `TIME_ZONE` diz ao _Django_ para converter esses dados para o fuso horário local quando for exibir as informações ao usuário.

Mude o `TIME_ZONE` para o seu fuso horário:

``` python
TIME_ZONE = 'America/Sao_Paulo' # Define o fuso horário oficial.
```

#### DATABASES

Por padrão a configuração do `DATABASES` usa o _SQLite_, ele é bem simples e ideal para quem está aprendendo sobre este framework. Mas em projetos reais é recomendado usar bancos de dados mais robustos como _PostgreSQL_.

``` python
DATABASES = {  
    'default': {  
        'ENGINE': 'django.db.backends.sqlite3',  
        'NAME': BASE_DIR / 'db.sqlite3',  
    }  
}
```

Caso queira usar o _SQLite_, não precisa alterar nada nessa parte, apenas pula para a próxima seção.

##### Configurando o Django para se conectar ao Postgres

Antes de conectar o _Django_ ao _Postgres_, você precisa ter uma instância local do _Postgres_ em execução. A maneira mais fácil de fazer isso é usando o _Docker_:

``` bash
$ docker run -d -p 5432:5432 --name postgres-db -e POSTGRES_PASSWORD=minha_senha -e POSTGRES_USER=meu_usuario postgres:18
```

Lembre-se de alterar o `meu_usuario` para um nome de usuário de sua preferência e `minha_senha` para uma senha de sua preferência.

Agora voltando para o arquivo `settings.py`, substitua o valor `default` pela seguinte configuração:

``` python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'nome_do_banco',
        'USER': 'meu_usuario',
        'PASSWORD': 'minha_senha',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

> [!WARNING] Cuidado com dados sensíveis expostos
> Não é recomendado colocar dados sensíveis como `PASSWORD` escritas em pleno texto caso você vá subir este projeto em um repositório no git. Para proteger esses tipos de dados, é recomendado usar [variáveis de ambiente](variaveis-de-ambiente.md).

Além disso, é necessário instalar uma outra dependência para que o _Django_ saiba conversar com o Banco de Dados, a `psycopg2`. Para instalar esse pacote, vou estar utilizando o [poetry](poetry.md), mas você pode usar o [gerenciadores de pacotes](linguagens/python/basico/gerenciadores-de-pacotes/README.md) que preferir.

``` bash
$ poetry add psycopg2 --group dev
```

Ou se preferir usar a versão mais recente deste pacote:

``` bash
$ poetry add "psycopg[binary,pool]" --group dev # instala o pacote psycopg ^3.3.4 junto com psycopg-binary e psycopg-pool
```

Por questões de segurança, conflitos de memória e otimização, não é recomendado usar esse pacote em ambiente de produção, por isso usei `--group dev` para isolar ele no ambiente de desenvolvimento, nesse caso o grupo de dependências `dev` no _poetry_.

>[!INFO] Suporte do Django para `psycopg3`
> Ele é totalmente suportado pelo Django 4.2 e versões mais atuais! Oferece melhor performance, suporte async nativo e API modernizada. 
> 
> Lembre-se de configurar `ENGINE: 'django.db.backends.postgresql'` dentro do `DATABASES` no `settings.py`.


#### INSTALLED_APPS

Esta lista informa ao _Django_ quais aplicativos nativos, de terceiros ou criados por você devem ser carregados no projeto. Ele ativa funcionalidades, permite a criação de tabelas no banco de dados e registra arquivos estáticos e templates.

Por padrão, o `INSTALLED_APPS` contém as seguintes aplicações, que vêm com o _Django_:

- **django.contrib.admin** - O site de administração, ativa o painel de administração visual do Django (`/admin`).

- **django.contrib.auth** - Um sistema de autenticação, controla o sistema de usuários, senhas, grupos e permissões.

- **django.contrib.contenttypes** - Um framework para tipos de conteúdo, permite que o Django rastreie todos os modelos criados no seu projeto e crie relações genéricas entre eles.

- **django.contrib.sessions** - Um framework de sessão, permite que o servidor guarde dados do usuário entre os cliques (como itens no carrinho).

- **django.contrib.messages** - Um framework de envio de mensagem, exibe notificações temporárias para o usuário (ex.: "Cadastro realizado com sucesso").

- **django.contrib.staticfiles** - Um framework para gerenciamento de arquivos, gerencia arquivos CSS, JavaScript e imagens.


> [!CAUTION] Cuidado ao modificar esta lista
A maioria dessas aplicações são opcionais, você pode removê-las caso realmente não precise de alguma delas, mas lembre-se de atualizar o `MIDLLEWARE` e `TEMPLATES` para evitar erros de dependência.

Quando você roda o servidor, o _Django_ não varre as pastas do projeto inteiro procurando códigos, ele lê apenas o que está explicitamente declarado nesta lista. Então sempre que você criar aplicativos no seu projeto, adicione o nome dele na lista:

``` python
INSTALLED_APPS = [  
    'django.contrib.admin',  
    'django.contrib.auth',  
    'django.contrib.contenttypes',
    'django.contrib.sessions',  
    'django.contrib.messages',
    'django.contrib.staticfiles', 
    'polls.apps.PollsConfig'  # agora o Django está rastreando seu aplicativo 'polls'
]
```

Algumas dessas aplicações fazem uso de pelo menos uma tabela no banco de dados, então precisamos criar essas tabelas no banco de dados para poder usá-las. Para isso rode o seguinte comando:

``` bash
$ python manage.py migrate
```


## Criando modelos

> [!INFORMATION] Filosofia do Django
> O Django segue o princípio DRY (Don't Repeat Yourself). O objetivo é definir os modelos em um único lugar e derivar as coisas a partir dos modelos. 
>
> Isso inclui as migrações, elas são inteiramente derivadas do seu arquivo de modelos que o Django usa para atualizar o esquema de banco de dados para coincidir com seus modelos atuais.

Vamos criar dois modelos: `Question` e `Choice` que são representados por simples classes _Python_. Edite o arquivo `polls/models.py` para que fique assim:

``` python
# polls/models.py

from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

Aqui, cada modelo é representado por uma classe derivada da classe [`django.db.models.Model`](https://docs.djangoproject.com/pt-br/5.2/ref/models/instances/#django.db.models.Model "django.db.models.Model"). Cada modelo possui alguns atributos de classe, as quais por sua vez representa um campo/coluna do banco de dados no modelo.

Cada campo é representado por uma instância de uma classe [`Field`](https://docs.djangoproject.com/pt-br/5.2/ref/models/fields/#django.db.models.Field "django.db.models.Field") – por exemplo, [`CharField`](https://docs.djangoproject.com/pt-br/5.2/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") para campos do tipo caractere e [`DateTimeField`](https://docs.djangoproject.com/pt-br/5.2/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField") para data/hora. Isto diz ao _Django_ qual tipo de dado cada campo contém.


