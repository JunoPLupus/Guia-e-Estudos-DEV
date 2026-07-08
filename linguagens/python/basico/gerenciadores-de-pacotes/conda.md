---
title: conda
tipo: ferramenta
categoria: gerenciador-pacotes
linguagem: python
tags:
  - gerenciador-pacotes
status: rascunho
atualizado: 2026-07-08
links:
  - https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html
---
# conda

## O que é

Gerenciador de pacotes e ambientes focado em Data Science e Machine Learning. Diferente do pip, não se limita a pacotes Python — gerencia dependências de sistema (C, C++) e o próprio interpretador Python. Excelente para instalar ferramentas como TensorFlow que têm dependências compiladas.

> [!note] Quando usar
> Em projetos de Ciência de Dados ou Machine Learning que exigem bibliotecas com componentes nativos (NumPy com BLAS, TensorFlow com CUDA, etc.). Para desenvolvimento web comum, pip/uv/poetry são mais adequados.

## Instaladores disponíveis

O conda tem três instaladores — escolha um:

**[Miniconda](https://www.anaconda.com/docs/getting-started/concepts/anaconda-or-miniconda)**
Instalador mínimo. Instala apenas o conda e o Python. Você adiciona o resto manualmente. Recomendado se quiser controle total sobre o que está instalado.

**[Anaconda Distribution](https://www.anaconda.com/download)**
Instalador completo. Vem com uma coleção de pacotes de Ciência de Dados pré-instalados e o Anaconda Navigator (GUI). Ocupa mais espaço em disco.

**[Miniforge](https://github.com/conda-forge/miniforge)**
Desenvolvido pela comunidade, pré-configurado para usar o canal `conda-forge` (mais atualizado e aberto que o canal padrão da Anaconda). Boa escolha para quem quer o ecossistema conda sem depender da Anaconda Inc.

Veja a comparação Miniconda vs Anaconda [aqui](https://www.anaconda.com/docs/getting-started/concepts/anaconda-or-miniconda).

## Relacionados

- [pip](pip.md)
- [uv](uv.md)
