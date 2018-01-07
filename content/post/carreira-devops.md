+++
title = "Carreira DevOps"
date = "2017-01-05"
draft = false
Categories = ["portugues", "blog"]
Tags = ["portugues", "blog"]
+++

## TL;DR

Que entrar na carreira "DevOps" e não sabe por onde começar? Esse texto tem como objetivo esclarecer alguns pontos e mostrar possíveis caminhos, com materiais para auxiliar no processo de aprendizado e dicas para acelerar seu processo.

### Motivação

Muitas pessoas me perguntam por onde começar a trabalhar com "DevOps" e eu desde sempre prometo escrever um artigo com alguma síntese do que acho. Aqui estou eu pagando minha promessa.

### Colaboração

Tudo que está aqui não é apenas meu, ou seja, imagine esse texto como uma colagem de muitas ideias e materiais produzidos por outras pessoas. Esse texto provavelmente sofrerá alteração ao longo do tempo. Isso que dizer que se você acha que algo deve ser colocado aqui, modifique [esse arquivo](https://github.com/gomex/gomex.me/blob/master/content/post/carreira-devops.md) e submeta um PR no github, vamos discutir nos comentários e aceitar a colaboração :)

### DevOps é carreira?

Então, a resposta direta e resumida é: NÃO, mas por outro lado o mercado já usa esse termo para denominar pessoas e vagas de trabalho relacionadas a entrega contínua, infraestrutura ágil e afins, não vou entrar nesse embate no texto. Se quiser discutir isso, entre [nesse canal](https://t.me/devopsbr) do telegram que tem pessoas dispostas a essa conversa.

### O que é DevOps de verdade?

É um mudança cultural, acredito que [esse](https://www.youtube.com/watch?v=9B2f3wcKp-I) vídeo e tenha uma ideia básica do que se trata. Esse vídeo tem legenda em português, caso precise.

### Requisitos

Vou presumir que você é uma pessoa que inglês não é problema na leitura de textos e já está inserido no básico de tecnologia da informação, ou seja, que já é ao menos uma pessoa de nível técnico de TI.

### Metodologia

Eu vou tentar organizar a ordem do conhecimento com base nos elementos mais básicos que você precisará para se qualificar para o mercado de trabalho e assunto teóricos mais básicos. Isso quer dizer que não precisa estudar tudo que eu falar para poder submeter para aquela vaga "DevOps", lembre-se: "O não você já tem sem tentar" (Autor desconhecido).

### Por onde começar?

Eu diria pelos assuntos com maior demanda e mais próximo do que a maioria dos técnicos de TI já tem alguma familiridade, mas quando falamos de TI se torna muito abrangente, sendo assim vou separar em dois sub-grupos de pessoas, que posteriormente eles se encontram em passos posteriores.

#### Separação com base na experiência/interesse prévio

Algumas pessoas gostam e tem mais experiência em escrever software e outras em manter a infraestrutura onde estarão hospedadas esses softwares, sendo assim acredito que os elementos básicos que se precisa obter são específicos.

##### Assuntos básicos para pessoas inclinadas a desenvolvimento de software

Estude sobre TCP/IP! Domine isso! Entenda como funciona endereço IP, mascara de rede, rotas de rede, portas TCP/UDP. O que é um socket e afins.

Material sugerido:
- [Livro Redes de Computadores - Andrew S Tanenbaum](https://www.estantevirtual.com.br/livros/andrew-s-tanenbaum/redes-de-computadores/2516815423?q=Redes+de+Computadores)
- [Redes de computadores e a internet uma nova abordagem - James Kurose e Keith Ross](https://www.estantevirtual.com.br/livros/james-f-kurose-keith-w-ross/redes-de-computadores-e-a-internet-uma-nova-abordagem/2190281067)

Estudo também sobre funcionamento do sistema operacional, o que são processos, como funciona o escolanamento de processamento e afins.

Material sugerido:
- [Livro Sistemas Operacionais Modernos - Andrew S Tanenbaum](https://www.estantevirtual.com.br/livros/andrew-s-tanenbaum/sistemas-operacionais-modernos/2487067088?q=Tanenbaum)

 Obs: Esses dois livros são enormes, então não precisa necessariamente ler todos. Apenas imagine que quanto mais você ler e absorver esses conhecimentos, você será um profissional melhor.

##### Assuntos básicos para pessoas inclinadas a infraestrutura de TI

Estudo sobre desenvolvimento de software! Não apenas sobre fazer scripts para tarefas simples. Você precisa entender como funciona o processo inteiro de desenvolvimento de software. Isso não quer dizer necessariamente que você se tornará uma pessoa desenvolvedora de software.

A proposta aqui é ter o mesmo conhecimento que um desenvolvedor junior deveria ter, isso quer dizer que você precisa aprender a coisa mais básica nesse assunto: ALGORITIMO!

Material sugerido:
- [Livro Algoritimos Teoria e Prática - Thomas H Cormen](https://www.estantevirtual.com.br/livros/thomas-h-cormen/algoritmos-teoria-e-pratica/1833161560?q=Thomas+H+Cormen)

Obs: A ideia aqui é ter uma base sólida sobre o que é Algoritimo, não precisa ler o livro inteiro

Um outro conselho básico é a necessidade de dominar ao menos uma linguagem de programação a nível iniciante. Que você consiga usar os elementos mais básicos da linguagem para desenvolvimento de recursos simples, como um job para execução pontual, um serviço web e afins.

Material sugerido:
- [Curso online gratuíto - Python Para Zumbis - Fernando Masanori](https://www.pycursos.com/python-para-zumbis/)
- [Curso online pago - POO com Ruby - Jackson Pires](https://www.udemy.com/poo-ruby/learn/v4/content)

Se quiser validar seus conhecimentos, tem alguns sites que podem lhe ajudar nisso:
- [Hacker Rank](https://www.hackerrank.com)
- [Project Euler](https://projecteuler.net/)

#### Conhecimentos comuns

A partir daqui os conhecimentos são para qualquer tipo de pessoa, sendo assim aconselho a todos fazerem a medida que vejam necessidade dentro da sua carreira.

##### Conhecimento de Cloud

Aprenda sobre como utilizar algum fornecedor de Cloud, pois boa parte dos serviços são parecidos em seu funcionamento, ou seja, se você aprender um deles não encontrará muitos problemas em utilizar seus concorrentes. Na perspectiva de possibilidade de trabalho, eu aconselharia você começar pela AWS, que tem o maior números de clientes hoje.

Material sugerido para AWS:
- [Curso online pago - Cloud Guru - AWS Certified Solutions Architect Associate](https://www.udemy.com/aws-certified-solutions-architect-associate/)
- [Canal Youtube Cloud Guru](https://www.youtube.com/channel/UCp8lLM2JP_1pv6E0NQ38pqw)

Material sugerido para Google Cloud:
- [Lab: Build a Continuous Deployment Pipeline with Jenkins and Kubernetes ](https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes)

##### Conhecimento de gerencia de configuração

Você precisará automatizar a configuração dos seus ativos (Servidores, switchs e afins), com isso algumas ferramentas precisam de alguma atenção no seu estudo. Você precisa de um nível acima do básico em ao menos uma delas. Eu aconselho experimentar todas descritas abaixo, mas escolha uma para especialização:

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

##### Conhecimento de containeres

Você precisará saber sobre containers e duas ferramentas dominam esse assunto, Docker e Kubernetes. Você precisa saber além do básico em ambos produtos.

Material sugerido para Docker:
- [Livro Containers com Docker - Daniel Romero ](https://www.casadocodigo.com.br/products/livro-docker)
- [Livro Aprendendo Docker - Wellington da Silva ](https://www.amazon.com.br/Aprendendo-Docker-Wellington-Figueira-Silva/dp/8575224867?tag=goog0ef-20&smid=A1ZZFT5FULY4LN&ascsubtag=65e71e4e-1464-4ddc-a0a5-491e02f24552)
- [Livro Descomplicando Docker - Jeferson Fernando Noronha](http://www.brasport.com.br/informatica-e-tecnologia/arquitetura-de-nuvem/descomplicando-o-docker/)
- [Livro Docker para desenvolvedores - Livro Código Aberto - Rafael Gomes](https://leanpub.com/dockerparadesenvolvedores)

Material sugerido para Kubernetes:
- [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- [Tradução do Kubernetes The Hard Way](https://github.com/cgbas/kubernetes-do-jeito-dificil)

### Fontes
- [Guto Carvalho](http://gutocarvalho.net/blog/2016/09/06/por-onde-iniciar-os-estudos-sobre-devops/)
- [Awesome DevOps-BR](https://github.com/devops-br/awesome-devops-br)
