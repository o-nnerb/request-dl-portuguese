# Criando a propriedade do projeto

Comece criando a propriedade compartilhada do projeto para todas as requisições.

## Visão Geral

Para começar a integrar o RequestDL em seu projeto, recomendamos criar um objeto que configure as propriedades compartilhadas para todas as requisições a um host.

Por exemplo, vamos construir um aplicativo que consome a API do Github. Na [documentação](https://docs.github.com/en/rest), especifica-se que o cabeçalho `Accept` deve sempre ser definido como `application/vnd.github+json`, e o cabeçalho `X-GitHub-Api-Version` deve ser definido como `2022-11-28`, que é a versão mais recente da API. Além disso, todas as chamadas devem ser feitas para `https://api.github.com`.

### A propriedade do projeto

Para implementar essas especificações para a API do Github, precisamos criar o seguinte objeto:

```swift
import RequestDL

struct GithubAPI: Property {

    var body: some Property {
        BaseURL("api.github.com")

        AcceptHeader("application/vnd.github+json")
        CustomHeader(name: "X-GitHub-Api-Version", value: "2022-11-28")
    }
}
```

#### Autenticação

A documentação do Github fornece vários métodos de autenticação, que não são abordados neste exemplo. Para fins educacionais, vamos nos concentrar em consumir um endpoint do Github que requer o cabeçalho `Authorization: Bearer ***`.

Para realizar isso, podemos aproveitar o objeto `GithubAPI` com a seguinte implementação:

- Se você deseja fornecer o token externamente:

    ```swift
    import RequestDL

    struct GithubAPI: Property {

        let token: String?

        var body: some Property {
            BaseURL("api.github.com")

            AcceptHeader("application/vnd.github+json")
            CustomHeader(name: "X-GitHub-Api-Version", value: "2022-11-28")

            if let token {
                Authorization(.bearer, token: token)
            }
        }
    }
    ```

- Se você deseja consumir um serviço:

    ```swift
    import RequestDL

    struct GithubAPI: Property {

        var body: some Property {
            BaseURL("api.github.com")

            AcceptHeader("application/vnd.github+json")
            CustomHeader(name: "X-GitHub-Api-Version", value: "2022-11-28")

            if let token = GithubService.shared.token {
                Authorization(.bearer, token: token)
            }
        }
    }
    ```

É claro que existem várias opções para alcançar isso, e aqui estamos explorando apenas algumas abordagens válidas.

#### ContentType

Outro aspecto comumente usado em requisições é ``RequestDL/ContentType``. O caso do Github é um excelente exemplo, pois requer um valor personalizado que difere do valor padrão existente na biblioteca, `application/json`.

Para realizar isso, você precisa configurar um arquivo separado para estender ``RequestDL/ContentType`` da seguinte forma:

```swift
import RequestDL

extension ContentType {

    static let githubJSON = ContentType("application/vnd.github+json")
}
```

Dessa forma, você pode usar esse tipo de conteúdo sempre que necessário em seu código.

## Próximos passos

Agora que você fez essas configurações iniciais, é claro que existem outros aspectos a serem explorados e utilizados de acordo com as necessidades específicas de cada aplicativo. No entanto, este é o exemplo mais básico para começar, e agora você está pronto para avançar para os próximos passos.