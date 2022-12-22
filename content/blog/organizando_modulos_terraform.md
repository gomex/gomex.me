+++
title = "Terraform como produto - Organizando os módulos do terraform"
date = "2022-12-21"
draft = false
Categories = ["iac", "terraform"]
Tags = ["iac", "terraform"]
+++

# Introdução

Existe uma alta demanda para utilização de terraform nas mais diversas organizações do mercado e existe bastante material sobre como usá-lo, a [documentação](https://developer.hashicorp.com/terraform/language) da Hashicorp é muito boa inclusive, mas o que eu percebo que é um grande problema a longo prazo é a organização do código.

Parte do problema de organização do código acontece pela falta de maturidade na escrita do código por parte de quem normalmente o escreve, que é o time de infra. Esse time normalmente tem uma experiência mais voltada a suporte de infraestrutura e uma menor familiaridade com escrita de código. Essa parte não será coberta nesse artigo, pois demandaria mais contexto e elementos a serem apresentados.

A outra parte do problema de organização de código é a baixa oferta de propostas claras e diretas sobre como fazer. Esse será o foco principal deste artigo: apresentar uma proposta sobre como devem ser organizados os módulos, e a partir de onde e de que forma ele pode ser consumido.

É importante salientar que esse artigo é uma proposta de debate, Não pretendo ser o dono da verdade.

# Módulos do terraform


Para a [Hashicorp](https://developer.hashicorp.com/terraform/language/modules/develop), o módulo é um **container** para múltiplos recursos que serão utilizados juntos. Você pode usar abstrações leves, de modo que possa descrever sua infraestrutura em termos de arquitetura, em vez de diretamente em termos de objetos físicos.

Para entender esse parágrafo você precisa saber inicialmente que esse "container" mencionado ali não é do docker, nem nada parecido, ok? Container ali é um termo usado para dizer que arquivos com a extensão .tf estão contidos no mesmo "lugar", que nesse caso é o módulo em questão.

Quando a Hashicorp conclui falando "Você pode usar abstrações leves, de modo que possa descrever sua infraestrutura em termos de arquitetura, em vez de diretamente em termos de objetos físicos" ela quer dizer que o objetos físicos são as infraestruturas criadas de fato na cloud (ou qualquer outro serviço que suporte terraform). 

Resumidamente: é possível dizer que o terraform te permite definir toda sua infraestrutura em um lugar, que é de onde você executa os comandos **terraform plan -out plano** (para criar o plano de mudança) e o **terraform apply plano** (para aplicar o plano criado no comando plan) a partir de apenas um lugar, e que todo código está contido ali naquele "container". Porém usando **module** você consegue separar esse código e oferecer a ele um maior reuso.

## Exemplo

O time de produto responsável pelo projeto ACME, que é um backend escrito em python, precisa de um servidor e um endereço DNS para que ele possa ser acessível por nome e não por IP.

Se você utiliza AWS, você fatalmente deverá criar um código para instanciar um servidor na AWS, e colocar o seu apontamento no DNS (route53).

Você pode criar esses código no terraform e colocar tudo numa única pasta, configurar o backend (armazenamento do estado do terraform) no mesmo lugar e ele será executado sem nenhum problema. 

Se o time de produto responsável pelo projeto PernaLonga resolver pedir a mesma coisa pra você, o que você faz? Cria um repositório chamado IaC e coloca tudo no mesmo lugar, só colocando mais um item na lista de infra a ser criada ou copia e cola o código?

Em ambas situações você possivelmente terá problemas. Na opção de apenas um repo de IaC, esse será uma grande dor de cabeça para quem opera, pois todo estado de **todos** os projetos da empresa estarão em um único lugar, e serão usados muitos recursos para que seja viável manter este projeto sem causar um grande estrago ou ser bloqueado por diversos motivos.

Quando você somente copia e cola o código se perde a possibilidade de reuso, além  de ter que manter todos os códigos atualizados. Por exemplo, sempre que mudar uma linha em um deles, terá que rastrear todos os repositórios que o utilizam e fazer esta mesma alteração manualmente em cada um deles.

## Como funciona o module

O **module** do terraform foi essencialmente concebido pensando nesses três elementos:

 * resource
 * input
 * output

### Resource

São os recursos a serem criados pelo terraform. Nesse caso seria o código responsável por criar a máquina virtual e o apontamento DNS.

### Input do module

É a possibilidade de parametrizar o uso dos módulos. Usando o exemplo em questão, a criação de uma máquina virtual e um apontamento DNS, quais desses recursos demandam possibilidade de parâmetros? Vamos focar nos mais essenciais levando em consideração aos usos mais comuns:

 * Nome da máquina
 * Tipo de máquina
 
Observação: Vamos levar em consideração que os projetos não desejam mudar o tamanho dos discos e nem nada muito elaborado, ok?

### Output do module

É o que resulta da execução de um module terraform. Usando o exemplo em questão seria a possibilidade do module retornar o IP e o nome completo do apontamento DNS que deve ser usado.

# Organização dos módulos

A ideia é usar o conceito de module para se criar um agrupamento do código, ou seja: cria-se um repositório à parte, para que ele possa ser chamado a partir de cada um dos repositórios de projetos.

## Exemplo

Nos casos já apresentados anteriormente, em vez de replicar código ou colocar tudo em um repositório só, seria criado um repositório chamado "criar_instancia_iac". Nesse repositório seriam colocados os arquivos com extensão .tf responsáveis pela criação dos recursos já citados anteriormente. Porém neste repositório não seria incluída a configuração do backend, por exemplo.

No module poderia ser assim por exemplo:

**instance.tf**
```
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
  owners = ["099720109477"] # Canonical
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "var.instance_type"
  tags = {
    Name = "HelloWorld"
  }
}
```

No arquivo acima o bloco de código **data** é responsável por obter na AWS a imagem mais recente do Ubuntu, que será usado no bloco **resource** que é onde define como a máquina virtual será criada. 

**dns.tf**
```
data "aws_route53_zone" "selected" {
  name         = var.dominio
  private_zone = false
}


resource "aws_route53_record" "record" {
  zone_id = var.zone_id
  name    = "${var.hostname}.${data.aws_route53_zone.selected.name}"
  type    = var.record_type
  ttl     = var.record_ttl
  records = [aws_instance.main.public_ip]
}
```

No arquivo acima o bloco de código **data** é responsável por coletar algumas informações da zona do route53. Neste caso é preciso passar o nome do domínio, pois é com esse dado que ele coletará os dados da zona que usaremos a seguir.

O bloco **resource** é responsável por criar uma entrada DNS no route53. Perceba que eu coloquei diversos apontamentos de variáveis que não citei antes. Essas variáveis podem ser usadas pelo "usuário final", mas não precisam necessariamente de ter seus valores especificados, pois terão valores padrões.

A parte mais importante está na linha **name** e **records** que é onde informamos o nome da entrada e o IP a ser colocado respectivamente.


**variable.tf**
```
variable "dominio" { 
  description = "Nome do domínio"
  type = string 
  default = "empresa.com.br."
} 

variable "record_type" {
  description = "Tipo do registro DNS no route53"  
  type = string
  default = "A"
}

variable "record_ttl" {
  description = "TTL registro DNS no route53"  
  type = string
  default = "10"
}

variable "instance_type" {
  description = "Tipo da instancia"  
  type = string
  default = "t3.micro"
}

variable "hostname" {
  description = "Nome da instancia"  
  type = string
}
```

No arquivo acima definimos todas as variáveis que serão usadas neste módulo. Perceba que de todas elas, apenas uma não tem default, isso indica que essa variável em questão é o que chamamos de **obrigatória**, ou seja, se ela não for definida na chamada do módulo, a execução falhará.

Você deve estar se perguntando "mas por que justamente essa não foi definida Gomex?". Porque na perspectiva de arquitetura de construção de um módulo é importante você deixar obrigatórias as variáveis que você deseja que a pessoa informe, ou seja, nesse caso eu queria que o usuário informasse o nome da máquina.

## Onde chamar o módulo

No repositório de produto de cada projeto terá um arquivo chamado terrafile.tf nele terá a chamada do módulo criado acima.

**Projeto ACME**
```
provider "aws" {
  region = "us-east-1"
}

terraform {
  backend "s3" {
    bucket = "seu_bucket_s3_aqui"
    key    = "projeto_acme"
    region = "us-east-1"
  }
}

module "maquinas" {
  source = "git@github.com:organizacao/criar_instancia_iac.git?ref=v0.1.0"

  tipo = "web"
  name = "testando"
}

```

No código acima o bloco de código **provider** é responsável por informar onde o terraform deverá se conectar para criar os recursos, que nesse caso é a AWS. As credenciais podem ser passadas via variáveis de ambiente.

No bloco de código **terraform** é informado qual backend será usado e nesse caso é o s3. Também precisa ser informado o nome do bucket e qual chave deve ser criada lá dentro, ou seja, você pode criar apenas um bucket e uma **key** para cada projeto. Isso indica que cada **key** será um arquivo de estado diferente. Meu conselho é que você coloque uma **key** por projeto para evitar usar o mesmo arquivo de estado para múltiplos projetos.

O bloco de código **module** é onde o module será chamado. A linha **source** informa onde ele deverá ser obtido. Estamos usando o github para armazenar esse repositório, sendo assim devo especificar o endereço do mesmo. No final dessa linha temos o "?ref=v0.1.0" que é a forma usada para informar qual versão deste módulo queremos usar. Esse valor aqui pode ser o nome de uma branch ou uma tag desse repositório.

Especificar qual versão do módulo será usado é muito importante, pois é a forma de você garantir que o código já entregue para uso não será afetado em caso de um novo commit no repositório.

Por exemplo: Na versão v0.1.0 foi entregue a criação da máquina e apontamento no DNS, mas se no futuro você desejar a adição de um IP elastico por exemplo, as pessoas que usam a versão v0.1.0 não serão afetadas por um possível bug ou apenas uma mudança de comportamento indesejada quando você atualizar seu módulo, já que o código da versão **v0.1.0** não será afetado, apenas a branch **main** e suas versões mais novas quando forem criadas novas tags.

## Conclusão

Ao utilizar module, separe-o em um repositório diferente, chamando o mesmo a partir de diversos repositórios de times e/ou de projetos diferentes. Especificando a versão do mesmo, você viabiliza o reuso do seu código, assim como oferece a garantia que o código testado não será modificado sem o consentimento de quem está consumindo o seu module.

## Agradecimentos

[Juliana Gaioso](https://twitter.com/juligaioso) que ajudou na revisão desse artigo.

Escrevi esse artigo ouvindo:

- 3 Hours Chopin for Studying, Concentration, Relaxation
- Belle and Sebastian
