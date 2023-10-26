+++
title = "A importância do Packer no fluxo do Terraform"
date = "2023-10-25"
draft = false
Categories = ["devops", "terraform", "ansible", "packer", "IAC", "pipeline"]
Tags = ["devops", "terraform", "ansible", "packer", "IAC", "pipeline"]
+++

## Só lembram do terraform

Na criação de um servidor, muito se fala do [Terraform](https://www.terraform.io/), que é uma ferramenta muito boa, super útil e disponível para provisionar essa infraestrutura. Se você ainda não conhece, o Terraform é uma ferramenta que lê um arquivo de definição com a extensão ".tf" e aplica o que foi descrito no arquivo na infraestrutura remota, que pode ser algum provedor de Cloud ou um SaaS.

Vamos a um exemplo da provedora de cloud mais usada, que é a AWS:

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }

  owners = ["099720109477"]  # ID da conta da Canonical (proprietária das imagens do Ubuntu)
}

resource "aws_instance" "example" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"  # Tipo da instância, você pode escolher o desejado
  key_name      = "sua_chave_privada"  # Substitua pelo nome da chave SSH que você possui
  tags = {
    Name = "UbuntuServer"
  }
}
```

O código acima será responsável por, primeiramente, coletar a imagem AMI mais recente do ubuntu **focal-20.04-amd64**, como podemos ver no recorte do código abaixo:


```hcl
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
```
Porém, antes de nos aprofundarmos no código, é importante salientar que a AMI é o produto para criação e repositório de imagens da AWS, utilizado para criar suas máquinas. Cada AMI é como se fosse uma foto de um servidor em um instante do tempo.

A linha restante nesse bloco de código do tipo **data** especifica qual o ID do dono da AMI, que, nesse caso, é a Canonical. É sempre importante olhar na AWS o ID correto, para garantir que você está obtendo a imagem de um local confiável.

```hcl
 owners = ["099720109477"]  # ID da conta da Canonical (proprietária das imagens do Ubuntu)
}
```

O código restante é responsável por criar a máquina:

```hcl
resource "aws_instance" "example" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"  # Tipo da instância, você pode escolher o desejado
  key_name      = "your-key-pair"  # Substitua pelo nome da chave SSH que você possui
  tags = {
    Name = "UbuntuServer"
  }
}
```

No argumento **ami** dentro do recurso do tipo **aws_instance**, que é responsável por definir a criação de um servidor na AWS, há um apontamento para o **data.aws_ami.ubuntu.id**.

Agora, lembra do bloco de código que você definiu antes? Então! Ele definiu uma AMI e, agora no bloco recurso, você vai usar a saída dessa definição, que é a AMI em si. Você deve apontar para um dado em específico, que é o id dela, pois se você colocar **data.aws_ami.ubuntu** sem o **.id** no final, você coletará uma grande número de informações que, ao serem colocadas como valor no argumento **ami**, não farão sentido algum. Dessa forma, você define o ID da AMI no campo do recurso que cria a máquina e, assim, a máquina é criada.

## Máquina criada… e agora?

Lembre-se de que a máquina acabou de ser criada, a partir de uma imagem oficial da Canonical, ou seja, ela não tem praticamente nada do que você precisa para atender a demanda do seu produto. 

Para configurar a máquina, você precisa de uma outra ferramenta, que seja responsável pelo gerenciamento de configuração. Nesse artigo, usaremos o **Ansible**, mas você pode utilizar,nesse contexto, a ferramenta que desejar.

Vejamos um exemplo de código criado com Ansible:

```yaml
---
- name: Configuração do produto
  hosts: seu-servidor
  become: yes
  roles:
    - ansible_role_seu_produto
```

O arquivo acima é responsável por executar a role chamada **ansible_role_seu_produto**, normalmente, roles mais simples demoram alguns minutos para serem executados completamente. 

Imagine seu pipeline de produto demorar diversos minutos a cada vez que precisar de um servidor novo, esperando um recurso para fazer deploy do código..

Eu nem comentei dos requisitos para executar a role, pois normalmente não estará disponível no seu pipeline. Antes de executar, você precisará baixá-la.

Antes de baixar a **role ansible**, você normalmente precisará instalar algumas bibliotecas python para que tenha os binários para fazer todo esse processo. Nisso já se passou muito tempo, tornando o processo de criação de infraestrutura inviável dentro do fluxo de entrega de produtos.

## O que fazer?

Uma ferramenta pouco comentada dentro da comunidade de infraestrutura como código é o [Packer](https://www.packer.io/), que é uma ferramenta que tem como objetivo criar imagens. Ela pode criar imagens para diversos provedores de cloud, virtualizadores ou qualquer outro produto que utilize o imagens para criação de ambientes.

O **Packer** é uma ferramenta incrível, mas é pouco usada porque ela é responsável por uma parte do processo que normalmente as pessoas não otimizam, que é a criação de ambientes.

Vejamos um exemplo abaixo:

```hcl
{
  "variables": {
    "aws_access_key": "{{env `AWS_ACCESS_KEY`}}",
    "aws_secret_key": "{{env `AWS_SECRET_KEY`}}"
  },
  "builders": [
    {
      "type": "amazon-ebs",
      "access_key": "{{user `aws_access_key`}}",
      "secret_key": "{{user `aws_secret_key`}}",
      "region": "us-east-1",
      "source_ami_filter": {
        "filters": {
          "name": "ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*",
          "virtualization-type": "hvm",
          "root-device-type": "ebs"
        },
        "owners": ["099720109477"],
        "most_recent": true
      },
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "custom-ubuntu-{{timestamp}}"
    }
  ],
  "provisioners": [
    {
      "type": "ansible",
      "playbook_file": "caminho/do/playbook/seu-playbook.yml",
      "user": "ubuntu",
      "extra_arguments": ["--private-key", "caminho/do/sua-chave-privada.pem"]
    }
  ]
}
```

O código apresentado acima será responsável por criar uma imagem **AMI** na AWS dentro de alguns parâmetros.

É importante perceber no código acima que o processo do **Packer** funciona com a criação de uma máquina virtual na AWS, essa máquina é do tamanho **t2.micro** e usará a AMI mais recente do Ubuntu focal 20.04;e nesse servidor, por sua vez, será provisionado o playbook **Ansible**, que chama a role Ansible já explicada anteriormente.

Ao final deste processo, será criada uma imagem AMI com base no Ubuntu e com as modificações aplicadas pela role Ansible executada.

Abaixo, vamos explicar o código parte a parte, para ficar mais claro.

```hcl
{
  "variables": {
    "aws_access_key": "{{env `AWS_ACCESS_KEY`}}",
    "aws_secret_key": "{{env `AWS_SECRET_KEY`}}"
  },
```

Na primeira parte, ele define algumas variáveis que podem ser usadas no restante do código. A ideia aqui é pegar a variável de ambiente chamada **AWS_ACCESS_KEY** e colocar na variável do packer chamada **aws_access_key**. O mesmo com a outra variável.

```hcl
  "builders": [
    {
      "type": "amazon-ebs",
      "access_key": "{{user `aws_access_key`}}",
      "secret_key": "{{user `aws_secret_key`}}",
      "region": "us-east-1",
```

No parte de código acima, começamos a definir nosso **builder**, que é o lugar temporário onde a imagem será criada. Imagine que é aqui que será definido tudo referente a esse servidor, o qual será usado temporariamente para a imagem ser criada.

No bloco demonstrado acima, é especificado o tipo do builder, que, nesse caso, é **amazon-ebs**, (builder mais usado para criar AMI na AWS). O **access_key** e **secret_key** são referentes à segurança, como se fosse um usuário e senha para se conectar à AWS e iniciar o processo. Por fim, temos o **region**, que  é a localização aproximada do datacenter da AWS onde isso tudo acontecerá.

```hcl
"source_ami_filter": {
        "filters": {
          "name": "ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*",
          "virtualization-type": "hvm",
          "root-device-type": "ebs"
        },
        "owners": ["099720109477"],
        "most_recent": true
      },
```

No bloco de código acima é definido um filtro para buscar a imagem de origem, pois você precisará de um servidor rodando para executar suas atividades. Esse servidor precisará, por sua vez, de uma AMI inicial.

```hcl
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "seu-produto-{{timestamp}}"
    }
```

No código acima são definidos detalhes como o tamanho da máquina, qual nome SSH será usado para conectar na mesma e, por fim, mas não menos importante, o nome da imagem ao final de todo o processo. Perceba que **{{timestamp}}** é uma variável que conterá o tempo exato do momento da criação da imagem na AWS; assim, suas imagens nunca terão o mesmo nome.

```hcl
"provisioners": [
    {
      "type": "ansible",
      "playbook_file": "caminho/do/playbook/seu-playbook.yml",
      "user": "ubuntu",
      "extra_arguments": ["--private-key", "caminho/do/sua-chave-privada.pem"]
    }
  ]
```

No bloco de código **provisioners** é informada qual ferramenta de configuração será usada, e nesse caso é o **ansible**, assim como qual playbook será executado, usuário a se conectar via SSH e argumentos extras do ansible.

## O que tudo isso tem de mais relevante?

Lembra como é demorada a criação do servidor, junto a sua configuração, dentro do fluxo de entrega de produto? Então, é possível mover essa demora para outro fluxo de entrega.

A ideia é que esse fluxo todo do **Packer** seja outro pipeline e, assim, você não precisará de vários minutos para provisionar completamente uma servidor recém criado. Você já terá uma AMI pronta para ser usada no repositório da AWS e seu terraform ficará da seguinte maneira:

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["seu-produto-*"]
  }

  owners = ["seu_id_aqui"] 
}

resource "aws_instance" "example" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"  
  key_name      = "sua_chave_privada"  
  tags = {
    Name = "UbuntuServer"
  }
}
```

Com isso, seu fluxo de entrega de produtos será completamente descolado da configuração da máquina e, dessa forma, o terraform criará a máquina com todos os requisitos necessários para que seu software seja depositado sem longas esperas.

Teremos um pipeline para gerar imagens AMI específicas para seu produto e outro pipeline que usará a imagem mais recente criada pelo pipeline do packer para rodar seu software.

Através desse mecanismo, você poderá colocar mais etapas na criação da imagem, que têm como objetivo oferecer mais qualidade para sua imagem. Você pode checar se algum pacote instalado está vulnerável. Você pode testar se a imagem de fato atende a expectativa rodando alguns testes automatizados no servidor recém criado antes de gerar a imagem AMI na AWS. Por fim, tudo isso acontecerá em um fluxo completamente à parte e gerenciado pela equipe focada em infraestrutura como código.
 
## Agradecimento

Obrigado a [Saulo](https://twitter.com/madalozzo) e [Isis](https://www.instagram.com/isisduartef/) que colaboraram muito na revisão do texto. O texto está bem melhor com a colaboração de vocês.
