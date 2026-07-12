---
title: Requisições HTTP
aliases:
  - Requisições HTTP
tipo: guia
tags:
  - guia
nivel: basico
status: pronto
atualizado: 2026-07-11 20:43
criado: 2026-07-11 19:01
links:
  - https://medium.com/@jonesroberto/9-m%C3%A9todos-de-requisi%C3%A7%C3%A3o-http-20159bd6ee6c
  - https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Methods
---
[< Web (Índice)](../web.md)
# Requisições HTTP

![](../../../_assets/2026-07-11_cliente-servidor-requisicao-resposta-http.png)

Uma requisição **HTTP** é a mensagem que um cliente (como um navegador ou aplicativo) envia a um servidor para solicitar dados ou executar uma ação. Ela é composta por um método (ex: _GET_, _POST_), o endereço do recurso (URL), cabeçalhos de controle e, opcionalmente, um corpo com dados.

Você pode definir quais dados quer receber e quais dados retornar em cada rota (_path_).

## Métodos HTTP

![](../../../_assets/2026-07-11_estrutura-basica-requisicao-http.png)

Os **métodos** de solicitação são diferenciados por meio de vários fatores, primeiro pelo _verbo_, mas também pelas seguintes características:

- Um **método** de solicitação será **_idempotente_** se puder ser processado com êxito várias vezes sem alterar o resultado. Para obter mais informações, confira [RFC 9110: 9.2.2. Métodos idempotentes](https://www.rfc-editor.org/rfc/rfc9110.html#name-idempotent-methods).
- Um **método** de solicitação pode ser **_armazenável em cache_** quando sua resposta correspondente pode ser armazenada para reutilização. Para obter mais informações, consulte [RFC 9110: Seção 9.2.3. Métodos e cache](https://www.rfc-editor.org/rfc/rfc9110.html#name-methods-and-caching).
- Um **método** de solicitação será considerado um **_método seguro_** se ele não modifica o estado de um recurso. Todos os _métodos seguros_ também são _idempotentes_, mas nem todos os **métodos** _idempotentes_ são considerados _seguros_. Para obter mais informações, confira [RFC 9110: Seção 9.2.1. Métodos seguros](https://www.rfc-editor.org/rfc/rfc9110.html#name-safe-methods).

O protocolo **HTTP** funciona através de um sistema de "pedido e resposta" e define verbos específicos para cada tipo de ação que você deseja realizar. Os mais comuns são:

### GET

![260](../../../_assets/2026-07-11_metodo-http-get.png)

O objetivo do método **GET** é simplesmente recuperar dados do servidor. O método **GET** é usado para solicitar qualquer um dos seguintes recursos:

- Uma página da web ou arquivo _HTML_.
- Uma lista de dados filtrados (`GET /artigos?categoria=tech`)
- Uma imagem ou vídeo.
- Um documento _JSON_.
- Um arquivo CSS ou arquivo _JavaScript_.
- Um arquivo _XML_.

O método de solicitação **GET** é considerado uma operação segura, o que significa que não deve alterar o estado de nenhum recurso no servidor.

#### Exemplo de Requisição (_Request_)

```http
GET /api/usuarios/123 HTTP/1.1
```

#### Exemplo de Resposta (_Response_)

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 78

body : {
		  "id": 123,
		  "nome": "João Silva",
		  "email": "joao@email.com",
		  "ativo": true
		}
```

`Content-Type` e `Content-Lenght` são apenas dois dos campos do [Header](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Headers), o _header_ pode possuir diversos campos (muitos são definidos automaticamente, mas você pode atribuir os valores no _header_ da requisição, como é o caso do `Authorization` que pode ser usado para verificar se o usuário está autenticado e se possui permissão para acessar aquele recurso), os exemplos estão propositalmente simplificados.


---
### POST

![259](../../../_assets/2026-07-11_metodo-http-post.png)

O método de solicitação **POST** envia dados ao servidor para processamento.

Os dados enviados ao servidor normalmente estão no seguinte formato:

- Campos de entrada de formulários online.
- Dados _XML_ ou _JSON_.
- Dados de texto de parâmetros de consulta.

A especificação **HTTP** permite ao desenvolvedor decidir o tipo de processamento dos dados enviados através de um método **POST**. Os usos prototípicos do método **POST** incluem o seguinte:

- Salve dados de formulários _HTML_ em um banco de dados
- Upload de arquivos
- Acionar ações ( `POST /payments`)
- Envio de grandes quantidades de dados
- Calcule um resultado com base nos dados enviados
- Qualquer operação que altere o estado do servidor

Uma operação **POST** não é considerada uma operação segura, pois tem o poder de atualizar o estado do servidor e causar possíveis efeitos colaterais no estado do servidor quando executada

O método **POST** também não precisa ser idempotente, o que significa que ele pode deixar dados e recursos no servidor em um estado diferente cada vez que é invocado.

#### Exemplo de Requisição (_Request_)

```http
POST /api/usuarios HTTP/1.1
Content-Type: application/json

body : {
		  "nome": "João Silva",
		  "email": "joao@email.com"
		}
```

#### Exemplo de Resposta (_Response_)

```http
HTTP/1.1 201 Created
Location: /api/users/123
Content-Type: application/json

body : {
		  "id": 123,
		  "nome": "João Silva",
		  "email": "joao@email.com",
		  "ativo": true
		}
```


---
### PUT

![263](../../../_assets/2026-07-11_metodo-http-put.png)

O método **PUT** é usado para substituir completamente um recurso identificado por uma determinada URL.

O método de solicitação **PUT** inclui duas regras:

1. Uma operação **PUT** sempre inclui uma carga útil que descreve uma definição de recurso completamente nova a ser salva pelo servidor.
2. A operação **PUT** usa a URL exata do recurso de destino.

Se existir um recurso na URL fornecida por uma operação **PUT**, a representação do recurso será completamente substituída. Se não existir um recurso nesse URL, um novo recurso será criado.

A carga útil de uma operação **PUT** pode ser qualquer coisa que o servidor entenda, embora _JSON_ e _XML_ sejam os formatos de troca de dados mais comuns para webservices e microsserviços _RESTful_.

#### Exemplo de Requisição (_Request_)

```http
PUT /api/usuarios/123 HTTP/1.1
Content-Type: application/json

body : {
		  "nome": "João Silva",
		  "email": "joao@email.com"
		}
```

#### Exemplo de Resposta (_Response_)

Se o recurso de destino não tem uma representação atual e a requisição **PUT** foi criada com sucesso, então o servidor original deve informar o agente de usuário enviando uma resposta [`201`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/201) (`Created`).

```http
HTTP/1.1 201 Created
Location: /api/users/123
```

Se o recurso de destino tem uma representação atual e essa representação é modificada com sucesso de acordo com o estado de representação em anexo, então o servidor original deve enviar também uma resposta [`200`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/200) (`OK`) or a [`204`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/204) (`No Content`) para indicar a conclusão da requisição.

```http
HTTP/1.1 204 No Content
Location: /api/users/123
```


---
### PATCH

![268](../../../_assets/2026-07-11_metodo-http-patch.png)

Às vezes, as representações de objetos ficam muito grandes. A exigência de que uma operação _PUT_ sempre envie uma representação completa de recursos ao servidor é um desperdício se apenas uma pequena alteração for necessária em um recurso grande.

O método **PATH**, adicionado ao Protocolo de Transferência de Hipertexto (_HTTP_) de forma independente como parte da [RFC 5789](https://www.rfc-editor.org/rfc/rfc5789) , permite atualizações de recursos existentes. É significativamente mais eficiente, por exemplo, enviar uma pequena carga útil em vez de uma representação completa de recursos para o servidor.

#### Exemplo de Requisição (_Request_)

```http
PATCH /api/usuarios/123 HTTP/1.1
Content-Type: application/json

body : {
		  "nome": "João Silva"
		}
```

#### Exemplo de Resposta (_Response_)

Uma resposta sucedida é indicada pelo _status_ de resposta [`204`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/204), visto que a resposta não carrega um corpo de mensagem.

```http
HTTP/1.1 204 No Content
Location: /api/users/123
```


---
### DELETE

![247](../../../_assets/2026-07-11_metodo-http-delete.png)

O método **DELETE** é autoexplicativo. Após a execução, o recurso para o qual uma operação *DELETE* aponta é removido do servidor. Tal como acontece com as operações _PUT_, o método **DELETE** é idempotente e inseguro.

#### Exemplo de Requisição (_Request_)

```http
DELETE /api/usuarios/123 HTTP/1.1
Authorization: Bearer seu_token_de_autenticacao # caso sua rota exija estar autenticado para usar este método nesta rota
```

#### Exemplo de Resposta (_Response_)

Se um método **DELETE** for aplicado com sucesso, há muitos códigos de status de resposta possíveis:

- Um código de status [`202`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/202) (`Accepted`) se a ação provavelmente teve sucesso, porém ainda não foi realizada.

- Um código de status [`204`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/204) (`No Content`) se a ação foi realizada e nenhuma outra informação deve ser fornecida.

- Um código de status [`200`](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Reference/Status/200) (`OK`) se a ação foi realizada e a mensagem de resposta inclui uma representação descrevendo o status.

```http
HTTP/1.1 204 No Content
Date: Sat, 11 Jul 2026 23:11:00 GMT 
Server: Apache
```
