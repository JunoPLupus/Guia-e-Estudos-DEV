---
title: UV
tipo: ferramenta
categoria: gerenciador-pacotes
linguagem: python
versao_linguagem: 3.10+
substitui:
  - pip
  - pipx
tags:
  - gerenciador-pacotes
status: pronto
atualizado: 2026-07-08
links:
  - https://docs.astral.sh/uv/getting-started/installation/
---
[< Gerenciadores de Pacotes Python](linguagens/python/basico/gerenciadores-de-pacotes/README.md)
# UV

## O que é

Substituto direto e extremamente rápido do `pip` (até 100x mais rápido), escrito em Rust. Gerencia dependências, versões do Python, projetos completos e pode substituir o `pipx`. Gera um arquivo `uv.lock` para travar as versões.

> [!note] Quando usar
> Para projetos novos ou quando a velocidade de instalação importa. É a escolha moderna — cobre o que pip, pipx e poetry fazem, de forma unificada.

## Instalação do gerenciador

A forma recomendada é via pipx:

```bash
pipx install uv
```

Outras formas disponíveis na [documentação oficial](https://docs.astral.sh/uv/getting-started/installation/).

## Instalar / adicionar pacotes

Dentro de um projeto uv:

```bash
uv add requests                          # adiciona ao projeto
uv add --group dev ruff                  # adiciona no grupo 'dev'
uv add --group prod gunicorn --no-sync   # adiciona sem instalar ainda
uv sync                                  # instala todas as dependências
```

Como substituto direto do pip (sem projeto):

```bash
uv pip install requests
uv pip install -r requirements.txt
```

## Atualizar pacotes

```bash
uv lock --upgrade-package requests  # atualiza um pacote específico
uv lock --upgrade                   # atualiza todos
uv sync                             # aplica o lock atualizado
```

## Remover pacotes

```bash
uv remove requests
```

## Listar / inspecionar

```bash
uv pip list
uv pip show requests
uv tree              # exibe árvore de dependências do projeto
```

## Criar projeto

```bash
uv init myapp    # cria novo projeto com estrutura básica
cd myapp

# OU: inicializa em pasta existente
cd meu-projeto
uv init
```

Estrutura gerada:

```
myapp/
    ├── main.py
    ├── pyproject.toml
    └── README.md
```

## Grupos de dependências

```bash
uv add django                             # dependência principal
uv add --group dev ruff                   # grupo dev
uv add --group prod gunicorn --no-sync    # grupo prod, pula instalação
```

Para rodar sem as deps de dev (ex: produção):

```bash
uv run --no-group dev --group prod python main.py
```

## Arquivo de lock

O `uv.lock` é gerado automaticamente pelo `uv sync` e `uv add`. Ele trava as versões exatas de todas as dependências, garantindo ambiente reproduzível. Deve ser comitado no repositório.

## Notas de versão

> [!warning] Grupos de dependências
> Suporte a múltiplos grupos foi adicionado no final de 2024. Versões anteriores só tinham grupos `dev` e `default`.

## Relacionados

- [ambiente virtual](../ambiente-virtual.md) — o uv cria/gerencia o venv automaticamente
- [pipx](pipx.md) — uv pode substituir com `uv tool install`
- [poetry](poetry.md) — alternativa com foco em empacotamento
