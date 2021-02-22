+++
title = "Usando variáveis no Ansible"
date = "2021-02-22"
draft = false
Categories = ["portugues", "ansible", "QA"]
Tags = ["portugues", "devops", "ansible", "QA", "variavel"]
+++

# Introdução

O Ansible é uma ferramenta com muitas possibilidades e grande versatilidade, e justamente por isso pode ser um problema para quem usa suas funcionalidades sem seguir algumas boas práticas.

Esse artigo tem como objetivo apresentar as formas que eu aconselho na utilização de variáveis do Ansible. Eu vou tentar pontuar, da forma mais clara possível, os motivos para cada sugestão apresentada, mas percebam que boa parte delas partem de uma premissa de organização, e talvez não seja a sua forma de organizar.

# Como assim usar variáveis no Ansible? 

Normalmente você utilizará variáveis em uma __role__ Ansible por dois motivos: 

 - Mudar comportamento da __role__
 - Evitar repetição de valores dentro da __role__

## Mudar comportamento da __role__

Se você não sabe o que é uma __role__ no Ansible, vale muito a pena ler [esse](https://gomex.me/2021/01/18/como-organizar-as-roles-e-playbooks-do-ansible/) artigo.

Quando se constrói uma __role__ Ansible normalmente se espera que ela assegure um conjunto de comportamentos, que normalmente são agrupados para fazer algum sentido. 

Vamos usar como exemplo uma __role__ para controlar o [nginx](https://nginx.org/en/). Essa __role__ seria responsável por garantir que o pacote do nginx esteja instalado e um determinado conjunto de domínios seja configurado. Esses são os comportamentos esperados dessa __role__.

Como mudar o comportamento citado acima? Eu posso especificar qual versão do nginx será instalado, qual porta padrão será utilizada, quais plugins quero instalar no processo, localização da pasta de log e afins.

A possibilidade de modificar o comportamento de uma __role__ sempre estará limitado a quantidade de variáveis que essa __role__ tem, pois caso contrário você precisa mudar o código da __role__ para que ela se comporte de uma outra forma.

Imagine a execução de um playbook como esse:

```
---
- hosts: all
  roles:
    - nginx
  vars:
    nginx_version: 1.19
```

Nessa __role__ terá alguma task responsável por instalar o pacote e ele seria assim para uma plataforma que usa apt-get como instalador de pacotes:

```
- name: Instalando pacote
  apt:
    name: nginx={{ nginx_version }}
```

Com base nisso podemos concluir que o valor passado no **nginx_version** será usado no momento da instalação, isso quer dizer que se você esquecer de passar esse valor o Ansible quebrará.

Nesse momento você deve ser fazer uma pergunta muito importante ao criar variáveis para mudança de comportamento na __role__: "Essa variável **precisa mesmo** ser informada pelo usuário? Existe algum valor que eu possa sugerir sem que isso afete a lógica da __role__?" 

Se você pode passar um valor padrão, **SEMPRE** passe, pois uma __role__ que precisa de um lista enorme de valores para funcionar não é algo muito funcional, né?

Caso você possa passar um valor default, sempre faça isso na pasta **default**. Segue abaixo um exemplo:

```
---
nginx_enable: true
nginx_install: true
nginx_start: true
nginx_version: 1.18
```

Com esses valores dentro do main.yml da pasta default, indica que caso você não informe **nginx_version** o valor usado pela __role__ para informar qual versão do nginx que será instalada é **1.18**.

### Evite usar o filtro default

Existe [um filtro](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#providing-default-values) no Ansible para manipular valores de variáveis dentro da task.

Esse filtro é usado dentro da definição de variável na task:

```
- name: Instalando pacote ansible
  apt:
    name: nginx={{ nginx_version | default('1.18') }}
```

Com essa definição, caso você não especifique a versão usando a variável **nginx_version** essa task instalará o nginx na versão **1.18**.

Qual problema há nesse tipo de uso? Falta de padrão e desorganização da sua __role__. Imagine uma __role__ com diversos arquivos definindo tasks e os valores padrão espalhados por todos os arquivos. Se você precisar modificar o valor padrão? Como saber quais arquivos devem ser modificados? 

Se você utilizar os arquivos da pasta default ficará mais claro, afinal esses arquivos tem um propósito único. O que não é o caso dos arquivos de tasks.

### Tratando variáveis sem valores padrões

Um exemplo de uma variável que precisa **realmente** ser informada pelo usuário: Uma __role__ que é responsável por gerenciar entradas DNS em um serviço SaaS. Essa __role__ pode garantir que determinadas entradas não estejam lá. Se eu quiser usar esse comportamento eu necessariamente precisarei informar **qual** a entrada deve ser removida. Existe alguma forma de sugerir um valor padrão na __role__ nessa situação? Não. Sendo assim você não informará nenhum valor na pasta **default**.

Nesses casos usaremos o [assert](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/assert_module.html) do Ansible, para validar se a variável existe antes de continuar o uso dessa __role__:

```
- name: Checar se 'nome_variavel' está definida
    assert:
      that:
        - nome_variavel is defined
      msg: "'nome_variavel' precisa ser definida"
```

Com o código acima executado antes de qualquer outra tarefa poderemos garantir que a __role__ só será usada caso você informe a variável necessária. Você evita que a sua role seja executada pela metade e pare de funcionar antes de terminar todo processo. Deixando seu servidor com metade do que deveria ser assegurado na máquina.

## Evitar repetição de valores dentro da role

Dentro de uma __role__ podem existir valores que você pode precisar repetir, mas você quer criar uma __role__ bem parametrizada e evitar que uma mudança no nome do produto não demande que você busque em todo código o nome para que ele seja trocado um a um.
```
- name: Instalando pacote
  apt:
    name: {{ nome_produto }} ={{ versao_produto }}
- name: Garantir que o serviço está executando
  ansible.builtin.service:
    name: {{ nome_produto }}
    state: started
```

O valor de **nome_produto** deve ser armazenado na pasta **vars** dentro do arquivo main.yml:


```
---
nome_produto:  nginx
```

Lembre-se que o valor de **versao_produto** deve ser armazenado dentro da pasta **default**, pois esse é o comportamento que se espera que seja modificado pelo usuário. O valor da pasta **vars** é apenas para variáveis que não devem ser sobrescritas pelo usuário da __role__.

É importante entender a [precedência](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#understanding-variable-precedence) das variáveis quando elas são sobrescritas.

# Conclusão

Variáveis são formas interessantes de tornar sua __role__ Ansible mais configurável e assim garantir uma boa manutenção desse código.

Sempre imagine que sua __role__ vai crescer o suficiente para que seja necessário o uso de boas práticas, por isso aconselho sempre que começar já faça do jeito ideal, afinal depois de pronta, a refatoração pode ser mais trabalhosa.

# Fontes

 - https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html
 - https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html

# Agradecimentos

Obrigado a [somatorio](https://twitter.com/somatorio) que sempre revisa tudo que escrevo.

Obrigado também a lista de pessoas abaixo que também revisaram o texto:

 - [Kleber Cabral](https://br.linkedin.com/in/klebercabral)
 - [Prof. Dr. Vicente Marçal](https://twitter.com/riverfount)
 - [Francílio Araújo](https://www.linkedin.com/in/)