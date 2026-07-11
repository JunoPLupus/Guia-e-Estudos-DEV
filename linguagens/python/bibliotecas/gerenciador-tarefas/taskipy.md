---
title: Taskipy
tipo: ferramenta
categoria: gerenciador-tarefas
linguagem: python
tags:
  - automacao
status: pronto
atualizado: 2026-07-10T08:48:00
links:
  - https://pypi.org/project/taskipy/1.0.0/
---
[< Bibliotecas Python](linguagens/python/bibliotecas/README.md)
# Taskipy

É uma biblioteca de código aberto que funciona como um _task runner_ (executador de tarefas). Ela permite que você crie atalhos e comandos personalizados para automatizar rotinas do seu projeto, como rodar testes, formatar código ou limpar o ambiente, diretamente pelo terminal.

Exemplo de configuração no `pyproject.toml`:

```toml
[tool.taskipy.tasks]  
pre_lint = 'typos'  
lint = 'ruff check'  
pre_format = 'ruff check --fix'  
format = 'ruff format'  
run = 'fastapi dev fastapi_zero/app.py'  
pre_test = 'task lint'  
test = 'pytest -s -x --cov=fastapi_zero -vv'  
post_test = 'coverage html --show-contexts'
```

Ao executar o comando `task test`, ele vai rodar o comando que está dentro de `pre_test`, seguido de `test` e por fim o comando do `post_test`.
