---
title: poetry
tipo: ferramenta
categoria: gerenciador-pacotes
linguagem: python
versao_linguagem: "3.10+"
tags:
  - gerenciador-pacotes
status: pronto
atualizado: 2026-07-08
links:
  - https://python-poetry.org/docs/
---

# poetry

## O que é

Padrão moderno para gerenciamento e **empacotamento** de projetos Python. Usa `pyproject.toml` para centralizar configurações e `poetry.lock` para travar dependências.

> [!note] Quando usar
> Quando o projeto precisa ser empacotado e publicado (PyPI), ou quando você quer um gerenciador maduro com suporte a múltiplos grupos de dependências. Para projetos simples, `uv` tende a ser mais rápido.

## Instalação do gerenciador

Recomendado via pipx para manter isolado:

```bash
pipx install poetry
```

> Evite instalar via `pip install poetry` diretamente — o Poetry tende a quebrar ao atualizar o Python. O pipx mantém cada ferramenta em seu próprio ambiente.

## Instalar / adicionar pacotes

```bash
poetry add requests                    # dependência principal
poetry add ruff --group dev            # grupo dev
poetry install                         # instala todas as deps do pyproject.toml
```

## Atualizar pacotes

```bash
poetry show --outdated --top-level --with dev --with prod  # lista desatualizados
poetry update                         # atualiza tudo
poetry update django                  # atualiza um pacote específico
poetry update ruff --with dev         # atualiza incluindo grupo dev
```

## Remover pacotes

```bash
poetry remove requests
poetry remove ruff --group dev
```

## Listar / inspecionar

```bash
poetry show                    # lista dependências instaladas
poetry show --outdated         # lista desatualizadas
poetry show requests           # info de um pacote específico
```

## Criar projeto

```bash
poetry new myapp    # cria novo projeto
cd myapp

# OU: inicializa em projeto existente
cd meu-projeto
poetry init
```

Estrutura gerada pelo `poetry new`:

```
myapp/
    ├── myapp/
    │   └── __init__.py
    ├── pyproject.toml
    ├── README.md
    └── tests/
        └── __init__.py
```

## Grupos de dependências

```bash
poetry add django                       # produção
poetry add ruff --group dev             # desenvolvimento
poetry add pytest --group test          # testes
```

Para instalar incluindo grupos específicos:

```bash
poetry install --with dev
poetry install --with dev --with test
```

O Poetry sempre verifica se está dentro de um [ambiente virtual](../ambiente-virtual.md) antes de executar — se não estiver, cria um automaticamente.

## Arquivo de lock

O `poetry.lock` é gerado pelo `poetry install` e `poetry add`. Trava versões exatas de todas as dependências. Deve ser comitado no repositório.

## Relacionados

- [ambiente virtual](../ambiente-virtual.md)
- [pipx](pipx.md) — forma recomendada de instalar o poetry
- [uv](uv.md) — alternativa mais rápida, pode substituir poetry em muitos casos
