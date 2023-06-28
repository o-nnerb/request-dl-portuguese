# Explorando o payload

O corpo da requisição suporta vários tipos de dados. Saiba mais sobre todos os tipos suportados.

## Visão geral

As possibilidades de enviar uma sequência de bytes em uma requisição são infinitas. Além disso, para que o servidor interprete os bytes recebidos, ele utiliza o valor do cabeçalho `Content-Type`, conforme é conhecido.

A abordagem mais comum é enviar uma sequência bruta de bytes relacionados a um tipo de dados conhecido. `application/json` é um exemplo frequentemente usado em aplicativos móveis.

No entanto, um segundo tipo de dados que aparece em alguns aplicativos é `multipart/form-data`, que possui uma implementação mais complexa devido ao seu padrão exclusivo de dados a serem enviados.

É por isso que o RequestDL fornece ``RequestDL/Payload`` e ``RequestDL/Form`` para maximizar suas chamadas.

> Tip: Cada objeto possui 5 inicializadores que padronizam a configuração dos dados em uma requisição: Data, String, URL, Codable e JSON (também conhecido como [String: Any]).

### Payload

Podemos considerar ``RequestDL/Payload`` como o tipo primitivo para os bytes a serem enviados em uma requisição. Tenha em mente as seguintes 3 propriedades centrais de cada payload:

1. Data

    As opções incluem `Data`, `String`, `URL`, `Codable` e `JSON`.

2. Content-Type

    Pode ser personalizado especificando um valor do tipo ``RequestDL/ContentType``. Aqui está uma lista de valores padrão:

    - Data: `application/octet-stream`
    - String: `text/plain`
    - URL: Não aplicável
    - Codable: `application/json`
    - JSON: `application/json`

3. Content-Length

    Configurado automaticamente com base no tamanho da sequência de bytes a ser enviada como corpo.

### Form

Usando o formato de dados `multipart/form-data` para ``RequestDL/Form``, existem várias implementações para simplificar o processo de construção do corpo da requisição.

> Important: Ao declarar um ``RequestDL/Form`` independente, ele se comporta como o único objeto para o corpo. Se você deseja especificar vários `form-data`, **deve** agrupá-los em um ``RequestDL/FormGroup``.

``RequestDL/Form`` segue os mesmos princípios de inicialização que ``RequestDL/Payload`` com uma diferença, que é a necessidade de especificar o parâmetro `name` e, opcionalmente, o `filename`. Esse requisito faz parte da definição padrão do `form-data`.

Além disso, um recurso poderoso que pode ser explorado de acordo com as necessidades de cada projeto e serviço é a capacidade de especificar cabeçalhos adicionais para um único `form-data` usando o parâmetro `headers:` durante a inicialização de ``RequestDL/Form``.

### URL Encoded

Ambos os objetos suportam ``RequestDL/URLEncoder``, que opera no corpo, aplicando um ``RequestDL/Charset`` específico e usando ``RequestDL/ContentType/formURLEncoded``.

> Note: O objeto ``RequestDL/Payload`` possui uma verificação interna. Se o tipo de conteúdo for `application/x-www-form-urlencoded` e o método de endpoint for ``RequestDL/HTTPMethod/get`` ou ``RequestDL/HTTPMethod/head``, o corpo é convertido em parâmetros de URL quando estiver no formato `Codable` ou `JSON`.

O controle refinado da codificação do objeto usando ``RequestDL/ContentType/formURLEncoded`` é feito da seguinte maneira:

```swift
Payload(userModel, contentType: .formURLEncoded)
    .charset(.utf8)
    .urlEncoder(.init())

// Resultado: name=John&age=20
```

## Topics

### Inserindo bytes no corpo da requisição

- ``RequestDL/Payload``
- ``RequestDL/EncodingPayloadError``

### Usando o formulário multipart

- ``RequestDL/Form``
- ``RequestDL/FormGroup``

### Definindo o tipo de conteúdo

- ``RequestDL/ContentType``

### Fazendo a codificação do corpo em formato URL

- ``RequestDL/URLEncoder``
- ``RequestDL/QueryItem``
- ``RequestDL/URLEncoderError``
- ``RequestDL/Property/urlEncoder(_:)``

### Especificando o conjunto de caracteres do conteúdo

- ``RequestDL/Charset``
- ``RequestDL/Property/charset(_:)``

### Configurando como os dados serão enviados

- ``RequestDL/Property/payloadChunkSize(_:)``
- ``RequestDL/Property/payloadPartLength(_:)``

### Configurando a estratégia de leitura

- ``RequestDL/ReadingMode``