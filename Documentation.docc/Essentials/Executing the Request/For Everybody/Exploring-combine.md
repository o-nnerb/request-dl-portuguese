# Explorando o suporte ao Combine

Explore como usar o Combine com tarefas de maneira fácil e eficiente.

## Visão geral

O RequestDL fornece suporte para usar o Combine diretamente na execução de requisições. Você pode combinar uma série de `Modificadores` e `Interceptadores` para finalizar a construção da requisição com ``RequestDL/RequestTask/publisher()``.

A única diferença entre uma requisição async/await e uma que usa o Combine é o método usado para finalizar a construção da requisição. Isso é possível graças à integração completa dos recursos básicos implementados para ``RequestDL/RequestTask``.

```swift
func detalhesDoUsuario() -> PublishedTask<User> {
    DataTask {
        // As especificações da requisição
    }
    .logInConsole(true)
    .extractPayload()
    .keyPath(\.results)
    .decode(User.self)
    .publisher()
}
```

## Tópicos

### Conhecendo o modificador

- ``RequestDL/PublishedTask``
- ``RequestDL/RequestTask/publisher()``