+++
title = "O que deve ter no seu pipeline? Testes!"
date = "2020-06-08"
draft = false
Categories = ["portugues", "pipeline"]
Tags = ["portugues", "pipeline as code", "pipeline", "devops", "qa"]
+++

## Contextualização

Essa é a continuação da série ["O que deve ter no seu pipeline?"](https://gomex.me/categories/pipeline/), que tem como objetivo apresentar as melhores práticas para construção de um pipeline, baseada em minha experiência, seja em projetos ou em leitura.

## Github actions

Lembra que falamos sobre ["Qual software de pipeline (CI/CD) você deve usar?"](https://gomex.me/2020/05/28/qual-software-de-pipeline-ci/cd-voc%C3%AA-deve-usar/), para demonstrar aqui na prática o que deve ter no seu pipeline eu usarei o [Github actions](https://github.com/features/actions) como ferramenta de pipeline, pois para repositórios públicos ele funciona sem custos e precisa de quase nada para começar a usar.

Crie a pasta **.github/workflows** na raiz do seu repositório. Vamos precisar posteriormente.

Se você ainda tem dúvida de qual ferramenta usar e quer ler um pouco sobre isso. Leia de novo [esse](https://gomex.me/2020/05/28/qual-software-de-pipeline-ci/cd-voc%C3%AA-deve-usar/) artigo.

## Código exemplo

Usarei [esse](https://github.com/gomex/go-example-app) repositório como exemplo para demonstrar a implantação de todos os passos.

## O que deve ter no seu pipeline?

Como já exaustivamente explicado em outros momentos dessa série, sabemos que o teste estático de código é uma das primeiras etapas de um pipeline, sendo assim vamos demonstrar aqui como executar o teste unitário de uma aplicação exemplo escrita em go.

### Teste unitário

Para executar o teste é necessário uma série de passos e vamos detalhar eles um pouco mais nos tópicos a seguir.

Dentro da pasta .github/workflows crie um arquivo chamado validate.yml com o seguinte conteúdo:

```yaml
name: Go

on:
  push:
    branches: [ master ]

  pull_request:
    branches: [ master ]

jobs:

  test:
    name: test
    runs-on: ubuntu-20.04

    steps:

    - name: Set up Go 1.14
      uses: actions/setup-go@v2
      with:
        go-version: 1.14

    - name: Check out code
      uses: actions/checkout@v2

    - name: Test
      run: go test -v -coverprofile=coverage.out
```

#### Detalhando o arquivo descritivo do pipeline

Vamos por partes, o começo do arquivo basicamente descrever o nome e a condição de execução do workflow do github actions em questão, pois é possível ter múltiplos workflows executando em paralelo a partir do mesmo commit.

```yaml
name: Go

on:
  push:
    branches: [ master ]

  pull_request:
    branches: [ master ]
```

A opção **on** informa que esse workflow **apenas** será executado se houver um push na branch **master** ou um Pull Request (PR) que tem como destino a **master**. 

Se houver um commit direto na master, esse workflow executará automaticamente e se alguém criar uma branch, fizer modificações e então quiser fazer com que seu código seja feito merge com oa branch **master**, ao criar um PR acionará automaticamente esse workflow.

Vale a pena reforçar que esse fluxo de etapas acontecerá com alguma frequência. Evite tarefas que não precisem **necessariamente** serem executadas a cada PR. Alguns exemplos de etapas que não fazem sentido de executar quando alguém abrir um PR:

 - Build, tag e push de um artefato
 - Deploy em staging ou produção

##### O que é PR (Pull request)?

No git você tem a possibilidade criar várias branchs a partir de uma branch existente. A branch padrão normalmente é a master, dai normalmente o fluxo de trabalho para quem usa git é:

 - Criar uma branch nova de trabalho a partir da branch master;
 - Enviar suas modificações para esse branch de trabalho recém criada;
 - Em algum momento seu trabalho estará pronto para ser compartilhado com o time;
 - Você abre um **pull request**, que é o pedido para que o contéudo da sua branch seja copiada para a master;
 - Outras pessoas do seu time avaliam o seu pedido e olham se o código enviado de fato atende da melhor forma possível;
 - Essas pessoas aceitam o seu PR e seu código agora faz parte da branch master, onde o ciclo todo pode começar de novo com outra pessoa ou você novamente.

 Obs: O **pull request** pode ser aberto de qualquer branch para uma outra a sua escolha. O exemplo acima foi apenas para apresentar o conceito de forma prática

##### Por que determinadas tarefas não devem executar a cada PR?

Imagine que o PR é uma ferramenta que deve ser usado por qualquer pessoa da sua empresa. Isso deve ser estimulado, pois a transparência e ampla colaboração tende a colaborar a qualidade do seu código.

Levando com consideração que qualquer pessoa pode abrir um PR, você quer mesmo que seu pipeline gaste tempo gerando um artefato novo a cada PR? Se é um pedido, que deve ser revisado, existe motivo para que um artefato seja gerado, aplicado tag e enviado para um repositório centralizado? Repositório esse que normalmente tem seu custo associado a quantidade de uso, ou seja, quanto mais push de artefatos novos, mais caro será sua conta.

Uma consequência similar acontece no deploy, pois se você permite que o PR acione automaticamente um deploy em staging ou production, você não está fornecendo o tempo necessário que uma pessoa possa revisar devidamente o código antes dele ser aceito em staging ou production, ao abrir o PR o github action fará o deploy automaticamente.

##### Jobs

Dando continuidade ao detalhamento do arquivo **validate.yml** desse pipeline, falaremos sobre configuração de jobs:

```yaml
jobs:

  validate:
    name: validate
    runs-on: ubuntu-20.04
```

A definição de jobs nesse arquivo tem como proposito descrever quais etapas desse pipeline executarão a partir do mesmo sistema operacional e de for sequêncial, ou seja, se você precisar que seu build execute em multiplos sistemas operacionais, tal como windows e linux, será necessário informar um job para cada proposito.

Sempre que fizer um pipeline, inicie ele simples, coloque apenas um caminho a ser seguido, evite condicionais, faça com que ele cresça baseado nas demandas e não nas suas expectativas, isso quer dizer que deixar ele quebrar e apresentar alguns dados é uma boa maneira de deixar ele o mais enxuto possível, pois um pipeline complexo tem maior possibilidade de sofrer pouca manutenção por outros membros do time.

Lembre-se! O pipeline deve ser do time e não seu, pois se seu tempo for todo dedicado **apenas** para corrigir pipelines, voltaremos ao tempo em que as pessoas não tinham tempo porque estavam executando tarefas muitos simples e operacionais, que somente você poderia fazer.

A descrição **validate** define o nome desse job e o **runs-on** onde as etapas desse pipeline serão executadas.

Perceba que além de informar que precisa que seja GNU/Linux e distribuição ubuntu, é informado qual versão especifica do Ubuntu você deseja: **ubuntu-20.04**.

Sempre que possível informe as versões do que deseja para instalar, configurar ou afins, pois somente assim você terá controle que aquele pipeline se comportará de acordo com o que você espera dele. Imagine uma versão nova do Ubuntu que remova ou mude o comportamento de uma funcionalidade que você use para criar seu artefato. Seu pipeline irá falhar por um motivo completamente fora do seu escopo de trabalho.

Existe uma ideia amplamente seguida por pessoas que usam o pipeline, que é a ideia que se o pipeline quebrou, o motivo deveria ser daquele commit que fez esse pipeline ser inciado. Isso quer dizer que qualquer fator externo que possa fazer ele quebrar deve ser isolado.

##### Passos do seu pipeline

Na descrição seguinte temos a descrição dos passos. Lembre-se que elas seguem a ordem do arquivo, ou seja, as primeiras etapas serão as primeiras a serem executadas e a posterior só acontece se o passo em questão não falhar.

```yaml
    steps:

    - name: Set up Go 1.14
      uses: actions/setup-go@v2
      with:
        go-version: 1.14

    - name: Check out code
      uses: actions/checkout@v2

    - name: Test
      run: go test -v -coverprofile=coverage.out
```

Em cada passo você pode informar:


 - **name**: O nome do passo. Ele serve para que você possa saber qual passo falhou e por qual motivo no dashboard.
 - **uses**: Aqui é informado a ação a ser executada. Uma ação é um bloco de código que executa uma determina função. Ela pode ser de um repositório externo ou dentro do seu repositório. Por hora vamos usar apenas de outros repositórios. Se quiser mais detalhe pode ler [aqui](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsuses)
 - **run**: É uma alternativa do **uses** e deve ser usado sempre que você for apenas rodar um comando no sistema operacional escolhido na opção **runs-on** (ubuntu-20.04).
 - **with**: Você pode especificar variáveis de ambiente, que normalmente são necessários para utilizar uma determinada ação no **uses**. Cada ação especifica em sua documentação individual quais variáveis de ambiente é necessário informar. Acesse a [documentação](https://github.com/actions/setup-go) do setup-go e veja isso na prática.

O primeiro passo desse fluxo é o **Set up Go 1.14** ele é responsável por configurar o ambiente para se utilizar o go na versão que você deseja. Aqui é basicamente o momento que será instalado tudo que você precisa do go no ubuntu que foi especificado anteriormente nesse arquivo.

Um segundo passo é o **Check out code**, que é responsável por baixar todo o seu código no diretório de trabalho do build.  Esse passo é automatico na maioria das outras ferramentas de CI/CD, mas no Github actions é necessário especificar o passo, caso contrário tudo que precisar do código para executar não funcionará corretamente.

O terceiro passo é o **Test**, aqui é onde acontece o teste unitário do Go. Veja que ele depende dos binários do go, por isso ele acontece depois do **Set up Go 1.14** e também depende que o código esteja no diretório de trabalho, por isso ele será executado depois do passo **Check out code**.

Dentro do passo **Test** o comando acompanha a opção **-coverprofile=coverage.out** que será utilizada em um artigo posterior, onde avançaremos em mais detalhes sobre cobertura de testes e afins.

## Conclusão

É importante sempre ficar atento a ordem dos passos e como umas influenciam as outras. Cada ferramenta de CI/CD terá sua particularidade e isso muitas vezes afeta diretamente em como seu pipeline é configurado.

## Agradecimentos

Meu agradecimento a [Victor](https://twitter.com/vcrmartinez) que revisou esse artigo antes dele sair.

Escrevi esse artigo ouvindo:

- Foo Fighters
- Bob Dylan
- Arnaldo Baptista
- Marília Mendonça
- Beethoven
- Outras músicas do meu Daily Mix do Spotify

