+++
title = "Distroless! Pense mais em sua aplicação e menos na distribuição"
date = "2018-08-04"
draft = false
Categories = ["portugues", "devops", "docker", "scratch"]
Tags = ["portugues", "devops", "docker", ""scratch", "distroless"]
+++

## TL;DR

Apresentarei um conceito novo de focar na aplicação e suas dependências na construção de imagens Docker, falando sobre problemas com imagens grandes, superfície de ataque e como usar *Multi stage build* e a imagem scratch para resolver esse problema.

### Contextualização

Antes do advento dos containers pela maioria das pessoas, o modelo mais utilizado era baseado em **máquinas virtuais**. Que basicamente eram instâncias com um sistema operacional instalado, bibliotecas compartilhadas, softwares de acesso remoto (Ex. SSH), agentes em geral (Monitoramento, geranciamento de log e afins) e por fim sua aplicação (A coisa mais importante desse *setup* inteiro). 

A equipe de desenvolvimento, normalmente composta por várias pessoas, mantinham a versão da aplicação instalada nessa máquina e todo restante ficava a cargo do time de suporte, ou seja, aproximadamente 90% dos softwares instalados em cada maquina desse modelo estava a cargo de um time menor (suporte geralmente era menor na maioria das empresas).

Com a chegada do Docker tivemos a grande oportunidade de minimizar a necessidade de uma infra completa para suportar nossa aplicação. Era uma oportunidade do controle da infraesturutra mais próxima da aplicação para quem estava lidando com o código, os desenvolvedores nesse caso, pois com o Docker você poderia apenas instalar os pacotes necessários para sua aplicação e iniciar o processo de forma isolada em uma arquitetura isolada a nível de sistema operacional. Infelizmente não foi isso que aconteceu, ao menos não na maioria dos lugares.

### Problema

Em muitas situações o docker ainda é usado da mesma forma que se fazia com máquinas virtuais. Qual a consequência disso? Imagens enormes! E veja que o maior impacto não é apenas no custo com armazenamento de dados e sim em dois outros importantes pontos:

 - **Gerência**: Imagens grandes normalmente tem diversos pacotes instalados, arquivos de configuração que precisam ser modificados para mudar comportamento da imagem e afins. Dessa formas as pessoas responsáveis por essas imagens tem mais trabalho toda vez que precisam alterar alguma coisa na imagem.
 - **Segurança**: Quanto maior a sua imagem normalmente maior o número de pacotes instalados, dessa forma mais softwares para atualizar, maior superfície de ataque, que no resumo aumenta a possibilidade da sua aplicação ser comprometida.

### Proposta

A google apresentou um conceito interessante chamado **distroless**, eu achei bem relevante e foi o motivador para eu escrever esse artigo, mas eu precisava acrescentar mais exemplos e usar recursos mais novos do Docker.

O conceito **distroless** reside no fato de você pensar menos na distribuição (GNU/Linux) e focar na sua aplicação. É lembrar que o container Docker não é uma máquina mais leve e sim um processo isolado em execução. É reduzir ao máximo o que está sendo adicionado em sua imagem. É a ideia de colocar o mínimo possível.

### Exemplo

Usaremos esse código Go como exemplo:

```
package main

import (
    "fmt"
    "io/ioutil"
    "net/http"
    "os"
)

func main() {
    resp, err := http.Get("https://google.com")
    check(err)
    body, err := ioutil.ReadAll(resp.Body)
    check(err)
    fmt.Println(len(body))
}

func check(err error) {
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

Esse código basicamente baixa a página inicial do google e retorna a quantidade de linhas.

Como é que normalmente as pessoas constroem uma imagem Docker pra esse código? Utiliza a imagem base **golang** certo?

```
FROM golang:1.10.3 
RUN mkdir /app 
ADD . /app/ 
WORKDIR /app 
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main . 
CMD ["/app/main"]
```

#### Imagem golang:1.10.3 em detalhes

Vamos começar pelo seu consumo de espaço em disco:

```                                               
golang                    1.10.3              d0e7a411e3da        2 weeks ago         794MB
```

A imagem com seu código terá 800MB, ou seja, seu código ocupa apenas 6MB e você carregará todo restante do peso contigo ao utilizar essa imagem.

Outro detalhe importante é a quantidade de pacotes instalados, que é 189 softwares, ou seja, são 189 versões pra se preocupar e atualizar quando sair um pacote de atualização ou nova medida de segurança.

E pra finalizar temos 27098 arquivos nessa imagem.

Vale lembrar que essa imagem contém tudo que você precisa pra *buildar* seu código go, e normalmente imagens de build são relativamente grandes mesmo.

### Usando Multi Stage Build

Essa feature foi adicionada no Docker na versão 17.05. Ela tem como objetivo possibilitar que você use multíplas imagens base, cada uma para seu propósito e no final possa usar uma imagem mais enxuta para executar seu serviço após as etapas de build.

Seguindo exemplo do código go, o nosso Dockerfile seria escrito desse jeito:

```
FROM golang:1.10.3 as builder
RUN mkdir /app 
ADD . /app/ 
WORKDIR /app 
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:3.8
COPY --from=builder /app/main /app/main
RUN apk --no-cache add ca-certificates
CMD ["/app/main"]
```

Muita atenção para a instrução **COPY --from=builder /app/main /app/main** esse parâmetro **--from** informa de onde será obtido o arquivo, perceba que na primeira instrução **FROM** desse Dockerfile, temos um adento **as builder** que é responsável por fornecer um "apelido" para essa etapa do build. Quando você informa **COPY --from=builder /app/main /app/main** você quer dizer que pegue o arquivo **/app/main** dessa etapa e coloque em **/app/main** da atual.

#### Imagem alpine:3.8 em detalhes

Vamos começar pelo seu consumo de espaço em disco:

```                                               
alpine                    3.8                 11cd0b38bc3c        4 weeks ago          4.41MB
```

A imagem com seu código terá 10.5MB. É bem melhor do que os 800MB da imagem **golang**.

Com relação quantidade de pacotes instalados, temos 14 softwares, ou seja, multi melhor que os 189 do **golang**, mas ainda temos 14 pacotes pra se preocupar e atualizar quando sair um pacote de atualização ou nova medida de segurança.

E pra finalizar temos 478 arquivos nessa imagem.

### Usando a imagem scratch

Essa imagem é basicamente vazia, isso mesmo, sem nenhuma camada de dados extra. Você deve estar se perguntando como uma imagem dessa funcionaria sem resolução de nomes (/etc/hosts e afins), dev, proc e sys? De acordo com a [especificação](https://github.com/opencontainers/runc/blob/master/libcontainer/SPEC.md#filesystem) usada pela Docker atualmente existem algumas parte do seu sistema de arquivo que são montada automaticamente para todos os containers.

Seguindo exemplo do código go, o nosso Dockerfile seria escrito desse jeito:

```
FROM golang:1.10.3 as builder
RUN mkdir /app 
ADD . /app/ 
WORKDIR /app 
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM scratch
COPY --from=builder /app/main .
CMD ["/main"]
```

Aqui encontraremos nosso primeiro problema ao utilizar **scratch**:

```
Get https://google.com: x509: failed to load system roots and no roots provided
```

Basicamente nosso container precisa de um arquivo que contém todos os certificados de autoridade certificadora da internet (CA). No exemplo usando alpine resolvemos isso com a linha abaixo:

```
RUN apk --no-cache add ca-certificates
```

No **scratch** não temos sistema de pacote, sendo assim a forma possível para resolver esse problema é usando **Multi stage build** novamente:

```
FROM golang:1.10.3 as builder
RUN mkdir /app 
ADD . /app/ 
WORKDIR /app 
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:3.8 as certs
RUN apk --update add ca-certificates

FROM scratch
COPY --from=builder /app/main .
COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
CMD ["/main"] 
```

O que fizemos aqui foi o uso do melhor de cada etapa do build, retirando os arquivos que interessam de cada uma delas e colocando na imagem final, que será a utilizada na inicialização do serviço em produção.

A imagem é do tamanho do código, nesse caso 4MB. A quantidade de arquivos mesma situação.

0 pacotes para se preocupar com atualização e gerência de arquivos de apoio. Sua aplicação é sua única preocupação nesse caso.

### Considerações finais

É evidente que nem toda linguagem funcionará nesses moldes facilmente e talvez a quantidade de horas gastas nesse processo de desenvolimento da "melhor imagem" sejam tão alta que não justifique o processo. É muito claro que esse argumento nem sempre é uma desculpa de quem não quer seguir as melhores práticas, as vezes não vale a pena para o negócio de mudança, mas meu objetivo nesse artigo é apresentar um possível "norte" para onde sua aplicação poderia "mirar" toda vez que fosse refatorada.

Eu utilizei o [archore](https://hub.docker.com/r/anchore/anchore-engine/) para fazer as contagens de pacotes e arquivos nas imagems.

### Agradecimentos

[Pery Lenke](http://) que acordou mais cedo num dia de sábado com minha ligação pra perguntar sobre o problema dele com scratch com go.

### Fontes

- [Distroless](https://github.com/GoogleContainerTools/distroless)
- [Building Minimal Docker Containers for Go Applications](https://blog.codeship.com/building-minimal-docker-containers-for-go-applications/)
- [Inside Docker's "FROM scratch"](https://embano1.github.io/post/scratch/)
