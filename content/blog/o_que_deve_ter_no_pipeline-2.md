+++
title = "Qual software de pipeline (CI/CD) você deve usar?"
date = "2020-05-28"
draft = false
Categories = ["portugues", "pipeline"]
Tags = ["portugues", "pipeline as code", "pipeline", "devops", "qa"]
+++

## Contextualização

Esse artigo faz parte da série ["O que deve ter no seu pipeline?"](https://gomex.me/categories/pipeline/), que tem como objetivo apresentar as melhores práticas para construção de um pipeline, baseada em minha experiência, seja em projetos ou em leitura.

Quando pensei em escrever sobre "O que deve ter no seu pipeline?" muitas pessoas pediram para eu fazer comparações entre ferramentas de CI/CD, e por conta disso vou escrever um artigo especificamente sobre isso.

## Qual software de pipeline (CI/CD) você deve usar?

Antes de você iniciar uma saga para encontrar a melhor ferramenta de CI/CD que você possa, tente elaborar um pouco quais são seus requisitos. Os meus são nessa ordem:

 - Suporte a pipeline as code
 - Suporte a builds com container docker
 - Boa documentação
 - Opensource
 - Comunidade ativa

### Suporte a pipeline as code

Se a ferramenta não tiver suporte a definir meu pipeline como código, eu nem cogito a testar a ferramenta.

#### O que é Pipeline as code?

É a possibilidade de colocar as descrição de cada passo do seu pipeline em um arquivo definição, ao invés de ficar clicando em uma interface web e armazenando essas configurações em uma base de dados.

Segue abaixo as vantagens de ter as definições em um arquivo:

 - Os passos que sua aplicação segue até o deploy em produção estarão documentados em um arquivo dentro do seu repositório de código, ou seja, qualquer pessoa que ler essa informação estará rapidamente ciente de todo processo;
 - O arquivo de definição será versionado junto com seu código, isso indica que você pode ver quando um determinado passo mudou e afetou o comportamento do seu pipeline;
 - Uma vez versionado esse arquivo fica aberta as sugestões e qualquer pessoa pode mandar um PR (pull request) como proposta para mudar o comportamento do seu pipeline. É possível inclusive configurar seu pipeline para que esse PR seja executado com o arquivo proposto, dessa forma validando, ou não, a hipótese da pessoa que enviou o PR.

Segue um exemplo de um arquivo pipeline as code:

```
---
    kind: pipeline
    type: docker
    name: default
    
    steps:
    - name: teste
      image: <nome_da_imagem>
      commands:
        - <comandos para instalar pacotes que precisam no teste>
        - <comando para teste>
    
    - name: build
      image: <nome_da_imagem>
      environment:
        ENV: "production"
      commands:
        - <comandos para instalar pacotes que precisam no build>
        - <comando para build>
```

Como vocês podem ver no exemplo resumido acima, é possível saber quem primeiro executa o teste, sem qualquer variável de ambiente. No passo seguinte, uma variável de ambiente é necessário para que o build execute de forma correta, e em seguida são demonstradas os comandos que precisam para executar o build.

Qualquer pessoa do time que abrir esse arquivo, seja uma pessoa nova ou mais antiga na equipe conseguirá entender como o processo de teste e build funcionam no pipeline.

### Suporte a builds com container docker

A possibilidade de eu usar uma ferramenta de CI/CD que não permite executar builds com container docker é muito baixa.

#### O que é na prática "builds em container docker"? 

Cada passo do seu pipeline é executado em algum lugar, ou seja, quando você diz na etapa do pipeline que você quer executar o comando de teste unitário, por exemplo, esse teste será executado em um agente e caso seu pipeline não tenha suporte a execução em docker, esse agente será uma máquina que foi previamente configurada.

Essa máquina tem binários que foram instalados e configurados previamente. E se você precisar de bibliotecas novas ou uma versão mais atualizada do binário que é responsável pelo teste? A opção seria colocar o comando para instalar esses pacotes antes de executar o teste, correto? Errado!

Um agente de build nesse modelo descrito acima é normalmente compartilhado. Isso quer dizer que um comando para atualizar o pacote, poderá afetar outro time, ou até mesmo seu próprio time na execução de pipelines antigos para simulação de comportamentos anteriores ao código atual.

Quando seu pipeline tem suporte a execução das etapas em container docker, isso quer dizer que cada passo do seu pipeline iniciará um container docker a partir de uma imagem descrita por quem idealizou o pipeline. Isso implica que você pode informar a imagem que deseja usar, colocar todos os comandos para atualizar a biblioteca ou binário e então executar o teste sem culpa, pois ao terminar  todos os comandos desse passo, o container será destruído e reconstruído do zero na próxima execução.

Se você precisar executar um pipeline antigo, não terá problema, pois nesse pipeline estará a versão que funcionava, inclusive com os comandos específicos para instalação dos pacotes necessários. Aqui entra uma dica MUITO importante:

Sempre que utilizar uma imagem e precisar instalar um pacote dentro dela, informe de forma específica qual a versão que deseja instalar.

Vejam no exemplo abaixo:

```
---
    kind: pipeline
    type: docker
    name: default
    
    steps:
    - name: teste
      image: ruby:2.7.1-buster
      commands:
        - gem install bundler -v 2.1.3
        - bundle exec rspec
```

Se você não informar a versão específica de qual bundler você quer instalar, ele instalará a mais atual do momento e isso pode ser um problema, pois pacotes são atualizados o tempo todo e seu pipeline poderá quebrar porque você instalou de forma equivocada de algum binário a ser usado, pois o comportamento mudará.

Uma das coisas mais importantes de um pipeline é garantir que ele tenha o comportamento esperado. Quanto mais mecanismos você acrescentar para que cada execução de um pipeline sempre aconteça de maneira esperada, melhor ele será para você encontrar o problema de fato quando quebrar, pois você não terá dúvida que o motivo esteja de fato relacionado a modificação do código em questão.

#### Desvantagem ao utilizar "builds em container docker"? 

Lembra que cada execução de um pipeline é único e o container é destruído após o ultimo comando daquela etapa? Isso implica que todos os arquivos gerados naquele passo serão destruídos por padrão.

A ideia é que qualquer dado que precise ser usado em outra etapa que seja persistido de outra forma. Segue algumas dicas:


 Se for arquivo, a maioria das ferramentas oferece a possibilidade de montar um volume que pode ser compartilhado entre as etapas
Se for uma imagem, biblioteca ou pacote, esse deve ir para um repositório e então no passo que precise ser usado, deve conter um comando para baixar a dependência e instalar.

### Conclusão

Não é necessário detalhes sobre "Boa documentação", "OpenSource" e "Comunidade ativa". Basta que cada item desse seja verdadeiro para ser considerado como um ponto positivo.

Não há ferramenta perfeita, mas existem algumas funcionalidades que você não deveria abrir mão na hora de escolher a sua. Use algumas, veja qual seu time usaria melhor e proponha um teste, pois afinal de contas não importa qual ferramenta você usa e sim como você configura seu pipeline.

## Agradecimentos

Obrigado a [Somatório](https://twitter.com/somatorio) que, como sempre, revisou esse material antes dele sair.
Obrigado também a [Morvana](https://twitter.com/morvanabonin), [Giu](https://twitter.com/ReginaSauro) e [Victor](https://twitter.com/vcrmartinez) que também revisaram esse artigo antes dele sair.

Escrevi esse artigo ouvindo:

- Megadeth
- Bob Dylan
- Backstreet Boys
- Emicida
- Beethoven
- Outras músicas do meu Daily Mix do Spotify

