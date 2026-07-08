---
title: Comandos
tipo: ferramenta
linguagem: python
tags:
  - django-5-2
status: pronto
atualizado: 2026-07-08
links:
  - https://docs.djangoproject.com/pt-br/5.2/ref/django-admin/
---

# Comandos

## django-admin e manage.py

`django-admin` é um comando Django usado para tarefas administrativas.

`manage.py` é criado automaticamente em cada projeto Django. Este comando faz o mesmo que `django-admin`, mas também define a variável de ambiente `DJANGO_SETTINGS_MODULE` que aponta para o arquivo `settings.py` do seu projeto.

Caso esteja trabalhando apenas com um projeto Django, é mais fácil usar `manage.py`. Mas caso esteja trabalhando com múltiplos projetos e precise trocar entre múltiplos arquivos `settings`, é recomendado usar `django-admin` com `DJANGO_SETTINGS_MODULE` ou com `--settings` na linha de comando.

### Uso

``` bash
django-admin <comando> [opções]

manage.py <comando> [opções]

python -m django <comando> [opções]
```

Os comandos são obrigatórios e `opções`, como o próprio nome diz, são opcionais.


### Ajuda

``` bash
django-admin help # lista informações de uso e uma lista de comandos de cada aplicação.

django-admin help --comands # exibe uma lista de todos os comandos disponíveis.

django-admin help <comando> # exibe a descrição de um comando específico e opções disponíveis.
```
