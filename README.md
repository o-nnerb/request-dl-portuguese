[![Swift Compatibility](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2Frequest-dl%2Frequest-dl%2Fbadge%3Ftype%3Dswift-versions)](https://swiftpackageindex.com/request-dl/request-dl)
[![Platform Compatibility](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2Frequest-dl%2Frequest-dl%2Fbadge%3Ftype%3Dplatforms)](https://swiftpackageindex.com/request-dl/request-dl)
[![codecov](https://codecov.io/gh/request-dl/request-dl/branch/main/graph/badge.svg?token=MW5J053T85)](https://codecov.io/gh/request-dl/request-dl)

# [RequestDL](https://github.com/request-dl/request-dl)

> Graças ao **Mike Lewis**, o progresso foi integrado perfeitamente nesta biblioteca, proporcionando uma experiência simples e simplificada.

RequestDL é um pacote Swift projetado para simplificar o processo de realização de requisições de rede. Ele fornece um conjunto de ferramentas, incluindo o protocolo `RequestTask`, que suporta diferentes tipos de requisições, incluindo `DataTask`, `DownloadTask` e `UploadTask`.

Uma das principais características do RequestDL é o suporte à especificação de propriedades de uma requisição, como `Query`, `Payload` e `Headers`, entre outros. Você também pode usar `RequestTaskModifier` e `RequestTaskInterceptor` para processar a resposta após a conclusão da requisição, permitindo ações como decodificação, mapeamento, tratamento de erros com base em códigos de status e registro de respostas no console.

O protocolo `Property` é outra poderosa funcionalidade que permite que os desenvolvedores implementem propriedades personalizadas para definir vários aspectos da requisição dentro de uma especificação de struct ou usando o `@PropertyBuilder`. Isso facilita a personalização das requisições para atender às necessidades específicas.

## [Documentação](https://brennobemoura.github.io/request-dl-portuguese/documentation/requestdl/)

Confira nossa documentação abrangente para obter todas as informações necessárias para começar a usar o RequestDL em seu projeto.

### Primeiros passos

- [Criando a propriedade do projeto](https://brennobemoura.github.io/request-dl-portuguese/documentation/requestdl//creating-the-project-property)
- [Preparando os certificados](https://brennobemoura.github.io/request-dl-portuguese/documentation/requestdl//preparing-the-certificates)

### Conceitos essenciais

- [Construindo a requisição](https://brennobemoura.github.io/request-dl-portuguese/documentation/requestdl//building-the-request)
- [Executando a requisição](https://brennobemoura.github.io/request-dl-portuguese/documentation/requestdl//executing-the-request)

### Traduções

- [Português](https://brennobemoura.github.io/request-dl-portuguese/documentation/requestdl)

Ficaríamos encantados em contar com sua ajuda na tradução de nossa documentação para o seu idioma preferido! Basta abrir um pull request (PR) em nosso repositório com o link para a versão traduzida. Estamos ansiosos para receber sua contribuição!

## Instalação

O RequestDL pode ser instalado usando o Swift Package Manager. Para incluí-lo em seu projeto, adicione a seguinte dependência ao seu arquivo Package.swift:

```swift
dependencies: [
    .package(url: "https://github.com/request-dl/request-dl.git", from: "3.0.0")
]
```

## Uso

Usar o RequestDL é fácil e intuitivo. Você pode criar requisições de rede de maneira declarativa, especificando as propriedades da requisição por meio do uso do protocolo `Property` ou usando o atributo `@PropertyBuilder`.

Aqui está um exemplo de um simples `DataTask` que faz uma consulta ao Google para o termo "apple", registra a resposta no console e ignora o cabeçalho de resposta HTTP:

```swift
try await DataTask {
    BaseURL("google.com")

    HeaderGroup {
        AcceptHeader(.json)
        CustomHeader(name: "xxx-api-key", value: token)
    }

    Query(name: "q", value: "apple")
}
.logInConsole(true)
.decode(GoogleResponse.self)
.extractPayload()
.result()
```

Este código cria um `DataTask` com o `BaseURL` definido como "google.com", um `HeaderGroup` contendo o "Accept" definido como "application/json", um cabeçalho "xxx-api-key" definido com a chave da API e um parâmetro de consulta com a chave "q" e o valor "apple". Em seguida, define a propriedade `logInConsole` como true, que imprimirá a resposta no console quando a requisição for concluída. Ele também decodifica a resposta em uma instância de `GoogleResponse`.

Este é apenas um exemplo simples do que o RequestDL pode fazer. Consulte a documentação para obter mais informações sobre como usá-lo.

## Versionamento

Seguimos o versionamento semântico para este projeto. O número da versão é composto por três partes: MAJOR.MINOR.PATCH.

- Versão MAJOR: Aumenta quando há alterações incompatíveis e alterações que quebram a compatibilidade. Essas alterações podem exigir atualizações no código existente e podem quebrar a compatibilidade retroativa.

- Versão MINOR: Aumenta quando novos recursos ou aprimoramentos são adicionados de maneira compatível com versões anteriores. Pode incluir melhorias, adições ou modificações na funcionalidade existente.

- A versão PATCH inclui correções de bugs, patches e modificações seguras que corrigem problemas, bugs ou vulnerabilidades sem interromper a funcionalidade existente. Também pode incluir novos recursos, mas eles devem ser implementados com cuidado para evitar alterações que quebrem a compatibilidade ou problemas de compatibilidade.

É recomendado revisar as notas de lançamento de cada versão para entender as alterações e atualizações específicas feitas naquela versão específica.

## Contribuindo

Se você encontrar um bug ou tiver uma ideia para um novo recurso, abra uma issue ou envie um pull request. Aceitamos contribuições da comunidade!

## Agradecimentos

Esta biblioteca deve muito ao trabalho de Carson Katri e seu pacote Swift [Request](https://github.com/carson-katri/swift-request). Muitos dos conceitos e técnicas principais usados no RequestDL foram inspirados pela biblioteca de Carson, e a implementação original do RequestDL até usou um fork da biblioteca de Carson como base.

Sem o trabalho de Carson, esta biblioteca não existiria em sua forma atual. Obrigado, Carson, por suas contribuições à comunidade Swift e por inspirar o desenvolvimento do RequestDL.
