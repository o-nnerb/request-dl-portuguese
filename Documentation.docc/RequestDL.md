# ``RequestDL``

Uma camada de rede escrita em Swift baseada no paradigma de programação declarativa.

@Metadata {
    @PageImage(
        purpose: icon,
        source: "tucano",
        alt: "Um ícone de tecnologia representando o framework RequestDL.")

    @PageColor(blue)
}

## Visão Geral

O RequestDL tem como objetivo fornecer uma API abrangente para o desenvolvimento de aplicativos que consomem serviços de rede. Em sua essência, o RequestDL é construído sobre o **AsyncHTTPClient** da Apple, que, por sua vez, é baseado no **SwiftNIO**.

Com essas bases e aproveitando as técnicas modernas de networking, juntamente com a extensa integração fornecida pela Apple por meio desses dois módulos, o RequestDL pode realizar muito. Tudo isso é alcançado usando uma sintaxe simplificada e concisa.

### Por onde começar?

O RequestDL é dividido em duas partes principais: (a) ``RequestDL/Property``; e (b) ``RequestDL/RequestTask``.

#### O protocolo Property

Esta parte está relacionada à construção da requisição, fornecendo vários métodos e objetos para especificar a URL, o payload e os headers. A ``Property`` utiliza o tipo opaco `some` para permitir a implementação de blocos declarativos que são compilados com a ajuda do ``RequestDL/PropertyBuilder``, um `@resultBuilder`.

```swift
@PropertyBuilder
func appleWebsite() -> some Property {
    BaseURL("apple.com")

    RequestMethod(.post)
    AcceptHeader(.json)

    Payload(verbatim: "Hello Apple!")
}
```

#### O protocolo RequestTask

Este protocolo faz parte do processamento do resultado da requisição. Graças ao async/await, temos a possibilidade de receber os dados por meio de uma `AsyncSequence`, bem como os `Data` contendo os bytes brutos. Além disso, podemos usar o ``RequestDL/RequestTaskModifier`` e ``RequestDL/RequestTaskInterceptor`` para realizar vários tipos de operações. A requisição é finalizada usando o método ``RequestDL/RequestTask/result()``.

```swift
try await DataTask {
    BaseURL("apple.com")
    // Outras propriedades
}
.result()
```

### Recursos

- [x] [Construtor declarativo de requisições](<doc:Creating-requests-from-scratch>);
- [x] [mTLS / TLS / SSL / PSK](<doc:Secure-connection>);
- [x] [JSON / Codable / Multipart / URL Encoded](<doc:Exploring-payload>);
- [x] [UploadTask / DownloadTask / DataTask / MockedTask](<doc:Exploring-task>);
- [x] [Modificadores e Interceptadores](<doc:Modifiers-and-Interceptors>);
- [x] [Progresso de upload e download](<doc:Upload-and-download-progress>);
- [x] [Concorrência do Swift](<doc:Swift-concurrency>);
- [x] [Suporte ao Combine](<doc:Exploring-combine>);

Estamos empolgados em expandir esta lista com muitos outros recursos. Comece fazendo sua contribuição em [Discussões](https://github.com/orgs/request-dl/discussions) ou abrindo um PR (Pull Request).

## Topics

### Primeiros passos

- <doc:Creating-the-project-property>
- <doc:Preparing-the-certificates>

### Essenciais

- <doc:Building-the-request>
- <doc:Executing-the-request>
