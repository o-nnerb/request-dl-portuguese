# Usando Modificadores e Interceptadores

Descubra como implementar um modificador e um interceptador para lidar e tornar suas requisições auto-suficientes.

## Visão geral

Cada ponto de extremidade consultado está sujeito a diferentes regras de manipulação. Na maioria dos casos, o fluxo de receber a resposta e entregar os dados processados é o mesmo.

Para integrar a lógica de processamento da requisição diretamente na construção de uma ``RequestDL/RequestTask``, dois protocolos são fornecidos: ``RequestDL/RequestTaskModifier`` e ``RequestDL/RequestTaskInterceptor``.

### Modificador de tarefa

Existem infinitas possibilidades ao implementar um `Modificador`. É possível realizar operações tanto antes de fazer uma requisição quanto após receber a resposta.

Ao implementar seu `Modificador`, basta chamar o método ``RequestDL/RequestTask/modifier(_:)``, e sua requisição será manipulada por ele. Além disso, você pode especificar o `Input` e `Output` do `Modificador` para implementar lógica específica.

Algumas ideias interessantes para `Modificador` específico da API:

1. Atualização do token

    ```swift
    struct TokenRefreshModifier: RequestTaskModifier {

        typealias Input = TaskResult<Data>

        func body(_ task: Content) async throws -> Input {
            let result = try await task.result()

            guard result.head.status.code == 401 else {
                return result
            }

            // Se nenhum erro for lançado, então é seguro executar a requisição novamente
            try await refreshToken()
            return try await task.result()
        }
    }
    ```

2. Modificadores padrão do projeto

    ```swift
    struct DefaultsModifier<Object: Decodable>: RequestTaskModifier {

        typealias Input = TaskResult<Data>

        let objectType: Object.Type

        func body(_ task: Content) async throws -> Object {
            try await task
                .onStatusCode(.internalServerError) {
                    throw InternalServerError()
                }
                .refreshToken()
                .keyPath(\.result)
                .decode(objectType)
                .extractPayload()
                .result()
        }
    }
    ```

> Note: O RequestDL fornece uma variedade de modificadores implementados, que podem ser verificados por meio do enumerador ``RequestDL/Modifiers``.

### Interceptador de tarefa

O `Interceptador` tem como objetivo realizar uma operação independentemente do restante da requisição. Ele funciona como uma divergência de código que é sempre executada independentemente das condições.

Ao implementar seu `Interceptador`, você deve chamar o método ``RequestDL/RequestTask/interceptor(_:)`` para incorporá-lo em qualquer requisição. Cada `Interceptador` deve especificar o tipo do `Element`, que é o objeto de resultado da ``RequestTask`` original interceptada.

Aqui está um exemplo simples de como implementar um `Interceptador`:

```swift
struct AlwaysPrintInterceptor<Element>: RequestTaskInterceptor {

    func output(_ result: Result<Element, Error>) {
        switch result {
        case .success(let element):
            print("[Sucesso]", element)
        case .failure(let error):
            print("[Falha]", error)
        }
    }
}
```

> Note: O RequestDL fornece alguns interceptadores, que podem ser verificados por meio do enumerador ``RequestDL/Interceptors``.

## Tópicos

### Modificando a requisição

- ``RequestDL/RequestTaskModifier``
- ``RequestDL/ModifiedRequestTask``
- ``RequestDL/RequestTask/modifier(_:)``

### Interceptando a requisição

- ``RequestDL/RequestTaskInterceptor``
- ``RequestDL/InterceptedRequestTask``
- ``RequestDL/RequestTask/interceptor(_:)``

### Explorando os modificadores disponíveis

- ``RequestDL/Modifiers``

### Explorando os interceptadores disponíveis

- ``RequestDL/Interceptors``