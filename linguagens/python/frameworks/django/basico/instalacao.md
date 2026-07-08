---
title: Instalação
tipo: guia
linguagem: python
versao_linguagem: "3.12+"
tags:
  - guia
  - django-5-2
nivel: basico
status: pronto
atualizado: 2026-07-08
---
# Instalação

1. Instale o [pip](../../../basico/gerenciadores-de-pacotes/pip.md);

2. Crie um [ambiente virtual (venv)](../../../basico/ambiente-virtual.md). No _IntelliJ_ têm plugins que fazem isso mais automaticamente na criação do projeto, no _VSCode_ o processo tende a ser mais manual;

3. Depois de criar e ativar o ambiente virtual _venv_, usar o comando:

	``` bash
	python -m pip install Django ## instala a versão mais recente do Django.
	
	python -m pip install Django==5.2 # instala a versão 5.2 do Django.
	```
