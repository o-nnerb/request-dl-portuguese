# Usando Swift Concurrency desde o início

Descubra todos os recursos disponíveis para usar async/await desde o início.

## Visão geral

Construído desde o início com async/await em mente, o RequestDL sempre usa operações assíncronas internamente, travas de thread e Sendable.

O SwiftNIO e o AsyncHTTPClient também enviam e recebem constantemente dados de requisição em pacotes, incluindo cabeçalhos e corpo.

Ao combinar os conceitos de async/await com as ferramentas fornecidas pelo SwiftNIO e AsyncHTTPClient, foi implementado o conceito de etapas representadas por ``RequestDL/UploadStep``, ``RequestDL/DownloadStep`` e ``RequestDL/ResponseStep``.

### Construindo a requisição

Embora o foco esteja no uso de async/await na execução da requisição, também é possível usar código assíncrono durante a especificação da requisição.

Projetado para casos em que temos um serviço que é usado apenas para especificar um campo da requisição, surgiu a necessidade de um objeto que forneça esse suporte.

```swift
struct GithubAPI: Property {

    var body: some Property {
        // Propriedades comuns da API do Github
        AsyncProperty {
            Authorization(.bearer, token: try await tokenService.getToken())
        }
    }
}
```

Este código nos permite não apenas usar `await` durante a especificação da requisição, mas também `try`.

### Executando a requisição

``RequestTask`` possui certa flexibilidade em relação ao `Element` retornado na execução do método ``RequestTask/result()``. Isso possibilitou a implementação de ``RequestDL/UploadTask``, ``RequestDL/DownloadTask`` e ``RequestDL/DataTask``.

> Tip: Saiba mais sobre as tarefas em [Explorando a diversidade de tarefas](<doc:Exploring-task>).

``RequestDL/AsyncResponse`` é o objeto central da requisição. Com ele, obtemos as etapas para saber a quantidade de bytes enviados e os bytes que estamos recebendo.

``RequestDL/AsyncBytes`` nos informa a quantidade de bytes recebidos de forma assíncrona e também nos permite saber a quantidade que iremos receber através da propriedade ``RequestDL/AsyncBytes/totalSize``.

> Important: Ambos os objetos são uma `AsyncSequence` e devem ser usados em um loop `for try await _`.

### Pontos de atenção

Os objetos ``RequestDL/AsyncResponse`` e ``RequestDL/AsyncBytes`` funcionam como um fluxo aberto que recebe dados independentemente de quem está ouvindo.

Além disso, cada elemento recebido é inserido em uma fila que permanece disponível até que o objeto assíncrono deixe de existir. Portanto, é possível ter vários loops `for try await _` no código para observar as sequências quantas vezes forem necessárias.

> Warning: Uma vez que os objetos ``RequestDL/AsyncResponse`` e ``RequestDL/AsyncBytes`` deixam de existir durante uma requisição, isso pode resultar no cancelamento da operação.

## Tópicos

### Conhecendo as etapas

- ``RequestDL/UploadStep``
- ``RequestDL/DownloadStep``
- ``RequestDL/ResponseStep``

### Descobrindo as sequências

- ``RequestDL/AsyncResponse``
- ``RequestDL/AsyncBytes``