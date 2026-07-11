---
title: Variáveis de Ambiente em Django
aliases:
  - Variáveis de Ambiente em Django
tipo: guia
linguagem: python
versao_linguagem:
tags:
  - guia
nivel: basico
status: rascunho
atualizado: 2026-07-08T18:45:00
---
[< Django (Índice)](linguagens/python/frameworks/django/README.md)
# Variáveis de Ambiente em Django

Variáveis de ambiente são extremamente necessárias para proteger dados sensíveis, principalmente para projetos em repositórios do git. Para isso, temos duas bibliotecas: `django-environ` e `python-decouple`.

## Instalação

Para instalar esse pacote, vou estar utilizando o [poetry](poetry.md), mas você pode usar o [gerenciadores de pacotes](linguagens/python/basico/gerenciadores-de-pacotes/README.md) que preferir.

```bash
# Compatível com Python 3.9+
$ poetry add django-environ
```

OU

```bash
# Compatível com Python 2.7 e 3.6+
$ poetry add python-decouple

$ poetry add dj-database-url # o python-decouple puro não traduz uma url de banco de dados diretamente em um dicionário do Django, precisando desta biblioteca auxiliar
```

## Passos

### 1. Criar o arquivo `.env`

Crie um arquivo chamado `.env` na pasta principal do projeto (onde fica o `manage.py`) e adicione suas credenciais e configurações:

``` bash
SECRET_KEY=sua-chave-secreta-super-segura
ALLOWED_HOSTS=localhost,127.0.0.1
DATABASE_URL=postgres://usuario:senha@localhost:5432/meu_banco
PG_PASSWORD=sua-senha-aqui
```

### 2. Configurar o `settings.py`

#### django-environ

Abra o seu arquivo `settings.py` e adicione a inicialização do `django-environ` logo no início, para que ele carregue o `.env` antes de definir as variáveis principais:

``` python
import os
import environ


# Inicializa o ambiente
env = environ.Env()

# Constrói o caminho base do projeto
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# Lê o arquivo .env (se existir)
environ.Env.read_env(os.path.join(BASE_DIR, '.env'))

# --- Usando as variáveis ---

# Pega a string da chave secreta
SECRET_KEY = env('SECRET_KEY')

# Transforma uma string separada por vírgula em uma lista Python
ALLOWED_HOSTS = env.list('ALLOWED_HOSTS', default=['localhost'])

# Lê a URL do banco de dados e converte automaticamente para o formato do Django
DATABASES = {
    'default': env.db('DATABASE_URL', default='sqlite:///db.sqlite3')
}
```

#### python-decouple

Abra o seu arquivo `settings.py`, importe a função `config` e substitua os valores fixos pelas chamadas de ambiente:

``` python
from decouple import config, Csv
import dj_database_url

# Pega a string da chave secreta
SECRET_KEY = config('SECRET_KEY')

# Transforma uma string separada por vírgula em uma lista de strings Python
ALLOWED_HOSTS = config('ALLOWED_HOSTS', default=['localhost'], cast=Csv())

# Lê a URL do banco de dados e converte automaticamente para o formato do Django
DATABASES = {
    'default': config( 'DATABASE_URL',
    default='sqlite:///db.sqlite3',
    cast=dj_database_url.parse )
}
```


---

> [!WARNING] Nunca envie seu arquivo `.env` para o repositório
> Adicione o `.env` no `.gitignore` antes de fazer o commit após essas implementações.

