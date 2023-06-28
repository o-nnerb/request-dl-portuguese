# Armazenando e lendo valores dentro de propriedades

Descubra como obter valores e utilizar classes dentro de uma propriedade durante a especificação de uma requisição.

## Visão geral

A integração de vários recursos importantes em uma requisição foi possibilitada pela implementação de alguns `@propertyWrapper` que estão diretamente vinculados a ``RequestDL/Property``.

### Environment

``RequestDL/PropertyEnvironment`` permite que você obtenha o valor de uma propriedade de ``RequestDL/PropertyEnvironmentValues`` durante a construção de uma propriedade.

Internamente, alguns objetos no RequestDL usam o ambiente para recuperar objetos importantes que influenciam o resultado da requisição. Portanto, você pode explorar esses recursos para criar suas próprias ferramentas alinhadas com os requisitos do seu sistema.

Você pode começar especificando um ``RequestDL/PropertyEnvironmentKey`` da seguinte maneira:

```swift
struct PayloadKey: PropertyEnvironmentKey {
    public static let defaultValue: Data?
}

extension PropertyEnvironmentValues {

    var payload: Data? {
        get { self[PayloadKey.self] }
        set { self[PayloadKey.self] = newValue }
    }
}

extension Property {

    func payload(_ data: Data) -> some Property {
        environment(\.payload, data)
    }
}
```

E então recuperar o valor usando ``RequestDL/PropertyEnvironment``:

```swift
struct GithubAPI: Property {

    @PropertyEnvironment(\.payload) var payload

    var body: some Property {
        // Outras especificações da propriedade
        if let payload {
            Payload(data: payload, contentType: customGithubJSONType)
        }
    }
}
```

### StoredObject

``RequestDL/StoredObject`` armazena o objeto instanciado na memória para auxiliar em várias otimizações.

Seu principal caso de uso está na implementação de ``RequestDL/SecureConnection``, que, em alguns casos, requer a codificação de um objeto a ser usado durante a verificação TLS.

Seu uso é simples e intuitivo:

```swift
struct GithubAPI: Property {

    @StoredObject var psk = GithubSSLPSKIdentity()

    var body: some Property {
        // Outras especificações da propriedade
        SecureConnection {
            PSKIdentity(psk)
        }
    }
}
```

> Note: Existe um tempo de vida que mantém a referência do objeto por uma determinada duração. Após a expiração, um novo objeto é usado.

### Namespace

``RequestDL/PropertyNamespace`` influencia diretamente a referência de memória em tempo de execução onde os objetos de estado de ``RequestDL/Property`` são armazenados.

Devido a uma série de otimizações relacionadas ao funcionamento do SwiftNIO e AsyncHTTPClient, definir um Namespace ajuda o RequestDL a determinar se precisa criar novos objetos a partir do zero ou usar aqueles que estão em cache na memória.

> Warning: O cache de memória aqui referido está relacionado a objetos Swift e não a cache de requisições.

Para cada `@PropertyNamespace` definido, o RequestDL combina os valores para formar um identificador de memória único. **É crucial usá-los ao trabalhar com ``RequestDL/StoredObject``**.

## Topics

### Conheça o ambiente

- ``RequestDL/PropertyEnvironmentKey``
- ``RequestDL/PropertyEnvironmentValues``
- ``RequestDL/PropertyEnvironment``
- ``RequestDL/Property/environment(_:_:)``

### Mantenha objetos na memória

- ``RequestDL/StoredObject``
- ``RequestDL/DynamicValue``

### Potencialize requisições com namespace

- ``RequestDL/PropertyNamespace``