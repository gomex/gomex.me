+++
title = "Quando o terraform e a AWS não interagem como você espera"
date = "2025-04-10"
draft = false
Categories = ["portugues", "devops", "terraform", "aws"]
Tags = ["portugues", "devops", "terraform", "aws"]
+++

# Contexto

Em algumas situações o terraform vai aplicar mudanças na AWS e o resultado não será exatamente o que você espera. Vou apresentar um caso abaixo para que você possa entender como isso funciona.

Usarei o exemplo do autoscaling group e launch template e apresentarei que mesmo usando os valores oferecidos pela AWS como o esperado, o funcionamento dos serviços poderão não atender a sua expectativa.

# Introdução

**Autoscaling Group** (ASG) da AWS é um recurso que ajusta automaticamente o número de instâncias EC2 em execução, com base em regras pré-definidas. É um serviço da AWS para que você possa criar um cluster de máquinas para atender uma determinada demanda. Normalmente é usado junto com balanceadores de carga, que não serão tratados aqui pra evitar a mistura de assuntos.

**Launch Template** da AWS é um modelo que define configurações padronizadas para iniciar instâncias EC2, como tipo, AMI, rede e armazenamento, permitindo consistência, versionamento e automação no provisionamento de recursos. O **Autoscaling Group** usa o **Launch Template** para criar o seu conjunto de máquinas.

# Criando um Autoscaling Group com Launch Template

Segue abaixo o código do **Launch Template**  e **Autoscaling Group**. Eu vou ignorar os outros recursos e data que normalmente são usados nesse código, para evitar muito código pra ler. O importante aqui é entender o problema e não o código inteiro.

Primeiro o **Launch Template**:

```yaml
resource "aws_launch_template" "this" {
  name_prefix   = "${var.name}-lt-"
  image_id      = data.aws_ami.this.id
  instance_type = var.instance_type

  iam_instance_profile {
    name = local.iam_instance_profile_name
  }

  network_interfaces {
    associate_public_ip_address = var.subnet_tier == "public" ? true : false
  }

  tag_specifications {
    resource_type = "instance"

    tags = merge({ "Name" = var.name })
  }
}
```

Importante atentar para essa parte do código, que é onde especificamos qual a AMI ID usaremos nesse **Launch Template**:

```yaml
resource "aws_launch_template" "this" {
  ...
  image_id      = data.aws_ami.this.id
  ...
```

O data que coleta a ami está usando essa configuração:

```yaml
data "aws_caller_identity" "current" {}
data "aws_ami" "this" {
  most_recent = true

  filter {
    name   = "name"
    values = ["imagem-base-*"]
  }

  owners = [data.aws_caller_identity.current.account_id]
}
```

Como pode ver no código acima, ele pega a AMI mais recente, fazendo filtro por uma AMI que comece com "imagem-base" da sua conta, pois no **owners** estamos usando o data que coleta o account id da sessão atual.

Tendo o **Launch Template**: pronto, agora vamos para o **Autoscaling Group**:

```yaml
resource "aws_autoscaling_group" "this" {
  name_prefix         = "${var.name}-asg-"
  max_size            = var.max_size
  min_size            = var.min_size
  vpc_zone_identifier = data.aws_subnets.this.ids
  launch_template {
    id      = aws_launch_template.this.id
    version = "$Latest"
  }

  instance_refresh {
    strategy = "Rolling"

    preferences {
      min_healthy_percentage = 50
      instance_warmup        = 30
    }
  }

  tag {
    key                 = "Name"
    value               = var.name
    propagate_at_launch = true
  }

  lifecycle {
    create_before_destroy = true
  }
}
```

É importante atentar para esse parte do código:

```yaml
  launch_template {
    id      = aws_launch_template.this.id
    version = "$Latest"
  }
```

Esse bloco usa como base a [documentação oficial](https://registry.terraform.io/providers/hashicorp/aws/2.70.1/docs/resources/autoscaling_group#with-latest-version-of-launch-template) desse recurso.

# O conflito

Ao especificar o valor **"$Latest"** o **Autoscaling Group** será criado na primeira execução, mas caso você gere uma imagem nova ou troque qualquer configuração no **Launch Template** o **Autoscaling Group** não será alterado, e dessa forma ele não fará o que você espera, que seria reciclar as instâncias atuais, usando o **instance refresh** com a estratégia **Rolling** como especificado nessa parte do código do **Autoscaling Group**:

```yaml
  instance_refresh {
    strategy = "Rolling"

    preferences {
      min_healthy_percentage = 50
      instance_warmup        = 30
    }
  }
```

Após a criação de todos os recursos, ao executar o terraform plan, esse seria o resultado:

```
Terraform will perform the following actions:

  # module.ec2.aws_launch_template.this will be updated in-place
  ~ resource "aws_launch_template" "this" {
        id                      = "lt-0737f082513450ac"
      ~ image_id                = "ami-0311c1f1234d3743" -> "ami-07d391347313902"
      ~ latest_version          = 11 -> (known after apply)
        name                    = "image_base-lt-20250410135927734600000001"
        # (9 unchanged attributes hidden)

      + iam_instance_profile {}

        # (2 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```

Perceba que apenas o **Launch Template** é modificado e por conta disso o **Autoscaling Group** não fará nada com a instância. Eu esperava que existisse algum processo interno na AWS que percebesse a mudança de versão e assim executasse o **instance refresh** com a estratégia **Rolling**, que criaria uma maquina nova, com a AMI nova, esperaria 30 segundos, que foi configurado no **instance_warmup** e desligaria a instancia com a AMI antiga, mas nada acontece. 

Para corrigir esse problema forçaremos o **Autoscaling Group** ser modificado.

No bloco **launch_template** do **Autoscaling Group** faremos a modificação desse código:

```yaml
  launch_template {
    id      = aws_launch_template.this.id
    version = "$Latest"
  }
```

Para esse:

```yaml
  launch_template {
    id      = aws_launch_template.this.id
    version = aws_launch_template.this.latest_version
  }
```

E essa será o plano do terraform que você verá toda vez que a AMI mudar:

```
Terraform will perform the following actions:

  # module.ec2.aws_autoscaling_group.this will be updated in-place
  ~ resource "aws_autoscaling_group" "this" {
        id                               = "base_image-asg-20250410145900574600000002"
        name                             = "base_image-asg-20250410145900574600000002"
        # (27 unchanged attributes hidden)

      ~ launch_template {
            id      = "lt-0737f0825b14de0ac"
            name    = "base_image-lt-20250410135927734600000001"
          ~ version = "12" -> (known after apply)
        }

        # (2 unchanged blocks hidden)
    }

  # module.ec2.aws_launch_template.this will be updated in-place
  ~ resource "aws_launch_template" "this" {
        id                      = "lt-0737f0825b14de0ac"
      ~ image_id                = "ami-0311c1f1234d3743" -> "ami-07d391347313902"
      ~ latest_version          = 12 -> (known after apply)
        name                    = "base_image-lt-20250410135927734600000001"
        tags                    = {}
        # (9 unchanged attributes hidden)

      + iam_instance_profile {}

        # (2 unchanged blocks hidden)
    }

Plan: 0 to add, 2 to change, 0 to destroy.
```

Como pode ver acima, o terraform agora controla esse versionamento e garante que toda vez que uma versão nova do **Launch Template** é criada, será atualizado o seu número no **Autoscaling Group** e assim forçando a AWS a iniciar o processo de **Instance Refresh**.

# Conclusão

As vezes é importante avaliar se o controle não pode estar no seu código terraform e assim você ter um retorno mais garantido de como sua infraestrutura vai funcionar.


# Agradecimentos 

Obrigado a [somatorio](https://bsky.app/profile/somatorio.bsky.social) que sempre revisa tudo que escrevo.

Obrigado a [Flávia](https://www.instagram.com/flaviataide/) e [Roberta](https://www.instagram.com/roberta.oliveiraj/) que leram e me deram feedback.
