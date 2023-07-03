+++
title = "Pipelines como produto - Github actions e pipelines reutilizáveis"
date = "2023-07-03"
draft = false
Categories = ["devops", "IAC", "pipeline"]
Tags = ["devops", "IAC", "pipeline"]
+++

# Introdução

É muito comum, nas organizações que produzem software, que a necessidade de se criar pipelines só cresça com o passar do tempo. Afinal, o pipeline é a melhor forma de automatizar e garantir os melhores padrões de segurança para seu processo de entrega.

Como já foi citado [aqui](https://gomex.me/blog/o_que_deve_ter_no_pipeline-2/) anteriormente, não importa qual ferramenta você usará para criar seus pipelines, mas que ele tenha ao menos a funcionalidade que permite configurar seus pipelines em código, ou seja, você não precisará usar a interface gráfica para criar seus pipelines.

A opção de configurar o pipeline via interface gráfica pode ser tentadora para algumas pessoas que não têm experiência com o código, mas essa forma não é rastreável, ou seja, você não terá os registros das opções que foram modificadas em seu pipeline.

O pipeline como código oferece uma visualização mais clara de como seu pipeline funciona, pois o código apresenta um panorama completo de cada passo. Uma outra vantagem é a possibilidade de replicar esse pipeline em outro lugar, ou até mesmo apenas uma parte dele.

Trataremos aqui de uma forma organizada, padronizada, personalizada e segura para os projetos. E o melhor, criando apenas uma vez e o reutilizando, oferecendo-o como produto para sua organização, e não como um conjunto de arquivos que você copia e perde a referência, mas sim como produto, onde é utilizado o versionamento para que você possa oferecer múltiplas versões do mesmo produto e não mais precisar que o código seja copiado e colado toda vez que sofre uma alteração.

Para exemplificar a ideia de pipeline como produto será usado github actions como ferramenta, mas poderia ser qualquer outra que tivesse funcionalidade parecida.

# Github actions

É uma ferramenta SaaS que oferece o serviço de criação de pipeline como código. Você escreve o seu pipeline em um arquivo yaml dentro da pasta **.github/workflows** na raiz do seu repositório do github e o github actions automaticamente será iniciado a partir dos gatilhos (triggers) que você especificou no arquivo yaml.

Para entender como funciona o Github Actions, a [documentação](https://docs.github.com/pt/actions/learn-github-actions/understanding-github-actions) oficial é muito boa.

Será explicado apenas uma parte de como funciona o workflow do github actions. O suficiente para você entender o que se pretende explicar aqui.
Segue abaixo um exemplo de um workflow que o github actions entende e executará suas tarefas:

```yaml
---
name: CI
on:
  pull_request:
jobs:
  linter:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - name: Instalar o Python 3.9
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - name: Instalar dependencias
        run: python3 -m pip install -r requirements.txt
      - name: Executar linter
        run: ansible-lint
```

Explicarei passo a passo:

```yaml
name: CI
```

O parâmetro **name** é apenas o título do seu workflow.

```yaml
on:
  pull_request:
```

O parâmetro **on** define qual a trigger que fará esse workflow iniciar, que nesse caso é um pull-request, ou seja, toda vez que alguém criar qualquer pull-request nesse repositório, o workflow será executado.

Para entender um pouco mais de pull-request, leia [esse artigo](https://gomex.me/blog/o_que_deve_ter_no_pipeline-pr)

```yaml
jobs:
  linter:
    runs-on: ubuntu-20.04
```

O parâmetro jobs inicia os jobs desse workflows. Esses conceitos de jobs e steps [é detalhadamente explicado nesse artigo](https://gomex.me/blog/o_que_e_pipeline/).

O primeiro job é o **linter**, que abaixo dele tem a configuração de qual sistema operacional será usado para executar (**runs-on**), que nesse caso foi o **ubuntu-20.04**.

Até o momento sabemos que o workflow será iniciado a cada pull-request, que o primeiro job é chamado de linter e ele rodará em um Ubuntu da versão 20.04.

```yaml
steps:
      - name: Baixar o código
        uses: actions/checkout@v2
      - name: Instalar o Python 3.9
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - name: Instalar dependências
        run: python3 -m pip install -r requirements.txt
      - name: Executar linter
        run: ansible-lint
```

Abaixo do parâmetro **steps** temos todos os passos que serão executados nesse job. 

Cada etapa está com o parâmetro **name** que explica bem o que cada uma faz.

Com isso podemos concluir que esse pipeline será executada a cada pull-request, em um ubuntu, baixará seu código, instalará o python, depois as dependências escritas em python que você precisa para então rodar o **ansible-lint**. 

Parece simples e um trabalho aceitável para fazer repetidamente em vários repositórios, correto? Vamos para um caso de exemplo:

Sua empresa tem vários times que precisam desse pipeline, alguns deles não entendem tão bem de pipelines e nem o motivo que motivou você a colocar um **ansible-lint** ali no processo. 

# Qual time deve ser responsável por criar pipelines?

Idealmente todas as pessoas de tecnologia que fazem parte do processo de entrega de software da sua empresa deveriam ser capazes de ler um pipeline e entender o que acontece em cada etapa, ao menos superficialmente.

Em um "mundo perfeito" todas as pessoas também deveriam ter a capacidade de escrever os pipelines, e assim não centralizar em apenas um time a escrita de cada pipeline que roda na empresa.

É importante lembrar que a habilidade de se criar bons pipelines não é algo simples e fácil de se aprender e que o pipeline é uma das peças fundamentais para a velocidade e a qualidade de entrega do seu software.

Ter um time de pessoas que entendam o processo de entrega de software e o software usado para construir pipeline pode ser uma boa estratégia, mas como fazer para que esse time seja usado da melhor forma e não se transforme em um gargalo?

# Pipeline como produto

Pipelines como produtos é o caminho. É possível pensar na formatação de conjunto de etapas comuns que podem ser disponibilizadas como produto e consumidas dentro dos projetos.

Imagine um cenário hipotético de uma empresa que entrega software e ela tem vários times, que basicamente usam Ruby e NodeJS em seus projetos.

Os pipelines dos repositórios de cada produto dessa empresa terá, normalmente, os seguintes passos:

- Checagem estática de estilo e formatação (linter);
- Testes unitários de código;
- Construir a imagem docker;
- Aplicar tag e subir a imagem em um repositório;
- Colocar essa imagem em algum orquestrador de containers em ambiente de teste;
- Testar o serviço rodando em um ambiente não produtivo;
- Aplicar novamente a tag, agora com alguma marcação que indique que está pronta para produção;
- Colocar essa imagem em algum orquestrador de containers, agora em ambiente de produção.

Esses passos citados acima podem acontecer em pipelines distintas, não é necessariamente uma ordem única com apenas uma trigger.

Você pode criar os seguintes pipelines como produto:

- Rodar linter e executar o teste unitário para Ruby e outro para NodeJS;
- Construir sua imagem docker e aplicar tag de desenvolvimento baseado no commit;
- Aplicar uma tag baseado em ambiente;
- Implantar uma imagem qualquer em um ambiente qualquer de orquestração de containers.

Nos passos apresentados anteriormente, veja como ficaria a utilização dos produtos:

- Checagem estática de estilo e formatação (linter)
- Testes unitários de código

Seriam atendidos com o pipeline como produto para cada linguagem.

- Construir a imagem docker

O processo de construir imagens é padrão para qualquer linguagem, ou seja, não precisamos ter um pipeline para cada.
- Aplicar tag e subir a imagem em um repositório
…
- Aplicar novamente a tag, agora com alguma marcação que indique que está pronta para produção

O processo de aplicar tag é padrão, e ele pode ter uma variável de entrada que cuida para que esse pipeline como produto possa aplicar tag tanto de teste como de produção.

- Colocar essa imagem em algum orquestrador de containers em ambiente de teste
…
Colocar essa imagem em algum orquestrador de containers, agora em ambiente de produção

O processo de implantar uma imagem como container em um orquestrador pode ser padronizada e o ambiente escolhido pode ser uma variável.

# Workflows reutilizáveis no Github Actions

Tornar suas pipelines reutilizáveis não é uma tarefa difícil, basta que seu workflow seja genérico o suficiente para atender as demandas que você tem em mente.

Usando o exemplo do ansible-lint

```yaml
---
name: CI
on:
  pull_request:
jobs:
  linter:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - name: Instalar o Python 3.9
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - name: Instalar dependencias
        run: python3 -m pip install -r requirements.txt
      - name: Executar linter
        run: ansible-lint
```

O que mudaria nesse pipeline para que ele pudesse ser reusado em outro repositório?

A mudança principal é na trigger responsável por iniciar o workflow, que ao invés de **pull_request**:

```yaml
---
name: CI
on:
  pull_request:
```

Será **workflow_call**, que é a trigger usada para iniciar o workflow quando ele for convocado, ou seja, ficaria assim:

```yaml
---
name: CI
on:
  workflow_call:
```

E para usar esse workflow, como eu devo colocar no repositório destino?

```yaml
---
name: CI
on:
  pull_request:
jobs:
  linter:
    uses: nome-da-org/repo-workflows-reusaveis/.github/workflows/ansible-linter.yml@v1
```

O parâmetro **uses** é usado para apontar para o workflow que você deseja utilizar. 

Agora esse repositório terá toda aquela implementação de escolher o ubuntu, instalar o python, instalar as dependências e rodar o **ansible-lint** sem que o time responsável pelo pipeline como produto precise escrever tudo novamente.

Para entender melhor tecnicamente o reuso de pipelines no github veja [esse documento](https://docs.github.com/pt/actions/using-workflows/reusing-workflows).

É importante ler quais as restrições do uso dessa funcionalidade. Leia na documentação oficial.

# Organização dos workflows

A melhor parte é que você pode versionar seus workflows reutilizáveis e acrescentar melhorias em seu produto. Dessa forma, as pessoas que usam esse pipeline podem receber melhorias do processo. No caso do linter por exemplo, você pode no futuro descobrir um produto melhor do que o **ansible-lint** e alterar apenas em um arquivo apenas e isso ser replicado para todos os projetos que usam ele.

Você pode oferecer inclusive a possibilidade que as pessoas que usam seu pipeline possam permanecer em versões anteriores e mudem para versão nova quando desejarem e estiverem prontos para tal atividade. Tudo depende de como sua empresa se organiza e atua com relação a isso.

A minha sugestão é que foque nos pipelines mais comuns e com menor complexidade primeiro, pois esses pipelines já poderão oferecer uma boa experiência nesse processo de reuso.

O processo de padronização de pipelines mais simples podem auxiliar a empresa a discutir e definir padrões de entrega, onde toda organização se beneficiaria de cada debate.

Imagine uma empresa com 10 times, cada um fazendo seus pipelines separados, sem comunicação sobre isso. Várias pessoas poderão oferecer soluções incríveis em seus projetos que poderiam beneficiar outros times, mas sem a possibilidade de oferecer isso de forma centralizada esse trabalho é bem mais difícil.

A organização de seus pipelines podem ser feitas de várias formas, mas todas elas estarão em duas linhas essenciais:

Comandos simples
Fluxos complexos e bem rígidos

Usando nosso modelo de exemplo do **ansible-lint**:

```yaml
---
name: CI
on:
  workflow_call:
jobs:
  linter:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - name: Instalar o Python 3.9
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - name: Instalar dependencias
        run: python3 -m pip install -r requirements.txt
      - name: Executar linter
        run: ansible-lint
```

Nesse caso aqui, parece ser simples, mas não é um comando simples. Veja que esse fluxo diz exatamente como algumas regras são rígidas:

- Ele define que todo fluxo usará o ubuntu 20.04, ou seja, não pode fazer isso em outra versão, ou em um Mac, por exemplo.
- Ele define que a versão do python é 3.9 e que o arquivo que será usado para instalar as dependência seja sempre **requirements.txt**

Nenhum desses limites especificados acima é um problema em potencial necessariamente, mas você precisa entender que isso está definindo um fluxo bem específico. 

Para mudar isso você pode usar os inputs e secrets. 

```yaml
---
name: CI
on:
  workflow_call:
    inputs:
      os_version:
        description: "Versão do sistema operacional"
        required: false
        default: "ubuntu-20.04"
        type: string  
      python_version:
        description: "Versão do python"
        required: false
        type: string
        default: '3.9'  
    secrets:
      token:
        required: true
jobs:
  linter:
    runs-on: ${{ inputs.os_version }} 
    steps:
      - uses: actions/checkout@v2
      - name: Instalar o Python ${{ inputs.python_version }} 
        uses: actions/setup-python@v2
        with:
          python-version: ${{ inputs.python_version }} 
      - name: Instalar dependencias
        run: python3 -m pip install -r requirements.txt
      - name: Executar linter
        run: ansible-lint
```

Você manteve o mesmo comportamento, mas permitiu que a pessoa pudesse definir alguns valores e assim mudar o comportamento, caso necessário:

```yaml
---
name: CI
on:
  pull_request:
jobs:
  linter:
    uses: nome-da-org/repo-workflows-reusaveis/.github/workflows/ansible-linter.yml@v1
    with:
      python_version: "3.8"
    secrets:
      token: ${{ secrets.envPAT }}
```

Com esse código você poderá usar um workflow padronizado informando que quer usar o python 3.8 em vez do padrão 3.9 e ainda passa um secrets que será usado nesse execução, mas cujo valor será definido no repositório do produto em questão.

# Conclusão

Utilizar um time especializado em pipelines para entregar pipelines como produto dentro da sua organização pode ser uma boa ideia, desde que esses pipelines sejam parametrizados e pensados junto com os times de produto que usarão os pipelines.

O diálogo com o time responsável por entregar o produto, que constrói o código, os desenvolvedores, é crucial para o sucesso desse trabalho. 

Você pode focar inicialmente em etapas que talvez não sejam familiares para esse time, tal como deploy da aplicação no kubernetes, aplicação de tag, checagem de segurança em imagens docker, criação de infraestrutura com infraestrutura como código e afins.

# Agradecimentos

[Somatório](https://twitter.com/somatorio) que sempre apoia em praticamente tudo que produzo.

[Juliana Gaioso](https://twitter.com/juligaioso) obrigado pela revisão do texto inteiro. Ele ficou bem mais legal depois da sua revisão.

