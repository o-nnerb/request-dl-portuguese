# Armazenando em cache suas respostas

Armazenar em cache as requisições é crucial para economizar custos e evitar telas de carregamento desnecessárias. Aprofunde-se em como usar o sistema de cache do RequestDL.

## Visão geral

Armazenar em cache as requisições é crucial tanto para a experiência do usuário quanto para a redução de custos desnecessários associados ao consumo de APIs remotas.

O RequestDL oferece duas maneiras de fazer isso: por meio de ``RequestDL/Property/cache(memoryCapacity:diskCapacity:url:)`` ou por meio de ``RequestDL/DataCache``.

Dependendo das necessidades de cada projeto, pode ser interessante usar ambos os métodos, pois um deles é completamente independente da lógica da requisição.

> Warning: ``RequestDL/DataCache`` não aplica nenhuma validação em relação à validade do cache.

### Property

Configurar e usar o sistema de cache diretamente durante a especificação da requisição é vantajoso porque é um caminho mais curto. Além disso, dependendo da ``RequestDL/CacheStrategy`` utilizada, o RequestDL verifica se o cache é válido no ponto de extremidade, simples assim.

Para fazer isso, é necessário prestar atenção nos seguintes pontos:

1. Especificar a ``RequestDL/DataCache/Policy``.
2. Escolher a ``RequestDL/CacheStrategy``.
3. Definir o local de armazenamento com ``RequestDL/Property/cache(memoryCapacity:diskCapacity:url:)``.

Aqui está um exemplo de como esses três pontos são implementados na prática:

```swift
DataTask {
    BaseURL("apple.com")
        .cachePolicy(.memory)
        .cacheStrategy(.returnCachedDataElseLoad)
        .cache(url: cacheStorageURL)
}
```

A definição do local de armazenamento é opcional. Os valores padrão para uso em memória e em disco são 2 MB. No entanto, ``RequestDL/DataCache/Policy`` e ``RequestDL/CacheStrategy`` são essenciais para o cache ativo.

> Important: Existem três opções para escolher onde armazenar o cache: URL, suiteName ou exclusivamente reservado para o aplicativo.

### DataCache

Outra maneira de armazenar o resultado de uma requisição é usando ``RequestDL/DataCache`` diretamente. Você até mesmo pode implementar sua própria lógica para armazenar e usar o cache padrão do RequestDL ao fazer a requisição novamente.

Estes são os principais métodos de uso:

@Links(visualStyle: list) {
    - ``RequestDL/DataCache/getCachedData(forKey:policy:)``
    - ``RequestDL/DataCache/setCachedData(_:forKey:)``
    - ``RequestDL/DataCache/remove(forKey:)``
}

> Note: Assim como usamos métodos para especificar o local de armazenamento em uma especificação de requisição, os inicializadores do ``RequestDL/DataCache`` estão disponíveis com as mesmas opções.

## Topics

### O sistema de cache

- ``RequestDL/DataCache``
- ``RequestDL/CachedData``
- ``RequestDL/EmptyCachedDataError``

### Definindo a estratégia

- ``RequestDL/CacheStrategy``
- ``RequestDL/Property/cacheStrategy(_:)``

### Definindo a política

- ``RequestDL/DataCache/Policy``
- ``RequestDL/DataCache/Policy/Set``
- ``RequestDL/Property/cachePolicy(_:)``

### Inicializando o cache

- ``RequestDL/Property/cache(memoryCapacity:diskCapacity:)``
- ``RequestDL/Property/cache(memoryCapacity:diskCapacity:suiteName:)``
- ``RequestDL/Property/cache(memoryCapacity:diskCapacity:url:)``