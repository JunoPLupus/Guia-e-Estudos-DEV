---
title: Arquivos indesejados
tipo: problema
contexto: git
tags:
  - problema
status: pronto
atualizado: 2026-07-11 20:48
---
[< Resolução de Problemas (Índice)](./README.md)
# Arquivos indesejados

## Arquivos indesejados no repositório

Para remover arquivos que já estão sendo rastreados nos commits ou que já foram subidos no repositório remoto, usar o comando:

``` bash
git rm -r --cached .idea
```

## Arquivo `.gitignore` sendo ignorado

Uma das causas pode ser o formato que o arquivo foi escrito, para ser lido corretamente, ele deve estar no formato _UTF-8_. Se não estiver, é necessário reescrever o arquivo manualmente ou através de comando. Ás vezes é possível modificar através da opção de codificação na própria IDE como no _intelliJ_ (no canto inferior direito), mas às vezes a _IDE_ pode bloquear essa operação.

Através do terminal:

``` bash
# Substitui o formato do .gitignore de ISO-8859-1 para UTF-8, caso seja diferente.
iconv -f ISO-8859-1 -t UTF-8 .gitignore > .gitignore_temp && mv .gitignore_temp .gitignore
```

Através de código _python_:

``` python
# Lê o conteúdo com a codificação atual (ex: 'cp1252' ou 'UTF-16LE')
# e grava novamente em 'UTF-8'.

with open('.gitignore', 'r', encoding='cp1252') as f:
    conteudo = f.read()

with open('.gitignore', 'w', encoding='utf-8') as f:
    f.write(conteudo)
```
