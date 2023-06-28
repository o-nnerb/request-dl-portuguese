# Criando requisições do zero

Descubra os vários tipos disponíveis que podem ser usados para especificar uma requisição.

## Visão geral

Ao usar o RequestDL, é crucial entender os conceitos fundamentais que definem os componentes de uma requisição HTTP, pois eles desempenham um papel crucial na forma como as requisições são feitas.

As requisições são construídas combinando diferentes componentes essenciais: URL, método, cabeçalhos e corpo. Cada um desses componentes é representado por um extenso conjunto de objetos que permitem configurar e personalizar todos os aspectos de uma requisição.

Essa abordagem flexível e modular do RequestDL fornece controle granular sobre as requisições e permite adaptá-las de acordo com as necessidades específicas de cada cenário.

### URL

A URL de uma requisição consiste em quatro partes principais: esquema, host, caminhos e parâmetros de consulta. Existem vários objetos disponíveis no RequestDL que facilitam o trabalho com cada um desses componentes.

- **[BaseURL](<doc:RequestDL/BaseURL>)**

    Define o esquema e o host da URL. Permite especificar se a requisição usará HTTP ou HTTPS como esquema, bem como o host de destino.

    ```swift
    BaseURL(.http, host: "apple.com")
    ```

- **[Path](<doc:RequestDL/Path>)**

    Adiciona caminhos à URL. Permite incluir segmentos de caminho específicos na URL, fornecendo uma estrutura hierárquica para localizar os recursos desejados.

    ```swift
    Path("api/v1")
    ```

- **[Query](<doc:RequestDL/Query>)**

    Adiciona parâmetros de consulta à URL. Esses parâmetros são usados para transmitir informações adicionais na requisição.

    ```swift
    Query(name: "create_at", value: Date())
    ```

> Tip: Explore o ``RequestDL/URLEncoder`` para usar outras formas de inserir parâmetros na URL.

### Método

As requisições HTTP têm uma série de métodos para realizar diferentes operações no mesmo ponto de extremidade. Os métodos mais comuns são GET, POST, PUT e DELETE, cada um com seu próprio significado.

- **[RequestMethod](<doc:RequestDL/RequestMethod>)**

    Especifica o método a ser usado na requisição. Os valores são predefinidos pelo objeto ``RequestDL/HTTPMethod``. O método padrão é sempre GET.

    ```swift
    RequestMethod(.post)
    ```

> Observação: Consulte os [métodos de requisição HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) para saber mais.

### Cabeçalhos

Os cabeçalhos de uma requisição HTTP desempenham um papel fundamental no estabelecimento de configurações e na transmissão de informações relevantes entre o cliente e o servidor.

Eles são responsáveis por definir várias características do objeto transmitido e indicar as expectativas do cliente em relação à resposta.

Além disso, os cabeçalhos podem incluir chaves de acesso a APIs, detalhes de autenticação, tipos de conteúdo aceitos ou esperados, informações de localização e muitos outros detalhes importantes.

- **[CustomHeader](<doc:RequestDL/CustomHeader>)**

    Especifica um cabeçalho com um nome e valor personalizados. Recomendado para uso quando não há um objeto de cabeçalho específico definido pelo RequestDL.

    ```swift
    CustomHeader(name: "X-api-token", value: "*****")
    ```

- **[AcceptHeader](<doc:RequestDL/AcceptHeader>)**

    Define o tipo de dados esperado como a resposta do cliente. Valores predefinidos em ``RequestDL/ContentType``.

    ```swift
    AcceptHeader(.json)
    ```

Saiba mais em [Conheça os cabeçalhos](#conheça-os-cabeçalhos).

### Corpo

A parte mais essencial e crítica de uma requisição HTTP é o corpo. É essencial para enviar vários tipos de dados para o servidor. É crítico, pois requer vários recursos de otimização, monitoramento, compressão, integridade e segurança.

- **[Payload](<doc:RequestDL/Payload>)**

    Define o corpo da requisição no formato de bytes brutos. A compreensão dos dados enviados é definida pelo ``RequestDL/ContentType`` com valores predefinidos.

    ```swift
    Payload(verbatim: "Olá Mundo!", contentType: .text)
    ```

- **[Form](<doc:RequestDL/Form>)**

    Define uma parte do corpo da requisição no formato **multipart/form-data**. Para definir o conjunto completo, confira o ``RequestDL/FormGroup``.

    ```swift
    Form(
        name: "hello_world",
        contentType: .text,
        verbatim: "Olá Mundo!"
    )
    ```

    > Warning: O parâmetro **name** é obrigatório, enquanto o **filename** é opcional.

Explore mais sobre o corpo da requisição em [Explorando o payload](<doc:Exploring-payload>).

### Crie o seu próprio

O `@resultBuilder` do Swift combinado com o tipo opaco torna mais flexível o desenvolvimento de soluções, especialmente respeitando as necessidades únicas de cada problema.

A programação declarativa tem uma série de vantagens e desvantagens, e cada solução deve explorar as ferramentas disponíveis.

Com ``RequestDL/Property`` e ``RequestDL/PropertyBuilder``, você pode definir uma propriedade única que configura uma série de comportamentos dentro da requisição, que podem ser reutilizados em todos os cenários.

```swift
struct AppleAPI: Property {

    var body: some Property {
        // Especificação do corpo
    }
}
```

Você pode explorar essa abordagem para criar soluções exclusivas para qualquer componente dentro da requisição, seja definindo a URL, cabeçalhos ou corpo, bem como outras propriedades relacionadas à configuração da requisição.

Saiba mais em:

- **<doc:Creating-the-project-property>**
- **<doc:Secure-connection>**
- **<doc:Cache-support>**

## Tópicos

### O cerne de todas as requisições

- ``RequestDL/Property``
- ``RequestDL/PropertyBuilder``

### O poder do construtor de resultados

- ``RequestDL/PropertyGroup``
- ``RequestDL/PropertyForEach``
- ``RequestDL/EmptyProperty``
- ``RequestDL/AnyProperty``
- ``RequestDL/AsyncProperty``

### Especificando a URL

- ``RequestDL/BaseURL``
- ``RequestDL/URLScheme``
- ``RequestDL/InternetProtocol``
- ``RequestDL/BaseURLError``
- ``RequestDL/Path``

### Adicionando parâmetros de consulta

- ``RequestDL/Query``
- ``RequestDL/QueryGroup``

### Definindo o método da requisição

- ``RequestDL/RequestMethod``
- ``RequestDL/HTTPMethod``

### Trabalhando com autenticação

- ``RequestDL/Authorization``

### Conheça os cabeçalhos

- ``RequestDL/CustomHeader``
- ``RequestDL/AcceptHeader``
- ``RequestDL/HostHeader``
- ``RequestDL/OriginHeader``
- ``RequestDL/RefererHeader``
- ``RequestDL/CacheHeader``
- ``RequestDL/AcceptCharsetHeader``
- ``RequestDL/UserAgentHeader``
- ``RequestDL/HeaderGroup``

### Alterando o comportamento dos cabeçalhos

- ``RequestDL/HeaderStrategy``
- ``RequestDL/Property/headerStrategy(_:)``
- ``RequestDL/Property/headerSeparator(_:)``

### Trabalhando com o corpo da requisição

- <doc:Exploring-payload>

### Realizando requisições seguras

- <doc:Secure-connection>

### Configuração da sessão

- ``RequestDL/Session``

### Adicionando tempo limite de requisição

- ``RequestDL/Timeout``
- ``RequestDL/UnitTime``

### Modificando as propriedades

- ``RequestDL/PropertyModifier``
- ``RequestDL/Property/modifier(_:)``
