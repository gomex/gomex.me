+++
title = "O que é Deploy"
date = "2020-07-17"
draft = false
Categories = ["portugues", "pipeline"]
Tags = ["portugues", "deploy", "devops"]
+++

## Contextualização

Esse artigo segue na série sobre ["Deploy em produção para desenvolvedores?"](https://gomex.me/categories/pipeline/), que tem como objetivo apresentar as melhores práticas para entregar em produção os produtos.

Nesse artigo falaremos sobre o que é deploy e quais ambientes normalmente estão envolvidos nesse processo.

## O que é deploy?

Muito se fala sobre deploy e acredito que a maioria das pessoas que já estão na área de TI há algum tempo talvez já tenham algum entendimento sobre o que é isso e quem começou já pegou "alguma coisa" pelo contexto.

Essa é a oportunidade para tentarmos estabelecer aqui um entendimento sobre o que é deploy, como ele funciona e porque ele é tão importante para área de TI.

Pra começar, deploy é um verbo do idioma inglês, que a tradução mais próxima talvez seria posicionar. 

Tentando em vista que quando se falar em fazer deploy, imagine que é uma forma de posicionar algo, ou seja, é basicamente pegar algo que está em uma posição/localização e colocar em outra.

Isso quer dizer que quando alguém falar que vai "deployar" algo, é basicamente o nosso jeitinho de usar uma palavra em inglês e verbalizar ela em português, o que não é de todo ruim, uma vez que quem escuta entende perfeitamente, ou seja, a comunicação funciona e não há problema algum com isso. Chato mesmo é alguém que complica a comunicação para forçar palavras não usuais e assim demonstrar uma certa superioridade linguística. Aconselho a leitura [desse livro](https://www.amazon.com.br/Preconceito-Lingu%C3%ADstico-Marcos-Bagno/dp/8579340985/ref=asc_df_8579340985/?tag=googleshopp00-20&linkCode=df0&hvadid=379708411098&hvpos=&hvnetw=g&hvrand=890780466304631420&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1001533&hvtargid=pla-387685959250&psc=1) sobre preconceito linguístico.

### Como funciona o deploy de um produto de software

O que é dito aqui como "produto de software" é qualquer conjunto de arquivos que tenha como objetivo entregar uma funcionalidade como produto final. Um exemplo seria um site, que pelo conjunto de arquivos html, css e javascript, é entregue uma exibição de informações que é traduzida pelo seu navegador e assim você pode ter acesso a informações navegando na internet.

O deploy é o ato de pegar esse conjunto de arquivos e levar até um determinado lugar. Esse lugar é normalmente um servidor, que hospedará essa software e exibirá para o usuário sempre que solicitado.

### Para onde se faz deploy? 

Normalmente um software passa por alguns destinos antes de chegar no seu habitat final, que é produção. O que se chama de produção é basicamente o lugar oficial, de onde os usuários finais deste produto poderão acessar.

As boas práticas apontam que antes de chegar em produção esse software passe por outros ambientes, que normalmente são esses, e muitas vezes seguem também nessa ordem:

 - Desenvolvimento
 - Teste/Staging
 - Produção

#### Desenvolvimento

É o local onde a pessoa que desenvolve tem um acesso direto, é onde se executa rapidamente o código, afim de verificar se o que está sendo escrito atenderá as expectativas da funcionalidade que está sendo desenvolvida

Normalmente essa é a máquina da pessoa que desenvolve e o verbo "deployar" faz pouco sentido aqui, mas na prática quando mesmo localmente colocar o código para ser testado seria o equivalente a fazer "deploy em dev"

Em alguns casos a infraestrutura necessária para simular o ambiente de produção é tão complexa que se faz necessário um ambiente de desenvolvimento fora da máquina local, dessa forma faz o verbo "deployar" ter seu sentido completo novamente.

#### Teste/Staging

Não existe um nome para esse ambiente que possa ser entendido como unânime, mas esse é o ambiente onde se espera que o software esteja mais maduro, isso quer dizer que aqui o código já passou por alguma análise e está pronto para ser validado por outras pessoas.

O que aqui é chamado de **análise** será detalhado um pouco mais nos capítulos posteriores, mas por hora basta saber que esse é o processo usado para avaliar se há algum problema no código, normalmente de forma manual e feito por uma outra pessoa que olha seu código afim de encontrar possíveis erros.

Esse é normalmente o último local que o código "visitará" antes de ser conduzido para ambiente que será usado pelos usuários reais do serviço. Isso quer dizer que é aqui onde costumeiramente acontece os testes mais "pesados".

Esses testes muitas vezes usam dados mais próximos do que seria no ambiente real, e validações mais bem elaboradas acontecem. Habitualmente times de software simulam o uso sistema, de forma automatizada ou não, afim de encontrar possíveis erros. Detalharemos esses tipos de testes posteriormente. Agora é suficiente saber que é normalmente nesse ambiente que isso acontece.

É importante salientar que se você copiar os dados de produção para ajudar na validação dos ambiente de "teste" e/ou desenvolvimento, lembre-se de apagar dados pessoais das pessoas que utilizam seu sistema. Imagine se fosse você a pessoa que utiliza um sistema, sabendo que seu dado pessoal está acessível para qualquer membro da equipe de desenvolvimento?

#### Produção

Aqui é oficial, todo produto agora pode ser utilizado pelos clientes. Normalmente esse ambiente tem um cuidado maior, tanto de quem pode fazer o deploy, como na disposição da quantidade de recurso. Muitas vezes o ambiente de produção tem ao menos duas máquinas, que carregam o mesmo contéudo, isso permite criar uma situação que chamamos de alta-disponibilidade. É quando o serviço permite que uma máquina seja perdida por falhas não esperadas, sem que isso afete a disponibilidade do serviço.

Usando o exemplo anterior do site, basicamente seria ter duas máquinas hospedando os mesmos arquivos do site, e caso por falha elétrica, ou qualquer outro problema, aconteça em uma das máquinas, a segunda pode assumir o serviço sozinha sem muitos prejuízos a disponibilidade do serviço ofertado, que nesse caso aqui é exibir o site para as pessoas que acessam.

### Na prática, como funciona o deploy?

Normalmente, o ato de fazer deploy se resume às ações de copiar os arquivos de um lugar, que pode ser o repositório de código ou de artefato e depositar ele no ambiente de destino. 

Seguindo o exemplo anterior do site, seria copiar os arquivos html, css e javascript que estão no repositório de código e depositar ele no servidor que hospedará aquele ambiente.

Deploy para testes? Copiar os arquivos do site e depositá-los no servidor que foi designado para ser ambiente de teste.

## Conclusão

O processo de deploy pode parecer simples, mas entender de fato como ele funciona é essencial para acompanhar tudo que é apresentado por aqui.

## Agradecimentos

Obrigado a [Somatório](https://twitter.com/somatorio) que, como sempre, revisou esse material antes dele sair.

Escrevi esse artigo ouvindo:

- Yung Buda
- My Chemical Romance
- Projota
- Beatles
- Outras músicas do meu Daily Mix do Spotify

