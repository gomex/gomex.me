+++
title = "O que instalar em um Mac de alguém de infra"
date = "2020-01-02"
draft = false
Categories = ["devops", "mac"]
Tags = ["devops", "mac", "brew", "saas"]
+++

Pra começar bem o ano, resolvi fazer um backup e formatar meu Mac, reiniciar do zero, pois o meu notebook tem apenas 128GB de espaço em disco e ultimamente tenho brigado por cada último byte livre no HD.

Assim que acabei de formatar, lembrei o motivo de tanto retardo: A necessidade de reinstalar tudo que preciso do zero.

Fiz uma [postagem](https://twitter.com/gomex/status/1212585682650177537) no Twitter solicitando ajuda e prometi criar um artigo, e aqui está.

<!-- TOC -->

- [.1. Usando o Brew](#1-usando-o-brew)
- [.2. Aplicativos fora do Brew](#2-aplicativos-fora-do-brew)
    - [.2.1. Dash](#2-1-dash)
    - [.2.2. Amphetamine](#2-2-amphetamine)
    - [.2.3. Oh My ZSH](#2-3-oh-my-zsh)
- [.3. Aplicativos genéricos para se usar no Mac (Com Brew)](#3-aplicativos-genéricos-para-se-usar-no-mac-com-brew)
    - [.3.1. Alfred](#3-1-alfred)
    - [.3.2. Firefox e Chrome](#3-2-firefox-e-chrome)
    - [.3.3. Grammarly](#3-3-grammarly)
    - [.3.4. Iterm2](#3-4-iterm2)
    - [.3.5. Slack](#3-5-slack)
    - [.3.6. Spectacle](#3-6-spectacle)
    - [.3.7. Telegram](#3-7-telegram)
    - [.3.8. Vanilla](#3-8-vanilla)
    - [.3.9. Whatsapp](#3-9-whatsapp)
    - [.3.10. GPG Suite](#3-10-gpg-suite)
    - [.3.11. Keybase](#3-11-keybase)
    - [.3.12. Transmission](#3-12-transmission)
    - [.3.13. Spotify](#3-13-spotify)
    - [.3.14. Flycut](#3-14-flycut)
- [.4. Aplicativos para desenvolvedores (Com Brew)](#4-aplicativos-para-desenvolvedores-com-brew)
    - [.4.1. Visual Studio Code](#4-1-visual-studio-code)
    - [.4.2. Kubernetes-cli](#4-2-kubernetes-cli)
    - [.4.3. JQ](#4-3-jq)
    - [.4.4. Nmap](#4-4-nmap)
    - [.4.5. Openssl](#4-5-openssl)
    - [.4.6. Watch](#4-6-watch)
    - [.4.7. Docker](#4-7-docker)
    - [.4.8. Popeye](#4-8-popeye)
    - [.4.9. Stern](#4-9-stern)
    - [.4.10. Kubectx + Kubens](#4-10-kubectx--kubens)
    - [.4.11. Insomnia](#4-11-insomnia)
    - [.4.12. Docker Compose](#4-12-docker-compose)
- [.5. Agradecimentos](#5-agradecimentos)

<!-- /TOC -->

## .1. Usando o Brew

Todo mundo que tem Mac e usa ele para trabalhar com programação/devops/automação/afins já deve conhecer o brew. A instalação a é simples e pode ser vista [aqui](https://brew.sh/index_pt-br).

O que poucas pessoas sabem é sobre a existência do ```brew bundle``` (Aqui um [link](https://github.com/Homebrew/homebrew-bundle) para saber mais) basicamente ele faz o mesmo que o bundle do ruby faz. Reuni tudo que vc precisa instalar em um arquivo, que aqui é chamado de ```Brewfile``` onde você informa linha a linha, dizendo se é **brew** on **cask** para instalar os pacotes.

Ao final do arquivo pronto, basta um simples comando: 

```bash
brew bundle install
```

Ele instala tudo pra você. Segue meu [Brewfile](https://github.com/gomex/mac-setup/blob/master/Brewfile)

## .2. Aplicativos fora do Brew

Vou começar com os aplicativos que não estão no Brew, pois esses você precisará entrar no site e instalar manualmente mesmo. Seguindo a documentação de cada um deles.

### .2.1. Dash

 - Site: [https://kapeli.com/dash](https://kapeli.com/dash)
 - Valor: Grátis com limitações

É uma ferramenta muito interessante de documentação, tudo a partir de um campo de busca centralizado. A melhor parte dele é a possibilidade de pesquisar offline.

### .2.2. Amphetamine

 - Site: [https://apps.apple.com/us/app/amphetamine/id937984704?mt=12](https://apps.apple.com/us/app/amphetamine/id937984704?mt=12)
 - Valor: Grátis

Um aplicativo simples que pode ser usado para manter seu Mac ligado por um espaço específico de tempo. É ótimo para quem faz apresentações sem carregador e não quer que o Mac desligue no meio de apresentação, mas quer quer que esse comportamento volte depois da palestra.

### .2.3. Oh My ZSH

 - Site: [https://ohmyz.sh/](https://ohmyz.sh/)
 - Valor: Opensource

É um framework para gerenciamento de seu console Zsh, onde deixa ele mais bonito e bem mais produtivo, com vários atalhos e funcionalidades novas.

## .3. Aplicativos genéricos para se usar no Mac (Com Brew)

Vou passar rapidamente aplicativo por aplicativo para defender porque escolhi ele:

### .3.1. Alfred

 - Site: [https://www.alfredapp.com/](https://www.alfredapp.com/)
 - Valor: Grátis, com exceção do Powerpack

Esse é uma aposta, pois todo mundo fala, mas até então não tinha testado ainda. Tenho pouco a falar dele, que tem como objetivo substituir o spotlight (Aquela busca do CMD+Barra de espaço). Dizem que ele tem mais funcionalidades, é mais rápido e afins.

Eu ja tive muitos problemas com o spotlight, dai estou apostando no Alfred.

### .3.2. Firefox e Chrome

 - Site: [https://www.google.com/intl/pt-BR/chrome/](https://www.google.com/intl/pt-BR/chrome/) e [https://www.mozilla.org/pt-BR/firefox/new/](https://www.mozilla.org/pt-BR/firefox/new/)
 - Valor: Opensource (Firefox) e Grátis (Chrome)

Outra opção meio padrão para maioria das pessoas, pois o uso do navegador Safari é bem baixa ainda. Eu tenho maior uso do Chrome, pois ele tem uma funcionalidade de múltiplos perfis de conta por janela de navegador. Dai eu consigo gerenciar meu perfil pessoal e profissional alternando as janelas.

### .3.3. Grammarly

 - Site: [https://www.grammarly.com/](https://www.grammarly.com/)
 - Valor: Grátis com limites (Versão paga no plano anual sai USD$ 11,66 por mês)

Se você não é tão bom escrita de inglês quanto eu, e precisar escrever coisas com algumas constância, você deveria considerar um app como esse. Eu adoro o Grammarly, pois ele não verificar apenas erro de ortografia, ele verifica concordância e outros aspectos que um corretor comum não dá conta. Ele faz sugestões quando você repete demais a palavra e afins. Funciona bem. Eu aconselho.

### .3.4. Iterm2

 - Site: [https://iterm2.com/](https://iterm2.com/)
 - Valor: Opensource

Aqui temos outra unanimidade, pois a maioria esmagadora das pessoas que conhecem o Iterm2, acabam usando ele como padrão e esquecem o terminal padrão do Mac por completo.

### .3.5. Slack

 - Site: [https://slack.com/intl/pt-br/](https://slack.com/intl/pt-br/)
 - Valor: Grátis

A maioria esmagadora das empresas que já trabalhei usam o Slack como ferramente de comunicação interna, sendo assim fica difícil não instalar esse app. Ele consome MUITA memória, mas necessidade não se discute, né?

### .3.6. Spectacle

 - Site: [https://www.spectacleapp.com/](https://www.spectacleapp.com/)
 - Valor: Grátis

O Mac não tem teclas de atalho para gerenciamento da posição das janelas. As vezes você quer colocar a janela ocupando toda tela, mas não quer tela cheia, pois o Mac tem um comportamento diferente e deixa todas as outras telas em preto, ou seja, não da pra ficar no Mac sem um software que ofereça opções para  isso. O Spectacle é uma boa opção. Com o atalho ```control + CMD + F``` você faz com que a janela do software atual ocupe toda tela. 

Ao fazer esse post, descobri que ele foi descontinuado e sugere a utilização do [Rectangle](https://github.com/rxhanson/Rectangle)

Vale a pena olhar esse novo. Vou continuar com o Spectacle, pois ele me atende tranquilamente.

### .3.7. Telegram

 - Site: https://telegram.org/
 - Valor: Opensource

Uma ótima ferramente de troca de mensagens. No Brasil a maioria das comunidades técnicas estão nesse software de bate papo. A prova disso é esse extensa lista de grupos no telegram: [https://listatelegram.github.io/](https://listatelegram.github.io/)

Ele é praticamente igual ao Whatsapp, só que com mais funcionalidades, e sem necessidade de celular ligado para acessar versão desktop/web, por exemplo.

### .3.8. Vanilla

 - Site: https://matthewpalmer.net/vanilla/
 - Valor: Grátis com limites

Se você instala uma grande lista de apps e isso lota sua barra de notificação (Aquela que fica ao lado do relógio), esse app é pra você, pois ele oculta e mostra os apps com apenas um clique.

Simples e direto. Gostei.

### .3.9. Whatsapp

 - Site: [https://www.whatsapp.com/](https://www.whatsapp.com/)
 - Valor: Grátis

É praticamente um padrão de comunicação entre as pessoas no Brasil. Por mais que não curto esse mensageiro, sou obrigado a usá-lo por conta disso.

### .3.10. GPG Suite

 - Site: [https://gpgtools.org/](https://gpgtools.org/)
 - Valor: Opensource

Se você encripta seus emails, arquivos e afins, acredito que esse app é essencial pra você.

### .3.11. Keybase

 - Site: [https://keybase.io/](https://keybase.io/)
 - Valor: Opensource

Esse é uma outra aposta. Muita gente fala bem dele, mas confesso que ainda não usei de verdade. A ideia dele é oferecer uma plataforma segura para comunicação, troca de arquivos e afins.

### .3.12. Transmission

 - Site: [https://transmissionbt.com/](https://transmissionbt.com/)
 - Valor: Opensource

Um cliente para baixar arquivos via BitTorrent. Muito leve e bom.

### .3.13. Spotify

 - Site: [https://www.spotify.com/br/](https://www.spotify.com/br/)
 - Valor: Grátis com limites

Um cliente para uma plataforma de músicas online, que também oferece opção para Podcasts. Muito usado por boa parte das pessoas. 

### .3.14. Flycut

 - Site: [https://apps.apple.com/br/app/flycut-clipboard-manager/id442160987?mt=12](https://apps.apple.com/br/app/flycut-clipboard-manager/id442160987?mt=12)
 - Valor: Grátis

Outra aposta. O objetivo dessa ferramenta é oferecer uma opção mais potente de gerenciamento de área de transferência (O que você pega no ```CMD+C``` ).

## .4. Aplicativos para desenvolvedores (Com Brew)

### .4.1. Visual Studio Code

 - Site: [https://code.visualstudio.com/](https://code.visualstudio.com/)
 - Valor: Opensource

Esse editor de código tem crescido cada vez mais entre as pessoas que trabalham com infra/devops/automação. Acredito que por conta dos plugins/extensões que aumentam bastante a sua produtividade.

Prometo que depois faço um artigo apenas sobre as extensões do meu Visual Studio Code.

### .4.2. Kubernetes-cli

 - Site: [https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos)
 - Valor: Opensource

Se você trabalha com kubernetes de alguma forma, essa ferramenta é necessária. Ela é usada para se comunicar com o cluster kubernetes. 

### .4.3. JQ

 - Site: [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)
 - Valor: Opensource

Essa ferramenta é considerada o ```sed``` para formato JSON. É muito potente na coleta e manipulação de dados que contenham o formato JSON. 

Se quiser aprender um pouco como usar, veja esse [link](https://stedolan.github.io/jq/tutorial/)

### .4.4. Nmap

 - Site: [https://nmap.org/](https://nmap.org/)
 - Valor: Opensource

Ferramenta potente e extremamente extensível para descoberta e auditoria de segurança de rede. Muito bom para descobrir quais portas estão abertas no seu servidor e afins.

### .4.5. Openssl

 - Site: [https://www.openssl.org/](https://www.openssl.org/)
 - Valor: Opensource

Ferramenta necessária para gerenciamento e criação de certificados SSL/TLS. 

### .4.6. Watch

 - Site: [https://en.wikipedia.org/wiki/Watch_(Unix)](https://en.wikipedia.org/wiki/Watch_(Unix))
 - Valor: Opensource

Ferramente que é usada para mostrar repetidamente a mesma tela, atualizando automaticamente periodicamente com base em um espaço de tempo que você estipulou.

Exemplo:

Você quer saber se seu processo do PHP subiu corretamente, ou quantos processo tem aparecido em um espaço de tempo. Você pode usar esse comando: ```watch "ps -e | grep php"``` 

Por padrão o tempo de atualização é de 2 segundos, sendo assim ele executará o comando ```ps -e | grep php``` e mostrará na sua tela a saída do mesmo. Sendo assim você não precisa ficar dando o mesmo comando várias vezes, "sujando" seu histórico de comandos com vários comandos repetidos sucessivamente. 

### .4.7. Docker

 - Site: [https://docs.docker.com/docker-for-mac/](https://docs.docker.com/docker-for-mac/)
 - Valor: Opensource 

Esse eu preciso de pouco pra defender, correto? Ele é basicamente hoje a base da maioria das pessoas que trabalham com automação de infraestrutura. Eu mesmo uso ele para praticamente tudo. Eu não instalo o aws-cli, terraform, packer e afins, pois eu uso um container com esses binários dentro a partir do docker. Por exemplo:

```
docker run -it -v $PWD:/app -w /app --entrypoint="" terraform:light sh
```

Com esse comando eu tenho uma cli com terraform mais atualizado instalado com sucesso.

### .4.8. Popeye

 - Site: [https://github.com/derailed/popeye](https://github.com/derailed/popeye)
 - Valor: Opensource

Ferramenta interessante para scan de cluster kubernetes, que reporta possíveis problemas em recursos "deployados" e configurações. É uma aposta, dica do [@badtux_](https://twitter.com/badtux_).

### .4.9. Stern

 - Site: [https://github.com/wercker/stern](https://github.com/wercker/stern)
 - Valor: Opensource

Mais uma ferramente sugerida pelo [@badtux_](https://twitter.com/badtux_) para ajudar na gestão de logs de múltiplos pods em um cluster kubernetes.

### .4.10. Kubectx + Kubens

 - Site: [https://github.com/ahmetb/kubectx](https://github.com/ahmetb/kubectx)
 - Valor: Opensource

De novo o [@badtux_](https://twitter.com/badtux_) com sugestões massas para ajudar na gestão do kubernetes. Essas oferecem funcionalidades de rápida troca de namespace e cluster kubernetes . Outra aposta. Valeu [@badtux_](https://twitter.com/badtux_).

### .4.11. Insomnia

 - Site: [https://insomnia.rest/](https://insomnia.rest/)
 - Valor: Opensource

Esse foi amplamente pedido por várias pessoas em diversos canais (Obrigado [@@jabezerra](https://twitter.com/jabezerra) e [@malaquiasdev](https://twitter.com/malaquiasdev) por serem os primeiros). Resolvi testar e gostei. Está na lista, como uma boa aposta.

### .4.12. Docker Compose

- Site: [https://docs.docker.com/compose/](https://docs.docker.com/compose/)
- Valor: Opensource

Não menos importante, mas eu sempre esqueço dele e só vou instalar depois que preciso. Essa ferramente é responsável por gerenciar a execução de múltiplos containers com apenas um comando e um arquivo de configuração. É uma ótima ferramenta para iniciar infraestrutura complexa localmente na máquina de uma pessoa que desenvolve o produto.

## .5. Agradecimentos

 - Obrigado a todo mundo que ajudou nessa [thread](https://twitter.com/gomex/status/1212585682650177537).
 - Obrigado a [@somatorio](https://twitter.com/Somatorio) que sempre revisa meus trabalhos ;) <3
 - Obrigado a [@jjunior0x2A](https://twitter.com/jjunior0x2A) que deu uns pitacos também.
 - Obrigado a [@badtux_](https://twitter.com/badtux_) que colocou bastante coisa de Kubernetes na lista.
 - Obrigado a [@@jabezerra](https://twitter.com/jabezerra) e [@malaquiasdev](https://twitter.com/malaquiasdev) pelo lembrete do Insomnia.
 