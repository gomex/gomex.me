+++
title = "O que é pipeline"
date = "2026-04-04"
draft = false
Categories = ["portugues", "pipeline"]
Tags = ["portugues", "pipeline as code", "pipeline", "devops", "qa"]
+++

## Contextualização

Muitas pessoas ainda não estão seguras sobre a diferença entre um código de software e a infraestrutura que o sustenta. Esses conceitos são os pilares da tecnologia, mas nem sempre ficam claros para quem está começando.

Este artigo apresenta um resumo prático para você entender, de uma vez por todas, o que é cada um e como eles trabalham juntos.

## Escrevendo código

O código de software é, na essência, um arquivo de texto escrito em um formato e idioma específicos (a linguagem de programação). Esse arquivo funciona como um manual de instruções que será lido por outro software preparado para entendê-lo.

Exemplo Prático: Conversor de Moedas

Imagine que você deseja criar um software que recebe um valor em dólares e retorna o equivalente em reais. No exemplo abaixo, usamos a linguagem Go.

Dica: Não se preocupe com a sintaxe agora; foque apenas na ideia central.

```
package main

import (
	"fmt"
)

func main() {
	// Definimos uma taxa de câmbio fixa para o exemplo
	// Em um cenário real, você buscaria isso externamente
             // Buscar externamente fica pra um próximo artigo
	const taxaDolarHoje = 5.15 

	var dolares float64

	fmt.Print("Digite a quantidade de dólares (USD): ")
	fmt.Scan(&dolares)

	reais := dolares * taxaDolarHoje

	fmt.Printf("O valor de $%.2f convertido para Reais é: R$%.2f\n", dolares, reais)
}
```

Ao salvar esse texto e "executá-lo", o computador traduz essas instruções para a **linguagem de máquina**, que é a única que o computador entende.

## Como o código é executado?

Para entender a execução, precisamos olhar para três componentes fundamentais: **Sistema Operacional (SO)**, **Processos** e **CPU**.

### Sistema operacional, processos e CPU

De forma resumida, o **Sistema Operacional** é o intermediário entre seus aplicativos e a máquina física.

  - Processos: Quando você executa seu código (ou abre o Chrome/Firefox), ele se torna um processo rodando no sistema operacional.
  - A CPU (Processador): É a parte do computador que faz o "trabalho pesado" de processamento.

É importante salientar que o software usado como exemplo aqui e seu navegador (Firefox ou Chrome) são a mesma coisa. Ambos são aplicativos que executam na forma de processos no sistema operacional. Ambos se comunicam com o sistema operacional para usar os recursos de infraestrutura da máquina.

![Sistema operacional, processos e CPU](/img/codigo_infra_1.png)

Ou seja, o código que você escreveu executará como processo no sistema operacional, que utiliza uma máquina física para realizar suas atividades. 

```
const taxaDolarHoje = 5.15 
…
reais := dolares * taxaDolarHoje
```

Quando o código diz **reais := dolares * taxaDolarHoje**, ele está pedindo ao Sistema Operacional: "Por favor, peça ao processador para multiplicar estes dois números". O processador faz a conta e devolve o resultado para o seu programa exibir na tela.

## E a infraestrutura?

É basicamente tudo relacionado à máquina onde esse processo está sendo executado. É importante perceber que a infraestrutura não é apenas a máquina, pois os equipamentos responsáveis por fazer os dados chegarem até a máquina são igualmente importantes, pois caso contrário a máquina não poderia ser acessada remotamente.

Em um cenário simplista, apenas pra facilitar o entendimento mais básico do que seria essa infraestrutura, é correto afirmar que em ambientes maduros o que normalmente se encontra é o seguinte:

![Infraestrutura](/img/codigo_infra_2.png)

O firewall é o equipamento responsável por dizer quem pode acessar o seu processo, ele vai bloquear, baseado em regras predefinidas, qual tráfego pode passar. Se o seu processo quiser ser acessado externamente, você deve liberar no firewall a porta que ele usa para comunicação remota.

O roteador é o equipamento normalmente responsável por conectar a rede externa com a sua rede interna, onde estarão as máquinas dentro da sua rede. 

O processo em execução está sendo executado em uma máquina e ele às vezes precisa acessar um dado, que pode estar em um storage remoto ou banco de dados.

O banco de dados é um software rodando em outra máquina, um processo, igual ao seu código em execução, mas é um software feito para armazenar informações de forma profissional, assim como o storage, mas cada um tem um propósito específico que abordaremos em outros conteúdos, para não tornar esse texto mais complexo.


## Conclusão

Podemos resumir a relação de uma forma simples:

 - O Código é a inteligência, o conjunto de instruções e a lógica do negócio
 - A Infraestrutura é o corpo, os músculos e as estradas que permitem que essa inteligência funcione e seja acessível ao mundo.

Sem código, a infraestrutura é um monte de ferro parado. Sem infraestrutura, o código é apenas um texto sem utilidade. 

### Agradecimentos

 - Obrigado a [Somatório](https://twitter.com/somatorio) que revisou a primeira versão deste material antes dele sair e fez o mesmo com essa nova versão. Obrigado demais!
 - Obrigado a [Italo Valcy](https://www.linkedin.com/in/italo-valcy-883a1a18a) que leu o artigo e me deu ótimos feedbacks para melhora do texto.