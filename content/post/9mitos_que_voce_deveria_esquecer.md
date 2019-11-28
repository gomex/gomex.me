+++
title = "9 mitos que você deveria esquercer"
date = "2019-11-28"
draft = false
Categories = ["devops"]
Tags = ["devops"]
+++

Fui surpreendido essa semana com uma [matéria](https://cio.com.br/9-segredos-obscuros-sobre-devops-que-voce-precisa-saber/), de uma revista super conceituada, sobre "9 segredos obscuros sobre DevOps que você precisa saber".

Me marcaram no twitter, pedindo minha opinião, e como minha opinião era bem longa, e precisava de um contexto, eu resolvi escrevi um artigo aqui.

Eu não quero de forma alguma desrespeitar quem escreveu a materia, e nem criar nenhum constrangimento para a revista em sí, mas eu preciso me posicionar com relação a isso, pois o contéudo da matéria propaga uma senso comum que é bem ruim para quem trabalha com isso, e tem que lidar com informações desencontradas no mercado.

## 1. DevOps não programa

Aqui começa o desserviço desse artigo, pois a todo momento a comunidade da cultura DevOps tem tentado convencer as pessoas que migram para cargos dessa cultura DevOps, sobre a necessidade do conhecimento sobre desenvolvimento, ou seja, sobre programar de fato.

Não caia nessa armadilha. É muito cômodo se fechar em uma posição confortável e apontar que seu cargo não envolve desenvolvimento, pois não é desenvolvedor, mas lhe digo que a linha entre as responsabilidades e possibilidades são bem diferentes.

Um "Sysadmin" que entende de fato o processo de desenvolvimento, consegue olhar para um código e apontar um problema, que pode impactar na infraestrutura agrega muito mais para o negócio do que aquela pessoa que antigamente era considerada apenas como custo operacional, para suportar a aplicação no ar, onde na maioria esmagadora dos casos a medida do sucesso era apenas SLA (Service Level Agreement).

Não estou aqui defendendo que você seja responsável por enviar códigos e nem ser revisor de fato de código alheio, mas a possibilidade de fazer isso lhe coloca em outro patamar enquanto profissional.

## 2. Supervisão de outros programadores

Nesse "item" ele inicia citando a falta de necessidade de o "DevOps" em não programar, mas mesmo assim era o seu papel supervisionar o trabalho de envio de código. Isso é uma contradição, pois como alguém seria responsável por dizer o que entra ou não no "container" (como se isso fosse uma caixa, e o "DevOps" aqui seria o segurança portão que permite entrada ou não).

O fato é: Ninguém sozinho deveria ser responsável por dizer o que entra ou não "no container", mas o ideal é que o time entenda as demandas, na escrita do código, faça uso do melhor algorítimo para escrita da lógica ideal, afim de atender a demanda do negócio para aquele código que se prentende ser colocado em produção. Falo pretende, porquê o esperado é que exista um processo de revisão de código, que pode ser feito por outros desenvolvedores, QA (Quality Assurance) ou até mesmo a pessoa responsável pela automação de infra ("DevOps"?).

Não queira ser o gargalo do time, não almeje esse poder, pois isso é também uma prisão, onde você raramente poderá tirar férias, descansar no final de semana, estar com seu filho em algumas noites no meio da semana.

Permita que seu time sobreviva sem você, pois isso pode significar sua hora extra na empresa. Isso não é apenas garantia de trabalho (não ser demitido), isso é quase a certeza que qualquer movimento do time, independentemente se envolver mudanças na infra ou não, você precisará ser envolvido.

## 3. DevOps está assumindo o controle

Eu acho falsa a premissa que os desenvolvedores tinha o "controle" antes do microsserviço. Essa inclusive é uma das dores primordiais que estimulou a criação da cultura DevOps. "O controle" na maioria das empresas esteva na mão dos Sysadmin, que era o time responsável pelo que é colocado em produção.

A cultura DevOps não estimula que esse "controle" mude de lado e agora um outro time assuma essa responsabilidade. A premissa é **colaboração**, ou seja, o "controle" foi distribuído em partes menores e agora todos são corresponsáveis.

Se você pretende trabalhar com cultura DevOps, toda vez que seus movimentos tiverem como objetivo concentrar em você o "controle", você além de estar desvirtuando a idéia inicial da cultura, você estará arrumando um grande problema pra ti, e terá pouco tempo pra fazer as coisas que as empresas normalmente esperarão de você (Ex. Monitoramento de negócio, melhorias na arquitetura e afins).

## 4. Redução de custos

Isso até poderia ser algo que não mereceria nenhuma crítica a matéria, ao menos que no mesmo texto não fosse mencionado o baixo envolvimento do "devops" com o código, ou seja, não é possível ser efetivo na redução de custos quando seu envolvimento com código é baixo. Para reduzir custos, de verdade, é necessário um envolvimento que alguém que está distante do código raramente terá.

É possível reduzir custos olhando só pra infraestrutura? Claro que sim, mas a real redução demanda de um envolvimento com o código. Não tem pra onde fugir.

## 5. Aumento de desempenho

Estaria aqui a possível redenção do artigo, mas infelizmente falha no alvo. Poderia entrar aqui o papel desse profissional "devops" no auxilio ao time de desenvolvedores sobre a perspectiva de infra no uso dos recursos de cloud, que tem bastante possibilidades de composições que impactam muito no custo e desempenho.

O profissional "devops" aqui atuaria como consultor interno, junto ao time de desenvolvimento, para que a melhor solução fosse adotada, analisando a demanda na perspectiva de infraestrutura, desenvolvimento, qualidade, negócio, produtividade, segurança e afins.

## 6. Demolição

O "encobrindo" citado na matéria só se torna um problema em ambientes que atuam no paradigma anterior, onde o monitoramento lida apenas com máquinas, onde o log raramente tem tags ou é centralizado, tudo precisa ser analisando em console.

Se um container reiniciou, qual problema com isso? Não tem log? Monitoramento? A ideia não é exatamente essa? Que o container reinicie em caso de problemas?

A minha impressão é que esse tipo de indagação sobre "encobrimento" é típico de quem usa container como máquinas, infelizmente.

## 7. Bancos de dados

Felizmente banco de dados não é mais "a única fonte da verdade", temos outras opções, que podem armazenar as interações com dados e até mesmo enviar novamente eventos numa fila (Ex. Kafka).

A verdade é que Banco de Dados ainda é um grande calo na maioria dos times, de fato.

## 8. Como o código está sendo executado

Aqui fica evidente o problema citado em outros pontos da matéria. A separação funcional entre os dois times sempre foi um dos maiores problemas que a cultura DevOps pretende resolver, ou seja, não deveria existir isso de "Essa questão é deixada para os programadores". Entender os números é uma tarefa do time! Claro que cada um com sua especialidade, mas todos trabalhando juntos para entender e atuar.

Não deixe que seja criado um muro imaginável entre os times. É uma demanda de todos. Não sabe ler o log, senta com alguém que sabe e aprende, pois isso pode ser importante na perspectiva de infra e você nem se atentou a isso. O inverso também pode acontecer.

## 9. Um pouco de mistério

Eu entendo o quanto é tentador colocar os incidentes na conta do azar, mas acredite, tudo tem uma causa raiz. O que pode acontecer é que talvez agora não tenhamos as ferramentas e conhecimentos necessários para descobrir isso, mas que ela existe, não há nenhuma dúvidas sobre isso.

Não deixe "o mistério acontecer", tenha sede pelo entendimento das tecnologias, não apenas nos seus parâmetros, mas no funcionamento mais elemental das mesmas. Evite esse papo de "muitas vezes é mais simples seguir em frente", pare um pouco, aprenda com seus incidentes (e dos outros também), pois eles tem muito para lhe ensinar e evitar que novos problemas aconteçam.