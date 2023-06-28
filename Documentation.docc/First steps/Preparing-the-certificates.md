# Preparando os Certificados

Configure todos os certificados necessários para garantir requisições seguras.

## Visão Geral

A configuração mais importante necessária em qualquer aplicativo é aplicar alguns protocolos de segurança, como TLS, mTLS ou PSK. Aqui, forneceremos instruções detalhadas sobre a forma mais básica de implementar esses protocolos diretamente em seu aplicativo.

Estamos compartilhando dois métodos para obter certificados de servidor para ajudar você a explorar os recursos disponíveis. Além disso, obter um certificado de servidor é bastante fácil nos dias de hoje.

### Obtendo o Certificado

#### via Navegador

Neste exemplo, vamos obter os certificados DER do servidor `api.github.com` para nosso aplicativo.

Um ponto crucial é baixar toda a hierarquia de certificados, porque ao configurá-los no RequestDL ou em outros frameworks como URLSession, o aplicativo irá comparar todos os certificados recebidos.

1. Acesse `https://api.github.com` e clique no cadeado.

    ![Captura de tela do Safari mostrando a URL carregada com o cadeado destacado](der.github.1.png)

2. Clique em **Mostrar Certificado**.

    ![Captura de tela do Safari mostrando o certificado da URL com o botão "Mostrar Certificado" destacado](der.github.2.png)

3. Selecione **\*.github.com** e arraste o certificado para a área de trabalho.

    ![Captura de tela do Safari mostrando os certificados em uso com o ícone do certificado destacado](der.github.3.png)

4. Repita o mesmo processo para **DigiCert TLS...**.

    ![Captura de tela do Safari mostrando os certificados em uso com o ícone do certificado destacado](der.github.4.png)

5. Repita o mesmo processo para **DigiCert Global...**.

    ![Captura de tela do Safari mostrando os certificados em uso com o ícone do certificado destacado](der.github.5.png)

6. Arraste os certificados para a pasta **Resources** do seu projeto ou módulo.

Neste exemplo, baixamos toda a hierarquia de certificados do servidor. No entanto, temos a opção de usar ``RequestDL/DefaultTrusts`` em conjunto com ``RequestDL/AdditionalTrusts`` para obter um resultado semelhante.

> Observação: É recomendado sempre converter os certificados para o formato PEM, pois isso permite combiná-los em um único arquivo para uso em qualquer lugar.

#### via Terminal

Outro método é usar o comando `openssl` no seu terminal. Ao usar o comando abaixo, você pode obter o certificado em formato **PEM**.

1. Abra o terminal.

2. Digite o comando `openssl s_client -connect api.github.com:443`.

3. Procure por `BEGIN CERTIFICATE` e `END CERTIFICATE` na saída:
    ```
    -----BEGIN CERTIFICATE-----
    ***
    ***
    ***
    -----END CERTIFICATE-----
    ```

4. Copie o padrão acima e cole-o em um arquivo.

5. Salve-o como **\*.github.com.pem**.

6. Arraste o arquivo gerado para a pasta **Resources** do seu projeto ou módulo.

Neste exemplo, baixamos apenas o certificado principal do servidor sem baixar a hierarquia completa. Você pode encontrar outros exemplos na comunidade que explicam como fazer isso via terminal.

### Configurando GithubAPI

Se você leu **[Criando a propriedade do projeto](<doc:Creating-the-project-property>)**, o exemplo de configuração que usaremos é uma continuação da implementação do GithubAPI.

#### Usando Certificados DER Individualmente

```swift
import RequestDL

struct GithubAPI: Property {

    var body: some Property {
        // Código anterior
        SecureConnection {
            Trusts {
                Certificate("DigiCert Global...", format: .der)
                Certificate("DigiCert TLS...", format: .der)
                Certificate("*.github.com", format: .der)
            }
        }
    }
}
```

#### Usando Certificados PEM

```swift
import RequestDL

struct GithubAPI: Property {

    var body: some Property {
        // Código anterior
        SecureConnection {
            Trusts {
                Certificate("*.github.com", format: .pem)
            }
        }
    }
}
```

## Próximos passos

Esses foram alguns exemplos básicos de como configurar certificados para validar o servidor durante uma requisição. Ao fazer isso, você estará usando segurança TLS em sua camada de rede.

Embora possa não ser a opção mais segura, também oferecemos suporte para implementar mTLS ou PSK. Continue explorando nossa documentação para obter informações mais relevantes.