+++
title = "Como começar na carreira de DevOps"
date = "2022-03-04"
draft = false
Categories = ["portugues", "devops", "carreira"]
Tags = ["portugues", "devops", "carreira"]
+++

# Resumo

Esse texto tem como objetivo oferecer o meu ponto de vista sobre o início na “carreira de DevOps”. Falarei sobre quais caminhos você pode seguir e o que eu faria se estivesse começando hoje. 

É importante ressaltar que comecei minha carreira a 16 anos atrás, ou seja, estou fazendo um grande esforço para demonstrar quais são os passos mais relevantes a serem dados por quem está começando hoje, dadas as necessidades, expectativas e o contexto atual do mercado.

A ideia é melhorar esse documento a medida que eu for recebendo retornos das pessoas, ou seja, se você leu esse material no lançamento, talvez precise voltar aqui depois e olhar a novidades.

Esse artigo é uma versão melhorada [do artigo antigo](https://gomex.me/blog/carreira-devops/) (de 2018), que tinha o mesmo objetivo.

# Colaboração

Imagine esse texto como uma colagem de muitas ideias e materiais produzidos por outras pessoas. Ele provavelmente sofrerá alteração ao longo do tempo. Isso quer dizer que se você acha que algo deve ser colocado aqui, envie um [pull request](https://gomex.me/blog/o_que_deve_ter_no_pipeline-pr/) para modificação [desse arquivo](https://github.com/gomex/gomex.me/blob/master/content/blog/primeiros_passos_devops.md), para que eu possa analisar o seu ponto de vista e acrescentar no texto, se for o caso.

# DevOps é carreira?

Então, a resposta direta e resumida é: NÃO, mas por outro lado o mercado já usa esse termo para denominar pessoas e vagas de trabalho relacionadas à entrega contínua, infraestrutura ágil e afins, não entrarei nesse embate no texto. Assumirei que quando falarmos de "carreira devops" estaremos falando sobre tudo o que o mercado entende a respeito disso, ok?

Não adianta lutar contra isso, pois é uma batalha perdida.

# Metodologia

Organizarei os conteúdos/temas  com base nos elementos mais básicos que você precisará para começar. Tentarei definir o nível de aprofundamento necessário que você precisará ter/buscar em cada um deles, mas tudo isso é muito subjetivo, sendo assim continue estudando até se sentir confiante.

<!-- vscode-markdown-toc -->
* 1. [Por onde começar?](#Porondecomear)
	* 1.1. [Sistemas operacionais](#Sistemasoperacionais)
	* 1.2. [Redes](#Redes)
	* 1.3. [Programação](#Programao)
* 2. [Próximos passos](#Prximospassos)
	* 2.1. [Cloud](#Cloud)
	* 2.2. [Containers](#Containers)
	* 2.3. [Infraestrutura como código](#Infraestruturacomocdigo)
	* 2.4. [CICD](#CICD)
	* 2.5. [Kubernetes](#Kubernetes)
* 3. [Outros assuntos importantes](#Outrosassuntosimportantes)
	* 3.1. [Git](#Git)
	* 3.2. [Comunicação HTTP](#ComunicaoHTTP)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

##  1. <a name='Porondecomear'></a>Por onde começar? 

Existem algumas tecnologias, padrões e conceitos que serão a base de tudo que você irá aprender durante toda sua carreira, logo, se eu fosse começar hoje daria especial atenção para esses "pré requisitos".

<img src="img/onde_comecar.png" width="200">


Sei que cada um de nós vive uma realidade completamente distinta e, nem sempre é fácil estudar. Cada um tem seu tempo, processo de aprendizado e isso é muito distinto. Portanto, apresentarei o conteúdo de maneira geral e, vocês podem acessar de acordo com a necessidade, da forma que melhor se ajustar ao seu processo.

Em minha opinião, existem 3 domínios que precisam ser estudados minimamente antes do que qualquer outra coisa, pois eles tem o básico que você precisa pra entender o restante. Isso não é uma regra que funciona com todo mundo, ou seja, se você achar mais legal estudar outra coisa avançada sem saber isso, sem problemas.

Os domínios são:

 - Sistema operacionais
 - Redes
 - Programação

###  1.1. <a name='Sistemasoperacionais'></a>Sistemas operacionais

Você precisa entender minimamente o que é um sistema operacional, e o mais usado em servidores é o Linux. Eu diria que se você estudar o Linux, aprender o Windows será uma tarefa menos complexa no futuro.

Segue abaixo alguns vídeos para começar:

 - [O que é Linux? - Do Diolinux](https://www.youtube.com/watch?v=K05CssAbQgo)
 - [Curso GNU Linux -  Do Kretcheu](https://www.youtube.com/watch?v=SZMIL87CyVE&list=PLuf64C8sPVT9L452PqdyYCNslctvCMs_n&ab_channel=PauloKretcheu)


Um bom material para você começar e também se aprofundar é o [Guia foca](https://guiafoca.org/guiaonline/iniciante/).

Se quiser se aprofundar com cursos, a Linuxtips é uma boa opção:

 - [Treinamento Linux Expert](https://www.linuxtips.io/products/treinamento-linux-expert)

Se quiser aprofundar com livro, esse são uma boas opções:

 - [Livro Como Linux Funciona - Brian Ward ](https://www.amazon.com.br/Como-Linux-Funciona-Brian-Ward/dp/8575224190)

 - [Livro Sistemas Operacionais Modernos - Andrew S Tanenbaum](https://www.estantevirtual.com.br/livros/andrew-s-tanenbaum/sistemas-operacionais-modernos/2487067088?q=Tanenbaum)

**O que você precisa saber para começar** é importante que consiga ao menos executar comandos na console e fazer o que chamamos de "troubleshooting" básico, que é a atividade de executar comandos para obter dados do problema que você está investigando. Exemplo: Você inicia uma aplicação web e tenta acessar no seu navegador, mas ela não funciona, daí você executa ```ss -napt``` para listar todas portas com os ips que estão "escutando" o tráfego. Percebe que a sua porta não está lá e depois executa ```ps -aux``` para descobrir que o processo não está rodando, ou seja, algo aconteceu na inicialização do serviço, por fim, talvez seja o caso de olhar o log da aplicação para entender o que possa ter acontecido, ou iniciar ela em modo "debug", que é quando você inicia a aplicação de uma forma que ela informa mais sobre comportamento do que o normal.

PS: Log é normalmente um arquivo de texto com informações sobre como sua aplicação está lidando com seu trabalho.

###  1.2. <a name='Redes'></a>Redes

Você precisa entender minimamente como uma máquina se comunica com a outra. Não acho que será necessário se aprofundar em todos os protocolos possíveis, mas entender o básico de um endereçamento IP, a pilha TCP/IP, cabeçalho HTTP, socket de rede, rotas e saber o que é loopback. 

 - [Esses vídeos](https://www.youtube.com/watch?v=EYQu7uNKvYg) vão do [Kretcheu](https://twitter.com/kretcheu) vão ajudar você a começar a entender o básico.

Se quiser aprofundar com livro, esses são uma boa opção:

 - [Redes de computadores e a internet uma nova abordagem - James Kurose e Keith Ross](https://www.estantevirtual.com.br/livros/james-f-kurose-keith-w-ross/redes-de-computadores-e-a-internet-uma-nova-abordagem/2190281067)
 - [Livro Redes de Computadores - Andrew S Tanenbaum](https://www.estantevirtual.com.br/livros/andrew-s-tanenbaum/redes-de-computadores/2516815423?q=Redes%20de%20Computadores)

**O que você precisa saber para começar** é importante que consiga ao menos entender como um computador conseguiu se comunicar com o outro e mais, você vai ter elementos para ajudar no *troubleshooting* que expliquei na parte de sistemas operacionais, pois aqui ficará mais claro o que são as portas, sockets e endereços IP.

PS: Os livros da sessão de redes são muito densos, ou seja, não precisa ler todo para começar.

###  1.3. <a name='Programao'></a>Programação

Você precisa ter noções de programação para as automatizações, contudo, não são necessários conhecimentos aprofundados em algorítmos, porém, o básico das linguagens deve ser entendido. A maioria dos scripts simples podem ser feitos em Shell. Para tarefas mais complexas, aconselho que você aprenda o básico de Python, Ruby ou Golang.

Um bom material pra começar é o [Pense Python](https://penseallen.github.io/PensePython2e/), que lhe ensinará algoritmo ao mesmo tempo que passa conhecimentos sobre python também. Os exercícios são a parte mais importante desse material, não deixe de fazer.

**O que você precisa saber para começar** é entender como funciona minimamente um processo de desenvolvimento de software. Saber rodar, entender como funciona o processo de **debug**, que consiste em pesquisar onde está o problema daquele software, e por fim ter empatia com quem escreve software, pois você entende minimamente que não é algo simples.

Se quiser se aprofundar em desenvolvimento de software, segue abaixo algumas sugestões:

- [Site do Aurélio Marinho Vargas - shell](https://aurelio.net/shell/)

- [Livro Shell Script Profissional do Aurélio](https://www.novatec.com.br/livros/shellscript/?idA=12)

- [Python Fluente de Luciano Ramalho](https://novatec.com.br/livros/pythonfluente/) é uma obra prima e merece sua atenção.


##  2. <a name='Prximospassos'></a>Próximos passos

Com o básico, você pode iniciar o aprendizado em elementos mais centrais do que seria o seu dia a dia de trabalho. São em sua maioria ferramentas que serão utilizadas, com base nos conhecimentos dos domínios básicos que apresentei anteriormente.

Os novos domínios são

  - Cloud
  - Containers
  - Infraestrutura como código 
  - CICD
  - Kubernetes

###  2.1. <a name='Cloud'></a>Cloud

Você precisa entender o que é a Cloud, como proposta de entrega de serviços mantidos por outra empresa e como usar as ofertas mais comuns, da cloud mais usada.

Pra começar eu indico alguns materiais:

 - [Uma série de vídeos em português do Um Inventor Qualquer sobre AWS](https://www.youtube.com/watch?v=j6yImUbs4OA&list=PLOF5f9_x-OYUaqJar6EKRAonJNSHDFZUm&ab_channel=UmInventorQualquer)
 - [Vídeo completo da live do Bonde do OCI da Linuxtips](https://www.youtube.com/watch?v=jWG3gVf2YWE&ab_channel=LINUXtips) OCI é a cloud da Oracle. Não é a mais usada, mas ajuda você a entender os fundamentos das clouds em geral.
 - [Material oficial da Oracle sobre sua cloud](https://videohub.oracle.com/playlist/details/1_msn4b3ax/categoryId/158145621)
 - [Cursos oficiais e grátis da AWS (a maioria em inglês, mas tem cursos em português)](https://explore.skillbuilder.aws/)


**O que você precisa saber para começar** é qual a ideia geral de uma cloud e um entendimento básico dos seus produtos mais comuns, que são os produtos que configuram sua rede, cria máquinas, banco de dados, permissão de acesso e object store.

Se quiser se aprofundar, a Linuxtips é uma boa opção, mas é pago:

 - [Treinamento AWS Expert](https://www.linuxtips.io/products/treinamento-aws-expert)

Na coursera tem cursos que também são muito bons:

- [Cursos Cloud Computing (em inglês)](https://www.coursera.org/specializations/cloud-computing#courses)

###  2.2. <a name='Containers'></a>Containers

Você precisa entender o que é um container. Não caia na armadilha de considerar ele uma máquina mais enxuta, pois esse é o erro mais comum de todo mundo que começa estudar containers.

Container é um processo isolado em execução. Se você estudou sistemas operacionais, processos deve ter sido um assunto que você leu, se não leu, eu aconselho fortemente voltar lá e ler essa parte, inclusive os comandos pra listar e manipular processos.

Pra começar eu indico alguns materiais:

 - [O livro que criei e ajudei a organizar - Docker para Desenvolvedores](https://leanpub.com/dockerparadesenvolvedores) - Você pode baixar ele de graça, só zerar o valor de doação.
 - [Decomplicando docker da Linuxtips](https://github.com/badtuxx/DescomplicandoDocker)
 - [Curso online do Caio Delgado](https://www.youtube.com/playlist?list=PL4ESbIHXST_TJ4TvoXezA0UssP1hYbP9_)

**O que você precisa saber para começar** é a utilização mais simples de container, que se resume a fazer build de imagem, e com isso saber o que é Dockerfile, iniciar containers expondo portas, montando volumes. É importante também fazer troubleshooting, que seria comandos como docker exec, docker logs e outros para encontrar os motivos que seus container não está iniciando corretamente.

Se quiser se aprofundar, a Linuxtips é uma boa opção, mas é pago:

 - [Treinamento Container Expert](https://www.linuxtips.io/products/treinamento-container-expert)

###  2.3. <a name='Infraestruturacomocdigo'></a>Infraestrutura como código 

Você precisa entender o objetivo da Infraestrutura como código (IaC - Infra as Code). É mandatório saber qual problema esse modelo de manutenção de infraestrutura resolve. Resumidamente podemos dizer que IaC tem como objetivo criar a possibilidade de que a partir de um arquivo com definições específicas a sua infra será criada automaticamente, sem necessidade de intervenção manual.

É como se você pudesse dizer para a ferramenta assim:

"Ei! Eu preciso de um servidor, com 2GB, 4 CPU, 100GB de disco, seja criado no datacenter de Nova York, ela precisa de apenas uma placa de rede, o firewall deve liberar apenas o acesso a porta 22 do SSH (Porta usada para conectar remotamente na linha de comando para manutenção) e 80 do HTTP (Porta usada para comunicação web)"

Com base nesse exemplo, você teria um arquivo assim:


Nome do arquivo: servidor.exemplo.config

```yaml
servidor1:
  memoria: 2GB
  cpu: 4
  disco: 100GB
  datacenter: nova_york
  firewall:
    http:
      porta: 80
      ação: liberar
    ssh:
      porta: 22
      ação: liberar
```

Com posse do arquivo, você teria um binário, que é um arquivo especial do seu sistema operacional, que pode ser executado. Esse é o software em si. Esse binário lerá o arquivo de configuração apresentado acima (servidor.exemplo.config).

A execução seria mais ou menos assim:

```shell
binario –configuracao servidor.exemplo.config aplicar
```


 - **binario** é o nome do binario no exemplo apresentado.
 - **--configuracao** é o parâmetro desse binario exemplo.
 - **servidor.exemplo.config** é o nome do arquivo de configuração.
 - **aplicar** é o comando para aplicar a infraestrutura baseada no arquivo de configuração.

PS: Os exemplos de comando e arquivo de configuração apresentados acima são **puramente** um exemplo didático, ou seja, nenhuma das ferramentas que IaC que conheço funciona com esses exatos parâmetros, comandos e formato de arquivo de configuração. A ideia desses exemplos é apenas apresentar o funcionamento mais elementar de uma ferramenta de IaC.

Como materiais para começar, eu aconselho esses abaixo:

 - [Ansible 101 - Infraestrutura como Código com Ansible do Caio Delgado](https://www.youtube.com/watch?v=GfOj2wgxyF4&ab_channel=CaioDelgado)
 - [Terraform 101 - Instalação e Deploy AWS EC2 o Caio Delgado](https://www.youtube.com/watch?v=bYvdJKTwx_I&ab_channel=CaioDelgado)
 - [Descomplicando o Terraform | HashiWeek comigo e Badtux do Linuxtips](https://www.youtube.com/watch?v=4FellihAcV8&ab_channel=LINUXtips)
 - [Curso do Igor sobre Terraform](https://www.youtube.com/watch?v=JayShFpuRdY&list=PLVGIivuHGmJpyciRgdZ-x4avdzlsdCTmH&ab_channel=IgorSouza)

**O que você precisa saber pra começar** é como usar os recursos fundamentais de ferramentas de criação de infra baixa, que o mais famoso é terraform, e o mesmo para uma ferramenta de gerência de configuração, que é o ansible, ou seja, aprendendo de terraform e ansible você já tem uma boa fundação para começar.

Materiais para se aprofundar:

 - [Cursos da carreira IaC expert da Linuxtips, eu sou instrutor de alguns cursos](https://www.linuxtips.io/products/treinamento-infra-as-code-expert)
 - [Livro Ansible for Devops do Jeff Geerling](https://leanpub.com/ansible-for-devops)
 - [Plataforma de labs da Hashicorp](https://learn.hashicorp.com/terraform?utm_source=terraform_io)

###  2.4. <a name='CICD'></a>CICD

Você precisa entender que CICD são duas coisas em uma, separando temos CI, que é Continuous Integration (Integração contínua) que é a ideia de você está sempre reunindo seu código em uma local, para que ele possa ser validado/tratado e ter um feedback rápido sobre a sua situação, pois a ideia é que cada membro do time possa fazer alterações no mesmo.

CD é Continuous Delivery (Entrega contínua), que é uma a ideia de seu código precisa ser entregue em algum ambiente o mais rápido possível. Esse modelo vai além da ferramenta, ele é uma forma de como pensar seus problemas e quebrar o código para atender cada parte dos desafios em pequenas entregas. A ferramenta nesse caso, é responsável pelo [pipeline](https://gomex.me/blog/o_que_e_pipeline/) estará focada na melhor forma de como viabilizar isso tecnicamente.

Materiais para começar a entender:

 - [Meu artigo sobre o que é deploy](https://gomex.me/blog/o_que_e_deploy/)
 - [Meu artigo sobre o que é pipeline](https://gomex.me/blog/o_que_e_pipeline/)
 - [Meu artigo sobre O que deve ter no seu pipeline](https://gomex.me/blog/o_que_deve_ter_no_pipeline/)
 - [Livro Deploy em produção para desenvolvedores](https://leanpub.com/deployemprodparadevs)

**O que você precisa saber para começar** é como configurar um pipeline em alguma ferramenta, que pode ser o [github actions](https://docs.github.com/pt/actions/learn-github-actions). 

Se quiser se aprofundar, o meu conselho é investir nesse livro aqui:


 - [Entrega Contínua do Jezz Humble e David Farley](https://www.amazon.com.br/Entrega-Cont%C3%ADnua-Entregar-Software-Confi%C3%A1vel/dp/8582601034) O livro é caro, mas vale a pena como investimento.

###  2.5. <a name='Kubernetes'></a>Kubernetes

Você precisa entender que o kubernetes, também comumente chamado de **k8s**, é um orquestrador de containers, isso quer dizer que ele é responsável por iniciar o container e prover todas as funcionalidades necessárias para ele funcionar corretamente. Tal como guardar segredos, configurações, localização de containers por nome, agrupamento por tags, alta disponibilidade, escalabilidade e afins.

Materiais para começar a entender:


 - [Descomplicando Kubernetes da Linuxtips](https://github.com/badtuxx/DescomplicandoKubernetes)
 - [Vídeo sobre Kubernetes do Caio Delgado](https://www.youtube.com/watch?v=PPBjWvUSgSE&ab_channel=CaioDelgado)

**O que você precisa saber para começar** são os elementos mais básicos do k8s, tal como os comandos mais comuns para iniciar um novo container, fazer deploy, listar os pods, services e afins.

Se quiser se aprofundar:


 - [Curso descomplicando kubernetes da Linuxtips](https://www.linuxtips.io/products/descomplicando-o-kubernetes)

##  3. <a name='Outrosassuntosimportantes'></a>Outros assuntos importantes

Existem outros assuntos que merecem atenção, e eles podem ser encarados depois de você iniciar em tudo que falei até o momento, mas perceba que isso não é uma regra. Tanto faz a ordem, desde que você consiga avançar.

###  3.1. <a name='Git'></a>Git

Git é uma ferramenta para fornecer controle de versão dos seus códigos. É usada para garantir que você terá o controle de cada mudança do seu código. É dessa forma que você conseguirá voltar para uma versão anterior, caso a mais nova esteja com problemas, por exemplo.

Materiais para começar a entender:

 - [Curso Git e Github para iniciantes do Willian Justen](https://www.udemy.com/course/git-e-github-para-iniciantes)
 - [Vídeo COMO USAR GIT E GITHUB NA PRÁTICA da Rafaella Ballerini](https://www.youtube.com/watch?v=UBAX-13g8OM&ab_channel=RafaellaBallerini)

**O que você precisa saber para começar** são os comandos mais básicos, que são git init, git add, git commit, git status e git push. Eles são os mais usados em boa parte das oportunidades que você terá no começo da carreira.

Para quem deseja se aprofundar, a documentação dele é uma boa pedida:

 - [Documentação](https://git-scm.com/doc)

###  3.2. <a name='ComunicaoHTTP'></a>Comunicação HTTP


Você precisa entender que a maioria da comunicação que será feita nas aplicações que você tomará conta será com HTTP, sendo assim, é importante ter um conhecimento mínimo do assunto.

Materiais para começar:


 - [Vídeo Como funciona uma requisição HTTP? da Space Rails](https://www.youtube.com/watch?v=fhAXgcD21iE)
 - [Documentação da Mozilla sobre HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Overview)

**O que você precisa saber para começar** é como funciona a comunicação, quais métodos existem, juntamente com seus propósitos, assim como HTTPS e como funciona o certificado.

Materiais para se aprofundar:

 - [Livro Desconstruindo a web do Willian Molinari (PotHix) ](https://www.casadocodigo.com.br/products/livro-desconstruindo-web)

# Conclusão

É importante perceber que isso aqui são os primeiros passos, ou seja, talvez uma vaga ou outra precise de outros assuntos, mas eu tentei apresentar os assuntos que fazem sentido ter minimamente para a maioria das vagas que já tive oportunidade de conhecer.

# Agradecimentos

[Somatório](https://twitter.com/somatorio) que sempre revisa tudo que escrevo.

Agradeço também as contribuições das pessoas que seguem abaixo:

- [Rafael Silva](https://twitter.com/rafaotetra)
- [Jairo]()
- [Infoslack](https://twitter.com/infoslack)
- [Saulo](https://twitter.com/madalozzo)
- [Guto Carvalho](https://twitter.com/gutocarvalho)
- [Edson](https://twitter.com/tuxpilgrim)
