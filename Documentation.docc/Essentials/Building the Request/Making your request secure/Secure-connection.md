# Adicionando um protocolo de conexão segura

Explore os diferentes métodos para manter uma conexão segura com o servidor e implemente aquele que melhor se adapte às necessidades do seu negócio.

## Visão geral

Ter e manter uma conexão segura é extremamente crítico em qualquer aplicativo e é essencial para a segurança e integridade do software. O SwiftNIO e o AsyncHTTPClient tornam a configuração necessária do TLS super fácil, fornecendo um manuseio simples de certificados.

Conforme suportado pelo SwiftNIO, temos as seguintes definições:

1. Trust (Confiança)

   É validado após receber o certificado do servidor para determinar se é confiável ou não para manter a conexão e prosseguir com a requisição em andamento.

2. Client Authorization (Autorização do Cliente)

   Um certificado local é enviado ao servidor para estabelecer a confiança do cliente e obter os dados resultantes do processo da API.

3. PSK (Pre-Shared Key)

   Uma forma alternativa de autenticação em que o cliente e o servidor compartilham uma chave simétrica para prosseguir com a requisição atual.

> Warning: Qualquer configuração envolvendo TLS deve ser feita dentro do ``RequestDL/SecureConnection``. Caso contrário, o RequestDL não será capaz de reconhecer o código declarado.

### Trust (Confiança)

Existem duas camadas de configuração de validação do servidor. A primeira camada são os certificados base para confiar no servidor ao qual estamos conectando. A segunda camada são certificados adicionais que também são usados para o mesmo propósito.

Existem duas maneiras de configurar os certificados base: uma usando ``RequestDL/DefaultTrusts`` e outra usando ``RequestDL/Trusts``. A primeira usa os certificados do sistema, enquanto a segunda substitui completamente a validação do certificado para usar apenas os especificados.

#### DefaultTrusts

```swift
DefaultTrusts()
```

#### Trusts

```swift
Trusts {
  Certificate(file1, format: .pem)
  Certificate(file2, format: .pem)
}
```

Após definir os certificados base, você precisa especificar certificados adicionais a serem usados como validação alternativa do servidor. Isso é opcional e pode ser explorado dependendo das especificações do servidor.

> Tip: Em um aplicativo onde a segurança não é uma prioridade, você pode combinar ``RequestDL/DefaultTrusts`` com ``RequestDL/AdditionalTrusts`` para incluir tanto os certificados do sistema quanto aqueles em que você confia.

### Client Authorization (Autorização do Cliente)

A autenticação do cliente é realizada combinando dois certificados, o público e o privado. A implementação é feita usando ``RequestDL/Certificates`` e ``RequestDL/PrivateKey``.

#### Certificates (Certificados)

Representa os certificados públicos usados pelo cliente para autenticação com o servidor.

```swift
Certificates {
    Certificate(file1, format: .pem)
    Certificate(file2, format: .pem)
}
```

#### PrivateKey (Chave Privada)

O certificado privado é normalmente usado para gerar o certificado público.

```swift
PrivateKey(privateFile1)
```

> Important: Certificados privados protegidos por senha podem ser usados adicionando o parâmetro **`password:`** durante a inicialização.

### PSK (Pre-Shared Key)

Usando chaves simétricas compartilhadas entre o servidor e o cliente, o PSK é uma maneira segura de se comunicar com o servidor.

A configuração é simples e requer apenas a implementação do ``SSLPSKIdentityResolver``. Ao usá-lo na ``Property``, você deve proteger a instância do resolvedor implementado usando ``StoredObject`` para otimizar o código.

```swift
struct GithubAPI: Property {

    @StoredObject var psk = GithubPSKResolver()

    var body: some Property {
        // Outras especificações de propriedade
        SecureConnection {
            PSKIdentity(psk)
        }
    }
}
```

> Warning: Você deve escolher exclusivamente Trust (Confiança) / Autorização do Cliente ou PSK. Definir ambos na mesma requisição pode resultar em comportamento inesperado.

### Otimizações

Apesar de ``RequestDL/Property/body-swift.property`` ser chamado constantemente para cada requisição feita, o RequestDL contém algumas otimizações para evitar a necessidade de recarregar o arquivo.

> Warning: Se os certificados forem atualizados em tempo de execução, o RequestDL não mudará automaticamente para a nova versão. Portanto, ao atualizar o certificado, altere o nome do arquivo para um que ainda não tenha sido usado.

Essa regra é necessária para evitar a criação de novos clientes fornecidos pelo `AsyncHTTPClient`. Além disso, se o seu aplicativo permanecer ocioso por um determinado período de tempo, o RequestDL expirará as informações salvas e começará a usar o novo certificado, a menos que medidas sejam tomadas.

## Topics

### Noções básicas sobre certificados

- ``RequestDL/Certificate``

### Configurando a confiança do servidor

- ``RequestDL/DefaultTrusts``
- ``RequestDL/Trusts``
- ``RequestDL/AdditionalTrusts``

### Configurando a autorização do cliente

- ``RequestDL/Certificates``
- ``RequestDL/PrivateKey``

### Trabalhando com PSK (Pre-Shared Key)

- ``RequestDL/PSKIdentity``
- ``RequestDL/SSLPSKIdentityResolver``

### A configuração do TLS

- ``RequestDL/SecureConnection``
- ``RequestDL/TLSVersion``
- ``RequestDL/TLSCipher``
- ``RequestDL/CertificateVerification``
- ``RequestDL/SignatureAlgorithm``
- ``RequestDL/RenegotiationSupport``
- ``RequestDL/SSLKeyLogger``