+++
title = "Construindo imagems docker no pipeline Jenkins"
date = "2019-01-20"
draft = false
Categories = ["portugues", "devops", "docker"]
Tags = ["portugues", "devops", "docker", "melhores-praticas"]
+++

## TL;DR

Nesse artigo apresentaremos passo a passo como configurar um pipeline para construção de imagens docker, seguindo as melhores práticas em relação a pipelines no Jenkins e imagens no Docker. Trataremos desde a análise de código Dockerfile até a publicação em repositório de imagens docker (Docker hub).

### Requisitos

 - Subindo um Jenkins server
 - Configurando integração com Github
 - Conhecimento básico de Jenkisfile

### Contexto

Antes de iniciarmos a demonstração de código e outras coisas mais técnicas, vamos primeiro entender o que está sendo feito aqui.

As imagens Docker tem se tornado um padrão como artefatos na construção de aplicações, principalmente software web modernos, mas ainda é comum encontrar pipelines de integração/entrega contínua usando Docker apenas como agente para build, teste e outras atividades operacionais apenas, ou seja, a maioria das pessoas usam apenas para subir um container, executar comandos a partir dele e pronto, a imagem não é usada como artefato final, é apenas uma ferramenta no meio do processo para facilitar o uso de agentes da ferramenta de CI/CD preferida.

O uso de imagens Docker pode ter um resultado muito melhor, ou seja, ao invés do seu pipeline ter como produto final um jar, war ou afins, o artefato pode ser uma imagem Docker, devidamente aplicada tag e armazenada em repositório adequado. Usando o Docker como produto final, temos o ambiente inteiro (software e "infra") em uma "foto" que pode ser reproduzido em qualquer lugar sem grandes problemas ou necessidade de pré-configuração complexa.

http://collabnix.com/wp-content/uploads/2018/03/ci-cd.png

### Usando Jenkinsfile

Usaremos a interface web para apenas o necessário, isso quer dizer que usaremos o Jenkinsfile para construir nosso *pipeline as code*. Isso quer dizer que toda nossa configuração do pipeline será armazenada na raiz do repositório da aplicação.

Para quem não conhece Jenkinsfile, eu aconselho esse [artigo]().

#### Criando um pipeline

Acesse a interface do Jenkins, escolha o local onde deseja criar seu pipeline e clique em **New Item**. Escolha a opção **multibranch pipeline**.

### Jenkinsfile

Vou inicialmente apresentar o jenkinsfile inteiro, mas não precisa se preocupar que explicarei ponto a ponto.



## Fontes

https://jenkins.io/doc/book/pipeline/docker/#
https://opensourceforu.com/2018/05/integration-of-a-simple-docker-workflow-with-jenkins-pipeline/
http://localhost/pipeline-syntax/globals