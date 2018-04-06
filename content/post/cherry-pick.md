+++
title = "Fazendo merge de apenas uma pasta entre duas branches no git"
date = "2018-04-05"
draft = false
Categories = ["portugues", "devops", "carreira"]
Tags = ["portugues", "devops", "carreira"]
+++

## TL;DR

Um dia precisei fazer merge entre duas branchs, mas apenas dos dados de uma pasta específica. Nesse artigo irei demonstrar como usar o "git cherry-pick" para resolver esse problema.

### Problema

Estava fazendo um trabalho extra e me deparei com um projeto usando incorretamente as branchs do repositório. 

Na verdade, eu estava usando uma branch, mas o pipeline foi modificado no meio do projeto, sem me informar, para usar uma outra branch, ou seja, eu tinha bastante trabalho não sendo aproveitado pelo pipeline e entregando a imagem errada para deploy.

### Solução rápida

Entre as opções para solução eu tinha a possibilidade de copiar a única pasta que eu trabalhei nesse repositório e então enviar um grande commit na branch correta, mas com isso eu perderia todo log do git com relação as modificações, certo?

# Solução ideal

A melhor opção era mover apenas os commits que eu enviei, certo? Como eu trabalhei em apenas uma pasta, isso ficou mais fácil.

Na branch de origem (onde meu trabalho estar hoje) executei o comando abaixo e assim foi possível obter a lista de commits da pasta em questão:

```
git log --oneline --decorate --color --graph pasta_escolhida/
```

Com posse dos identificadores de cada commit (Ex. f7d62e1) eu fiz uma lista e passei para a branch de destino (para onde meus commits devem ser migrados):

```
git checkout branch_destino
```

Agora vamos a cereja do bolo :P

Usando o comando abaixo é possível importar para a branch que você está no momento o commit com o identificador informado. Ele criará um outro commit, com outro identificador na sua branch de destino, mas com o mesmo conteúdo:

```
git cherry-pick f7d62e1
```

Execute o comando abaixo para validar se seu commit foi importado:

```
git log --oneline --decorate --color --graph pasta_escolhida/
```

Existe a possibilidade de algum conflito nessa importação, mas após resolvido basta apenas você dar o comando abaixo para continuar:

```
git cherry-pick --continue
```

Lembre-se sempre de validar se o commit foi importado:

```
git log --oneline --decorate --color --graph pasta_escolhida/
```

Depois de migrar todos os commits, basta apenas fazer o push normalmente.

```
git push
```

Pronto! 

### Agradecimentos

A todos do canal [Hora Extra Salvador](http://horaextra.org) que colaboram em apresentar opções e especialmente para [Massimo Rangoni](https://stackoverflow.com/users/1375596/manzapanza) que teve total paciência pra me ajudar nessa.

### Fontes

- [Git cherry-pick](https://git-scm.com/docs/git-cherry-pick)
