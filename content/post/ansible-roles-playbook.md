+++
title = "Como organizar as roles e playbooks do ansible"
date = "2021-01-18"
draft = false
Categories = ["portugues", "devops", "ansible"]
Tags = ["portugues", "devops", "ansible"]
+++

# Contexto

O ansible é por definição um gerenciador de configuração, que resumidamente é a ferramenta responsável por aplicar definições de infraestrutura como código nos ativos.

Exemplo:

Você quer instalar e configurar um servidor nginx, o gerenciador de configuração permite que você escreva um arquivo com tudo que precisa, e toda vez que você precisar instalar e configurar um novo nginx, você executa o software apontando para o arquivo de definição que você escreveu previamente, que o gerenciador de configuração se encarrega de instalar e configurar tudo exatamente da forma como foi definido anteriormente.

O problema de arquivos de definição de gerenciadores de configuração é a organização e separação das responsabilidades, pois à medida que a necessidade de infraestrutura como código aumenta, é comum que tudo fique em um único arquivo, o que dificulta a gestão, ou muitas vezes é separado de forma equivocada.

Esse artigo falará especificamente sobre ansible, mas com alguma adaptação, eu acho que ele pode ser usado para outro gerenciador de configuração também.

# Introdução

O ansible tem o conceito de **playbook** e **roles**. Ambos têm como objetivo organizar as definições que serão aplicadas no ativo. Para facilitar o entendimento, trataremos esse ativo como servidor, uma máquina, que receberá comandos para estar de acordo com nosso desejo. O ansible pode ser usado para configurar SaaS, PaaS e afins, mas nesse artigo trataremos especificamente de máquinas, para facilitar o entendimento.

## Playbook

Esse é o arquivo mais elementar do ansible, ao começar a usar ansible normalmente se aprende criando um [playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html) e algumas pessoas acabam usando apenas isso para o resto da vida, o que é bem ruim, mas trataremos disso depois.

O [playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html) é um arquivo [YAML]() que tem a definição inicial do ansible, toda execução do ansible deve partir de um playbook. Segue abaixo um exemplo comum:

```
---
- name: update web servers
  hosts: webservers

  tasks:
  - name: ensure nginx is at the latest version
    yum:
      name: nginx
      state: latest
```

No primeiro bloco temos:

```
- name: update web servers
```

Essa linha é o título desse playbook. Tem como objetivo dizer claramente qual objetivo desse playbook, o que ele irá fazer.

Em seguida a especificação sobre quais hosts ou grupos de hosts podem usar esse playbook:

```
  hosts: webservers
```

Isso quer dizer que se você executar esse playbook ele será aplicado apenas para os hosts que estiverem no grupo webservers. Segue um exemplo de inventário:

```
[databases]
mysql01.exemplo.com.br
mysql02.exemplo.com.br

[webservers]
web01.exemplo.com.br
web02.exemplo.com.br
```

Com base no exemplo de inventário acima, o playbook em questão será executado apenas para o grupo webservers, mesmo que seja especificado os hosts do databases na execução do ansible, pois nesse caso ele dirá que não tem servidor disponível para executar esse playbook.

No próximo bloco de código é onde começa as tarefas do ansible:

```
tasks:
  - name: ensure apache is at the latest version
    yum:
      name: nginx
      state: latest
```

O bloco **tasks** é usado para iniciar as tarefas que serão executadas no servidor destino, no caso exemplo apresentado acima, ele usará o módulo **yum** para instalar o pacote httpd na máquina alvo. 

É muito comum usar o bloco **tasks** para colocar todo tipo de tarefa e assim o playbook vira um arquivo único que tem todas as tarefas que você precisa. Quando esse arquivo tem apenas 2 ou 3 tasks talvez não demonstre o problema desse uso, mas ao aumentar o número de tasks se percebe o problema de gerência de um arquivo único.

Pensando nisso foi criado o conceito **role** do ansible.

## Role

Essa é a melhor forma para organizar as plays do seu ansible, onde cada pasta tem suas função específica para facilitar tanto no uso como na implementação do que se desejar aplicar com aquela role.

Normalmente dentro de uma role temos as seguintes pastas:

 - tasks/
 - files/
 - templates/
 - handlers/
 - meta/
 - defaults/
 - vars/

Quando uma pasta com um determinado nome, tem esse conjunto de subpastas dentro, com todos seus arquivos yaml dentro de cada uma delas, podemos dizer que temos uma role.

Ao utilizar uma role o ansible automaticamente procurará o arquivo main.yml dentro da pasta tasks para iniciar as tasks, uma vez que uma task tenha a cópia de um arquivo, usando o módulo **copy** por exemplo, essa task vai buscar o arquivo na pasta **files**. Segue abaixo um exemplo:

```
- name: Copiar o arquivo tests.sh
    copy:
        src: tests.sh
        dest: /opt/tests/tests.sh
```

Perceba que a pasta onde está o arquivo **tests.sh** não foi sequer mencionada, isso sendo uma role, ele vai buscar dentro da pasta **files** e não no local onde foi executado o binário do ansible, como é o comportamento padrão para execução de playbook puro.

O mesmo acontece ao utilizar templates:


```
- name: Copy o arquivo tests.sh
    template:
        src: tests.sh
        dest: /opt/tests/tests.sh
```

Na pasta **handlers** será usada a configuração dos [handlers](https://docs.ansible.com/ansible/latest/user_guide/playbooks_handlers.html) que ao ser chamado em uma task, será executado conforme sua configuração. Segue exemplo:

```
---
- name: Copy o arquivo tests.sh
    template:
        src: tests.sh
        dest: /opt/tests/tests.sh
    notify: restart test
```

Dentro da pasta **handlers** teríamos algo assim:

```
---
- name: restart test
      command: "docker restart test"
```
Percebam que nem no main.yml do **tasks** ou do **handlers** será necessário começar com os blocos **handlers:** ou **tasks:**, pois o fato do main.yml está dentro da pasta correta, já indica o propósito e execução daquela informação. Isso quer dizer que se você errar e colocar uma linha que deveria ser do **handlers** e colocar ela dentro do main.yml da pasta **tasks** ele será executado como uma **task** comum e não como um **handler**.

A pasta **meta/** é onde os metadados dessa role são armazenados. Não há muito a dizer além do que temos na [documentação oficial](https://galaxy.ansible.com/docs/contributing/creating_role.html#role-metadata).

A pasta **defaults/** é onde estará o valor padrão de todas as variáveis que não seja realmente necessária ser setada por quem está consumindo a role. Normalmente são valores que funcionam sem qualquer dano para quem está executando pela primeira vez, ou seja, o valor default de uma task que possa apagar algo na máquina da pessoa que consome essa role jamais deve ser definido, por exemplo.

```
---
integration_test: true
```

No exemplo acima a variável **integration_test**, que será usada pelas tasks dessa role, será usada com valor **true** toda vez que não for passado um valor de outra forma.

Atenção, para quem consome a role, a pasta **default** não deve ser editada para que um novo valor de variável seja aplicado.

A pasta **vars/** praticamente não será usada em uma role, pois em minha humilde opinião, que posso estar errado e depois eu venho corrigir isso aqui, se for o caso, as variáveis não devem ser aplicadas dentro da **role**. 

# Organização das roles e playbooks

Já foi dito que não deve usar o playbook para armazenar tudo que precisa para uma execução, como por exemplo colocar diversas **tasks** e **handlers** dentro do mesmo playbook.yml, que ao final do trabalho ficará enorme e insustentável do ponto de vista da organização e gestão de atualização.

Já foi dito também que os valores das variáveis não devem ser aplicadas dentro da role, que apenas os valores **defaults** devem ser aplicados lá e qualquer alteração por parte de quem consome a role, deveria ser aplicada em outro local, pois a pasta **vars** foi também apontada como não ideal para o caso.

Qual a forma de se utilizar as roles e playbooks?

Segue abaixo um ótimo exemplo para um playbook:

```
---
- hosts: all
  roles:
    - dokku_bot.ansible_dokku
    - iac-role-basica
  vars:
    dokku_version: 0.22.3
    sshcommand_version: 0.12.0
    dokku_hostname: mariaquiteria.example.org
```

Veja o propósito do playbook. Ele deve ser usado **apenas** como inicializador do processo, é onde deve iniciar o processo de configuração.

Nele deve estar qual limite de grupos devem ser usados para aplicar esse playbook em questão:

```
 - hosts: all
```

Nesse exemplo indica que esse playbook pode ser aplicado para qualquer grupo do seu inventário.

Uma coisa muito importante a pensar ao criar playbooks é que não deve estar nesse playbook nada de execução de tarefas, ou seja, se existir o bloco **tasks** nesse playbook ele não deve fazer nenhuma ação complexa. 

Pense que seu **playbook** é uma forma de dar vida a sua role, se você colocar tasks no seu playbook toda a ideia de **roles** perdem o seu benefício.

Para usar roles é simples:

```
roles:
    - dokku_bot.ansible_dokku
    - iac-role-basica
```

Essas roles precisam estar na máquina que fará o deploy do código ansible, ou seja, você deve usar um arquivo chamado **requirements.yml** e dentro dele colocar as roles que deseja baixar:

```
# Do galaxy
- src: dokku_bot.ansible_dokku

# Do repositório github que tem a role básica
- src: git@github.com:DadosAbertosDeFeira/iac-role-basica.git
  scm: git
  version: "0.1" 
```

O fato dessas roles estarem configuradas para baixar dentro do **requirements.yml** não indica que o ansible-playbook baixará as roles automaticamente ao executar. Isso precisa ser feito antes com o comando:

```
ansible-galaxy install -r requirements.yml
```

Dentro das roles será colocado todo código necessário para aplicar o estado que deseja na máquina de destino.

A organização ideal, em minha humilde opinião, segue no desenho abaixo:


Teremos um repositório inicial, de ponto de partida, onde nele será adicionado o **playbook.yml**, que será usado para iniciar o deploy e o **requirements.yml** que terá a configuração para baixar as roles que serão usadas no **playbook.yml**.

As roles produzidas e mantidas pelo seu time devem estar pelo menos em um repositório de controle de versão git, que pode ser usado para baixar a role a partir do **requirements.yml**, como já foi apresentado anteriormente.

É importante salientar que ao usar o repositório git as pastas apresentadas neste artigo devem estar na raiz do repositório git. Não crie uma pasta chamada roles e dentro dele coloque outros arquivos e pastas. A forma ideal você pode encontrar [nesse repositório](https://github.com/DadosAbertosDeFeira/iac-role-basica) por exemplo.

Segue abaixo um comando que pode ser usado para criar uma role:

```
ansible-galaxy init nome_da_sua_role
```

O resultado disso será uma pasta com o título **nome_da_sua_role**, essa será a pasta que você deve executar o git init para criar um repositório git e depois enviar para seu servidor git.

# Agradecimentos 

Obrigado a [somatorio](https://twitter.com/somatorio) que sempre revisa tudo que escrevo.

Obrigado também a lista de pessoas abaixo que também revisaram o texto:

 - [Kleber Cabral](https://twitter.com/klebercabral)
 - [Fábio Costa](https://www.linkedin.com/in/f%C3%A1bio-costa-4ba82614b)
 - [Juan Maia](https://twitter.com/juanCM_10x)