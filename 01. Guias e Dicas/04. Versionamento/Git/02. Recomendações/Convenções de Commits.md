# Convenções de mensagens de _commits_


## Tipos de prefixos no resumo do _commit_

``` bash
fix:  # commit que corrige algum bug

feat: # commit que introduzir uma nova feature no código

chore: # commit que mexe com tarefas rotineiras de manutenção, infraestrutura ou ferramentas do projeto (ex.: adicionar ou atualizar bibliotecas no package.json).

docs: # commit que adiciona ou modifica algo na documentação do projeto.

style: # commit que implementa ou altera algo visualmente (ex.: Ajustes visuais na tela de login).

refactor: # commit que refatora algo no código, mas sem mudar a lógica principal (ex.: em um trecho do código que percorre uma lista usando 'for', você decide usar 'stream()').

test: # commit que implementa ou modifica testes (unitários, de integração ou qualquer outro tipo). 
```

> Obs.: Têm alguns outros tipos que não foram listados aqui, ver link abaixo para mais informações.

Caso necessário, é possível também especificar o escopo do _commit_, como por exemplo:

``` bash
feat(api): Implementa coisa x

feat(lang): Adiciona idioma espanhol
```

---
## _Outras convenções_

Mensagem com `!` no resumo para chamar a atenção para um possível _breaking change_, exemplo:

``` bash
feat!: Implementa tal coisa que pode quebrar a aplicação
```

_Footer_ opcional na descrição do _commit_:

``` bash
BREAKING CHANGE: <descricao> # na última linha da descrição do commit, serve para avisar que uma alteração no código vai quebrar o funcionamento de partes dependentes, como APIs ou bibliotecas. Ela automatiza o versionamento semântico (mudando a versão MAJOR) e gera avisos claros para os usuários.
```

## Referências

- ﻿[![icon](https://www.google.com/s2/favicons?domain=conventionalcommits.org&sz=64)ConventionalcommitsConventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)