# Explorando a diversidade de tarefas

Descubra as variações disponíveis para executar uma requisição de acordo com as necessidades específicas de cada endpoint.

## Visão geral

A construção de requisições no RequestDL foi moldada de acordo com os conceitos da Foundation. Combinando com a implementação de ``RequestDL/RequestTask`` e async/await, foi possível fornecer ``RequestDL/UploadTask``, ``RequestDL/DownloadTask`` e ``RequestDL/DataTask``.

Cada forma de criar uma requisição tem um propósito único, que está diretamente relacionado ao resultado que esses objetos retornam.

### UploadTask

`UploadTask` foi desenvolvido para permitir o uso de ``RequestDL/AsyncResponse`` e obter informações sobre cada byte enviado durante o processo de upload. Isso é vantajoso se você estiver considerando implementar uma barra de progresso que informa o usuário sobre o status do upload.

> Dica: Você tem controle detalhado sobre o upload com ``RequestDL/Property/payloadChunkSize(_:)``. Basta especificá-lo durante a especificação da requisição para obter o processo de upload com ``RequestDL/RequestTask/progress(upload:)`` da maneira que preferir.

Aqui está um exemplo sem abstrair a solução para que você possa aprender a maneira mais básica de usar ``RequestDL/UploadTask``:

```swift
let response = try await UploadTask {
    BaseURL("apple.com")
    // Outras especificações
    Payload(url: video, contentType: .mp4)
        .payloadChunkSize(8_192)
}
.result()

for try await step in response {
    switch step {
    case .upload(let step):
        print(step.chunkSize, step.totalSize)
    case .download(let step):
        // Lidar com o passo de download
    }
}
```

Saiba mais sobre o uso de [async/await](<doc:Swift-concurrency>) desde o início.

Como toda requisição sempre começa com o processo de upload, seguido pelo download, o uso de ``RequestDL/UploadTask`` dá acesso a todas as etapas de uma requisição.

### DownloadTask

``RequestDL/DownloadTask`` resulta em ``RequestDL/ResponseHead`` e ``RequestDL/AsyncBytes``, ignorando as informações de upload. Através desses objetos, já é possível obter todos os dados da requisição, seja bem-sucedida ou não, e também monitorar a transmissão de bytes para o servidor, graças ao `async/await`.

> Dica: Você pode controlar como os bytes são lidos pelo cliente através de ``RequestDL/ReadingMode``, que deve ser especificado durante a construção da requisição. Dessa forma, você pode acompanhar o progresso do download usando ``RequestDL/RequestTask/progress(download:)-20p6u``.

Aqui está um exemplo sem abstrações disponíveis para explorar o uso de ``RequestDL/DownloadTask``:

```swift
let downloadStep = try await DownloadTask {
    BaseURL("apple.com")
    // Outras especificações
    Payload(url: video, contentType: .mp4)
        .payloadChunkSize(8_192)
}
.result()

let asyncBytes = downloadStep.bytes

for try await bytes in asyncBytes {
    print(bytes.count, asyncBytes.totalSize)
}
```

Ao usar ``RequestDL/DownloadTask``, você precisa implementar uma maneira de lidar e combinar os bytes recebidos para obter os dados completos.

### DataTask

``RequestDL/DataTask`` é a forma padrão de fazer requisições no RequestDL. O resultado é um ``RequestDL/TaskResult`` encapsulando os `Data`. Se o endpoint que você está consumindo não tiver regras para enviar ou baixar informações, você pode usá-lo como a opção recomendada.

Aqui está o uso padrão:

```swift
let result = try await DataTask {
    // Especificações da propriedade
}
.result()

print(result.payload)
```

> Dica: Explore o uso de [modificadores e interceptadores](<doc:Modifiers-and-Interceptors>) para aprimorar suas requisições.

### GroupTask

``RequestDL/GroupTask`` é útil para agrupar várias chamadas simultâneas em uma única chamada. Para usá-lo, você precisa ter uma sequência que será convertida em um ``RequestDL/RequestTask``.

Em seguida, para cada item na sequência, você terá acesso ao resultado individual por meio de ``RequestDL/GroupTask/result()``, que é um dicionário em que as chaves são identificadas pelo elemento da sequência.

> Aviso: O elemento deve conformar-se ao protocolo `Hashable`.

## Tópicos

### O básico

- ``RequestDL/RequestTask``
- ``RequestDL/TaskResultPrimitive``
- ``RequestDL/TaskError``
- ``RequestDL/TaskResult``

### Conhecendo as tarefas

- ``RequestDL/UploadTask``
- ``RequestDL/DownloadTask``
- ``RequestDL/DataTask``
- ``RequestDL/RequestFailureError``

### Realizando múltiplas tarefas

- ``RequestDL/GroupTask``
- ``RequestDL/GroupResult``

### Descobrindo a resposta

- ``RequestDL/ResponseHead``
- ``RequestDL/ResponseHead/Status-swift.struct``
- ``RequestDL/ResponseHead/Version-swift.struct``
- ``RequestDL/StatusCode``
- ``RequestDL/StatusCodeSet``

### Recebendo os cabeçalhos

- ``RequestDL/HTTPHeaders``

### Modificando e interceptando as respostas

- <doc:Modifiers-and-Interceptors>

### Monitorando o progresso

- <doc:Upload-and-download-progress>

### Testando e depurando

- ``RequestDL/MockedTask``