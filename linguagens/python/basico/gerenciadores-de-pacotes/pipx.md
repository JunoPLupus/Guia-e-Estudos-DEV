---
title: Pipx
tipo: ferramenta
categoria: gerenciador-pacotes
linguagem: python
versao_linguagem: 3.10+
tags:
  - gerenciador-pacotes
status: pronto
atualizado: 2026-07-08
links:
  - https://pipx.pypa.io/stable/how-to/install-pipx/
---
# Pipx

## O que é

Instala e executa **ferramentas CLI Python globalmente**, cada uma isolada em seu próprio ambiente virtual. Diferente do `pip` (que instala no ambiente ativo), o pipx é para aplicações executáveis como `poetry`, `ruff`, `black`, `ansible`.

> [!note] Quando usar
> Quando você quer uma ferramenta disponível em qualquer projeto, sem poluir o ambiente global ou criar conflitos de dependências entre ferramentas.

## Instalação do gerenciador

```bash
pip install pipx
pipx ensurepath  # adiciona o diretório do pipx ao $PATH do sistema
```

> Após `ensurepath`, reinicie o terminal para o PATH ser reconhecido.

## Instalar / adicionar pacotes

```bash
pipx install poetry           # versão mais recente
pipx install poetry==1.8.4    # versão específica
```

## Atualizar pacotes

```bash
pipx upgrade poetry           # atualiza uma ferramenta específica
pipx upgrade-all              # atualiza todas
```

## Remover pacotes

```bash
pipx uninstall poetry
```

## Listar / inspecionar

```bash
pipx list                     # lista todas as ferramentas instaladas via pipx
```

## Relacionados

- [pip](pip.md) para instalar bibliotecas dentro de um projeto
- [uv](uv.md) pode substituir o pipx também (`uv tool install`)
