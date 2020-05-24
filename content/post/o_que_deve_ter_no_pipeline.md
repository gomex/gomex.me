+++
title = "O que deve ter no seu pipeline? Parte 1"
date = "2020-05-23"
draft = false
Categories = ["portugues", "pipeline"]
Tags = ["portugues", "pipeline as code", "pipeline", "devops", "qa"]
+++

## Contextualização

Ao longo de alguns anos de experiência tenho percebido que muitas pessoas tem dúvidas, ou desconhecem os elementos que podem ser usados para compor um pipeline de entrega de produto.

Pretendo iniciar uma série de artigos para tentar compartilhar o pouco que sei sobre o assunto.

Não tenho pretenção alguma de aqui fundar nenhum padrão ou ideia nova. O que apresento aqui é nada mais do que a soma de experiências, algumas minhas, mas muito mais de outras pessoas, então não tenho intenção alguma de tomar para mim todo crédito, afinal toda construção de novo conteúdo é assim, correto? 10% experiência própria e 90% de aprendizado prévio.

Não vou me aprofundar sobre o que é pipeline, nem muito menos tentar lhe convencer a usá-lo, pois se chegou até aqui por esse título, acredito que você já tenha interesse. Prometo que se tiver demanda sobre um artigo especifico sobre o que é pipelione, eu escrevo em outra oportunidade sobre esses assuntos.

Vou levar em consideração também que você já sabe o que [pipeline as code](https://www.thoughtworks.com/radar/techniques/pipelines-as-code) significa, mas se não souber, não causará muito problema ao seu aprendizado.

## Problema

Antes de "colocar a mão na massa" e iniciar o processo de construção do seu pipeline, você precisa entender qual problema você está tentando resolver, pois toda intervenção na computação tem (ou deveria ter) como objetivo resolver algum problema, correto? Mesmo que o problema seja otimização, por conta de performance, ou trabalho proativo para que não exista problema no futuro.

Quando você inicia a construção de um pipeline, normalmente, seu objetivo é entregar um produto. Seja ele de software ou infraestrutura. 

Se a solução do problema aqui é entregar o produto de forma automatizada, você precisa entender quais são os passos que seu produto precisa seguir para ser colocado em produção.

Existem alguns passos que seguem alguns padrões, e são esses que serão foco nesse artigo. 

## A ordem importa? 

Antes de apresentar os passos, precisamos primeiro entender que a ordem das etapas do pipeline importam **e muito**, sendo assim apresentarei as etapas aqui na ordem que elas devem estar no seu pipeline. 


### Por que a ordem importa?

Uma das vantagens de usar pipeline no processo de entregar de software é a ideia dele "economizar" tempo das pessoas que estão produzindo código, então o ideal é que as tarefas que demoram menos, entregam algum valor e não dependem de outros passos posteriores sejam as primeiras no seu pipeline.

Vamos usar um exemplo abstrato. No processo de entrega de um software hipotético, temos os seguintes passos:

Build do artefato
Teste unitário
Provisionamento da infra pré-produção
Teste de integração
Teste de aceitação
Provisionamento de infra produção
Deploy de pré-produção
Deploy de produção

Na sua opinião, qual seria o primeiro? Vamos analisar cada passo:

**Build do artefato** precisa de outro passo? Não. Ele entrega valor? Entrega sim, pois se o build quebrar, a pessoa que está desenvolvendo saberá que tem problemas para fazer build, mas esse processo de build costuma demorar demasiadamente e isso pode fazer com que o feedback seja demora. Vamos imaginar juntos: A pessoa manda o commit para o repositório, o pipeline automaticamente é executado e depois de alguns longos minutos a pessoa que mandou o commit poderá descobrir que errou, muitas vezes um detalhe bobo.

E se pensarmos no **teste unitário**? Precisa de outro passo? Não. Ele entrega valor? Entrega sim, e aqui temos um detalhe diferente do **build do artefato**, pois o retorno é mais rápido, uma vez que, normalmente, nada precisa ser realmente construído. Por padrão os testes unitários demoram menos do que o build dos artefatos. Voltando ao processo de imaginação: A pessoa manda o commit para o repositório, o pipeline automaticamente é executado e depois de **segundos** ela já terá um feedback mais rápido que determinado teste não está passando. Tudo por culpa daquele "detalhe" bobo que falamos anteriormente.

Seguindo essa lógica, o primeiro passo desse pipeline seria o **teste unitário**, pois não há nada que demore menos e ainda assim não dependa de outro passo. Vejam que são sempre multiplos fatores para determinar a ordem e em minha opinião são normalmente esses:

- Dependência de outro passo
- Entrega de valor
- Tempo de feedback

Quando eu falo de entrega de valor, a preocupação é com o processo de desenvolvimento e não apenas com o produto finalizado. Para o produto finalizando o build talvez seja mais importante do que o teste unitário, pois levando em consideração o processo de desenvolvimento eles tem importâncias bem próximas.

### Agradecimentos

Obrigado a [Somatório](https://twitter.com/somatorio) que, como sempre, revisou esse material antes dele sair.

Escrevi esse artigo ouvindo:

- Foo Fighters
- Yung Buda
- Racionais
- Sabotagem
- Outras músicas do meu Daily Mix do Spotify

