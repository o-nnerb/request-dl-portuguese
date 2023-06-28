# Utilizando progresso para requisições de upload e download

Explore como monitorar suas requisições e rastrear o progresso de cada operação de forma precisa.

## Visão geral

A forma como o SwiftNIO e o AsyncHTTPClient enviam e recebem dados do servidor permite a implementação de uma variedade de recursos interessantes para lidar com os bytes brutos envolvidos na operação.

Seja enviando um ``RequestDL/Payload`` ou um ``RequestDL/Form``, é possível serializar a transmissão em partes e monitorar o progresso do upload. O mesmo se aplica ao download, pois o mesmo processo acontece internamente.

### Monitores

Explorar esse recurso no RequestDL envolve separar os conceitos de upload e download, que são representados pelo objeto ``RequestDL/AsyncResponse``. A base do RequestDL é assíncrona e é totalmente suportada pelos princípios discutidos aqui.

Ao usar o modificador ``RequestDL/Modifiers/Progress``, processamos os bytes enviados e recebidos e notificamos o respectivo monitor em cada etapa. Para esse fim, foram implementados o ``RequestDL/UploadProgress`` e o ``RequestDL/DownloadProgress``.

## Topics

### Documentação relacionada

- <doc:Exploring-payload>

### As tarefas essenciais

- ``RequestDL/UploadTask``
- ``RequestDL/DownloadTask``

### Conhecendo o progresso

- ``RequestDL/UploadProgress``
- ``RequestDL/DownloadProgress``
- ``RequestDL/Progress``

### Descobrindo os modificadores

- ``RequestDL/Modifiers/Progress``
- ``RequestDL/Modifiers/IgnoresProgress``
- ``RequestDL/Modifiers/CollectBytes``
- ``RequestDL/Modifiers/CollectData``