+++
title = "Os primeiros passos para uma carreira DevOps"
date = "2018-01-05"
draft = false
Categories = ["portugues", "devops", "carreira"]
Tags = ["portugues", "devops", "carreira"]
+++

## TL;DR

ESSE ARTIGO ESTÁ DEPRECIADO! Favor acessar o [artigo novo](https://gomex.me/blog/primeiros_passos_devops).

Quer entrar na carreira "DevOps" e não sabe por onde começar? Nesse texto falarei sobre possíveis caminhos para sua carreita técnica, com materiais para auxiliar no processo de aprendizado e dicas para acelerar seu processo.

### Motivação

Muitas pessoas me perguntam por onde começar a trabalhar com "DevOps" e sempre prometo escrever um artigo com alguma síntese das minhas opiniões sobre como seria um início "ideal" de carreira. Aqui estou pagando uma promessa do ano passado.

### Colaboração

O texto não é 100% autoral, ou seja, imagine esse texto como uma colagem de muitas ideias e materiais produzidos por outras pessoas. Ele provavelmente sofrerá alteração ao longo do tempo. Isso que dizer que se você acha que algo deve ser colocado aqui, modifique [esse arquivo](https://github.com/gomex/gomex.me/blob/master/content/post/carreira-devops.md) e submeta um PR no github, vamos discutir nos comentários e aceitar a colaboração :)

### DevOps é carreira?

Então, a resposta direta e resumida é: NÃO, mas por outro lado o mercado já usa esse termo para denominar pessoas e vagas de trabalho relacionadas a entrega contínua, infraestrutura ágil e afins, não vou entrar nesse embate no texto. Se quiser discutir isso, entre [nesse canal](https://t.me/devopsbr) do telegram que tem pessoas dispostas a essa conversa.

### O que é DevOps de verdade?

É um mudança cultural, acredito que [esse](https://www.youtube.com/watch?v=9B2f3wcKp-I) vídeo e tenha uma ideia básica do que se trata. Esse vídeo tem legenda em português, caso precise.

### Requisitos

Vou presumir que você é uma pessoa que possui conhecimentos que permitam boa leitura e interpretação de textos em inglês e já está inserida no básico de tecnologia da informação, ou seja, que já é ao menos uma pessoa de nível técnico de TI.

### Metodologia

Eu vou tentar organizar a ordem do conhecimento com base nos elementos mais básicos que você precisará para se qualificar para o mercado de trabalho e assunto teóricos mais básicos. Isso quer dizer que não precisa estudar tudo que eu falar para poder submeter para aquela vaga "DevOps", lembre-se: "O não você já tem sem tentar" (Autor desconhecido).

### Por onde começar?

Eu aconselharia os assuntos com maior demanda e mais próximo do que a maioria dos técnicos de TI já tem alguma familiaridade, sendo assim vou separar em dois sub-grupos de pessoas, que posteriormente se encontram em passos mais relacionados a esse novo paradigma.

Algumas pessoas gostam e tem mais experiência em escrever software e outras em manter a infraestrutura onde estarão hospedadas esses softwares, sendo assim acredito que os elementos básicos necessários para fazer a mudança na carreira são diferentes a depender da sua história.

#### Assuntos básicos para pessoas inclinadas a desenvolvimento de software

Estude sobre TCP/IP! Domine isso! Entenda como funciona endereço IP, mascara de rede, rotas de rede, portas TCP/UDP. O que é um socket e afins.

Material sugerido:

- [Livro Redes de Computadores - Andrew S Tanenbaum](https://www.estantevirtual.com.br/livros/andrew-s-tanenbaum/redes-de-computadores/2516815423?q=Redes+de+Computadores)
- [Redes de computadores e a internet uma nova abordagem - James Kurose e Keith Ross](https://www.estantevirtual.com.br/livros/james-f-kurose-keith-w-ross/redes-de-computadores-e-a-internet-uma-nova-abordagem/2190281067)

Estude também sobre funcionamento do sistema operacional, o que são processos, como funciona o escolanamento de processamento e afins.

Material sugerido:

- [Livro Sistemas Operacionais Modernos - Andrew S Tanenbaum](https://www.estantevirtual.com.br/livros/andrew-s-tanenbaum/sistemas-operacionais-modernos/2487067088?q=Tanenbaum)

 Obs: Esses dois livros são enormes, então não precisa necessariamente ler todos. Apenas imagine que quanto mais você ler e absorver esses conhecimentos, melhor profissional você será.

#### Assuntos básicos para pessoas inclinadas a infraestrutura de TI

Estude sobre desenvolvimento de software! Não apenas sobre fazer scripts para tarefas simples. Você precisa entender como funciona o processo inteiro de desenvolvimento de software. Isso não quer dizer necessariamente que você se tornará uma pessoa desenvolvedora de software.

A proposta aqui é ter o mesmo conhecimento que uma pessoa em início de carreira deveria ter, isso quer dizer que você precisa aprender a coisa mais básica nesse assunto: ALGORITMO!

Material sugerido:

- [Livro Algoritmos Teoria e Prática - Thomas H Cormen](https://www.estantevirtual.com.br/livros/thomas-h-cormen/algoritmos-teoria-e-pratica/1833161560?q=Thomas+H+Cormen)

Obs: A ideia aqui é ter uma base sólida sobre o que é Algoritmo, não precisa ler o livro inteiro

Um outro conselho básico é a necessidade de dominar ao menos uma linguagem de programação a nível iniciante. Que você consiga usar os elementos mais básicos da linguagem para desenvolvimento de recursos simples, como um job para execução pontual, um serviço web e afins.

Material sugerido:

- [Curso online gratuito - Python Para Zumbis - Fernando Masanori](https://www.pycursos.com/python-para-zumbis/)
- [Curso online pago - POO com Ruby - Jackson Pires](https://www.udemy.com/poo-ruby/learn/v4/content)
- [Curso online pago - Curso de Go - Jeff Prestes](https://www.udemy.com/cursodego/)

Se quiser validar seus conhecimentos, tem alguns sites que podem lhe ajudar nisso:

- [Hacker Rank](https://www.hackerrank.com)
- [Project Euler](https://projecteuler.net/)

### Conhecimentos comuns

A partir daqui os conhecimentos são para qualquer tipo de pessoa, sendo assim aconselho a todos fazerem a medida que vejam necessidade dentro da sua carreira.

Para as pessoas que não dominam desenvolvimento de software, perceberão que muitos dos assuntos são completamente novos e o caminho parece um pouco mais "tortuoso", o que em parte é verdade, mas veja pelo lado bom, uma vez concluído esse caminho você dominará a área profissional mais requisitada no mercado e será capaz de resolver a maioria dos desafios, mesmo com pouca ajuda.

#### Conhecimento de controle de versão de código

Esse é o "pontapé inicial" para o assunto "DevOps", pois tudo que será feito daqui pra frente será baseado em código, e manter esse conjuntos de fontes em um repositório de controle de versão é requisito mínimo até mesmo para níveis mais iniciantes nessa carreira.

Na minha opinião, o Git reina em absoluto nesse assunto. Há quem ainda use SVN, Mercurial e afins, mas a maioria dos lugares usam Git. Pode estudar sem culpa, pois aprender os outros será fácil depois do Git.

Material sugerido para Git:

- [Vídeo - Descomplicando o GIT - Parte 1 - LinuxTips](https://www.youtube.com/watch?v=_aj3hsEh9iw)
- [Material oficial Git](https://git-scm.com/book/en/v2)
- [Material oficial Git (Português)](https://git-scm.com/book/pt-br/v1/Primeiros-passos-No%C3%A7%C3%B5es-B%C3%A1sicas-de-Git)

### Conhecimento sobre virtualização

Você precisa saber o mínimo de virtualização, pois a partir desse momento em diante você necessariamente estará trabalhando com algum nível de virtualização na maioria dos serviços que for interagir/manter.

Material sugerido:

- [Virtualization Getting Started - Oficial Red Hat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/virtualization_getting_started_guide/index)

OBS: Não precisa ser especialista no assunto, mas antes de ver todo resto eu dedicaria um tempo entendendo esse assunto, pois os assuntos Cloud e containers serão mais fáceis pra você depois desse estudo.

#### Conhecimento de Cloud

Aprenda sobre como utilizar algum fornecedor de Cloud, pois boa parte dos serviços são parecidos em seu funcionamento, ou seja, se você aprender um deles não encontrará muitos problemas em utilizar seus concorrentes. Na perspectiva de possibilidade de trabalho, eu aconselharia você começar pela AWS, que tem o maior números de clientes hoje.

Material sugerido para AWS:

- [Curso online pago - Cloud Guru - AWS Certified Solutions Architect Associate](https://www.udemy.com/aws-certified-solutions-architect-associate/)
- [Canal Youtube Cloud Guru](https://www.youtube.com/channel/UCp8lLM2JP_1pv6E0NQ38pqw)

Material sugerido para Google Cloud:

- [Lab: Build a Continuous Deployment Pipeline with Jenkins and Kubernetes ](https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes)

Material sugerido para Azure:

- [Portal de treinamentos gratuitos de Azure criado pela Microsoft](http://aka.ms/azureskillspt)
- [Microsoft Virtual Academy - Academia virtual da Microsoft com treinamentos gratuitos de Azure](https://mva.microsoft.com/search/SearchResults.aspx#!q=Azure&prod=Azure)
- [Lista curada de aprendizado de Azure - Ricardo Martins](https://github.com/rmmartins/AzureReadiness)

#### Conhecimento de gerência de configuração

Você precisará automatizar a configuração dos seus ativos (servidores, switchs e afins), com isso algumas ferramentas precisam de alguma atenção no seu estudo. Você precisa de um nível acima do básico em ao menos uma delas. Eu aconselho experimentar todas descritas abaixo, mas escolha uma para especialização:

- Puppet
- Chef
- Ansible

Material sugerido para Puppet:

- [Curso grátis - Oficial Puppet](https://learn.puppet.com/category/self-paced-training)

Material sugerido para Chef:

- [Curso grátis - Oficial Chef](https://learn.chef.io/)

Material sugerido para Ansible:

- [Ansible - Getting started](http://docs.ansible.com/ansible/latest/intro_getting_started.html)
- [Webinars oficiais do Ansible](https://www.ansible.com/resources/webinars-training)

#### Conhecimento de containeres

Você precisará saber sobre containers e duas ferramentas dominam esse assunto, Docker e Kubernetes. Você precisa saber além do básico em ambos produtos.

Material sugerido para Docker:

- [Livro Containers com Docker - Daniel Romero ](https://www.casadocodigo.com.br/products/livro-docker)
- [Livro Aprendendo Docker - Wellington da Silva ](https://www.amazon.com.br/Aprendendo-Docker-Wellington-Figueira-Silva/dp/8575224867?tag=goog0ef-20&smid=A1ZZFT5FULY4LN&ascsubtag=65e71e4e-1464-4ddc-a0a5-491e02f24552)
- [Livro Descomplicando Docker - Jeferson Fernando Noronha](http://www.brasport.com.br/informatica-e-tecnologia/arquitetura-de-nuvem/descomplicando-o-docker/)
- [Livro Docker para desenvolvedores - Livro Código Aberto - Rafael Gomes](https://leanpub.com/dockerparadesenvolvedores)

Material sugerido para Kubernetes:

- [Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [Treinamentos online grátis oficiais](https://kubernetes.io/docs/tutorials/)
- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- [Tradução do Kubernetes The Hard Way](https://github.com/cgbas/kubernetes-do-jeito-dificil)

#### Conhecimento de CI/CD

É importante conhecer sobre Continuous Integration e Continuous Delivery. Ambos são conceitos centrais dessa mudança de paradigma de desenvolvimento de software e fornecimento de infraestrutura automatizada.

Como material sugerido, eu aconselho o que pra mim seria a bíblia do "DevOps":

- [Continuous Delivery - Jez Humble e  David Farley ](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912)

Esse livro é grande, mas uma leitura simples! Tem a versão traduzida se preferir:

- [Entrega Contínua - Jez Humble e  David Farley](https://www.estantevirtual.com.br/martins-livreiro/jez-humble-e-david-farley-entrega-continua-como-entregar-software-de-forma-rapida-e-confiavel-931382091)

O Jenkins "reina" com uma vantagem considerável entre as ferramentas mais comum sobre esses assuntos.

Material sugerido para Jenkins:

- [Curso online pago - Master Jenkins CI For DevOps and Developer](http://bit.ly/2COXw2U)
- [Livro Jenkins - Automatize tudo sem complicações - Fernando Boaglio](https://www.casadocodigo.com.br/products/livro-jenkins)

Como ferramentas alternativas temos o [GoCD](https://docs.gocd.org/current/), [TeamCity](https://confluence.jetbrains.com/display/TCD10/TeamCity+Documentation) e outras também relevantes, mas estudando o Jenkins você terá a base sólida sobre uso de ferramentas de CI/CD. Fique atento aos conceitos informados no livro "Entrega Contínua".

### Conclusão

Importante salientar que os conhecimentos necessários para uma carreira "DevOps" não acabam aqui, mas acredito que esses sejam os mais básicos/intermediários para quem tinha interesse em ter um "norte" mais curto/médio prazo.

Lembre-se, esse é um texto em construção constante e feito através de colaboração de muitas pessoas. Se você tem alguma sugestão, mande um PR a partir [desse arquivo](https://github.com/gomex/gomex.me/blob/master/content/post/carreira-devops.md).

### Agradecimentos

A todos do canal [DevOpsBR no Telegram](https://t.me/devopsbr) que colaboram em enriquecer o texto.

### Fontes

- [Guto Carvalho](http://gutocarvalho.net/blog/2016/09/06/por-onde-iniciar-os-estudos-sobre-devops/)
- [Awesome DevOps-BR](https://github.com/devops-br/awesome-devops-br)
