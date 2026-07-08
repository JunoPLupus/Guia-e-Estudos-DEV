---
title: pip
tipo: ferramenta
categoria: gerenciador-pacotes
linguagem: python
versao_linguagem: "3.0+"
tags:
  - gerenciador-pacotes
status: pronto
atualizado: 2026-07-08
links:
  - https://docs.python.org/3.13/installing/index.html
  - https://pypi.org/
---
# pip

## O que é

Gerenciador de pacotes padrão do Python. Instala pacotes do [Python Package Index (PyPI)](https://pypi.org/) no ambiente ativo. Já vem junto com o Python — não precisa instalar separadamente.

> [!note] Quando usar
> Para instalar bibliotecas dentro de um ambiente virtual. É o gerenciador básico — qualquer projeto Python usa pip em algum momento, mesmo que indiretamente.

## Instalação do gerenciador

Já vem com o Python. Para garantir que está atualizado:

```bash
python -m pip install --upgrade pip
```

## Instalar / adicionar pacotes

```bash
pip install requests              # versão mais recente
pip install requests==2.6.0       # versão específica
pip install novas requests django  # múltiplos pacotes de uma vez
pip install -r requirements.txt    # todos os pacotes de um arquivo
```

## Atualizar pacotes

```bash
pip install --upgrade requests
```

## Remover pacotes

```bash
pip uninstall requests
pip uninstall requests django  # múltiplos de uma vez
```

## Listar / inspecionar

```bash
pip list                    # lista todos os pacotes instalados
pip show requests           # info detalhada de um pacote específico
pip freeze                  # formato "pacote==versão", ideal pra requirements.txt
pip freeze > requirements.txt  # gera arquivo de dependências
```

## Relacionados

- Use sempre dentro de um [ambiente virtual](../ambiente-virtual.md)
- [pipx](pipx.md) para instalar ferramentas CLI globalmente
- [uv](uv.md) como substituto mais rápido do pip
