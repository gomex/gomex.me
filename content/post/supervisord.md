+++
title = "Precisa executar mais do que um processo por container?"
date = "2018-07-12"
draft = false
Categories = ["portugues", "devops", "docker", "supervisord"]
Tags = ["portugues", "devops", "carreira"]
+++

## TL;DR

Você precisa de mais do que um processo em um container, para tal você precisa seguir algumas melhores práticas para que seu container não se torne uma "máquina leve".

### Problema

No meu atual trabalho, preciso executar muitos processos em um container. Isso por sí já um sinal de problema na arquitetura do serviço em questão para um modelo de microserviço e utilizar melhores recursos de Cloud, mas caso você não possa modificar essa estrutura, você precisará ter alguns cuidados para não gerar mais problemas.

### Premissa

Precisamos ressaltar que um serviço executando em modelo de container não é uma máquina. Container Docker é virtualização a nível de sistema operacional, ou seja, um outro nível de abstração. É tentador utilizar o container Docker como uma máquina, mas evite bravamente, pois isso causará danos indiretos na gerência desse recurso no futuro.

# Supervisor

Se você usará mais do que um processo por container, você precisa de um gerenciador para tal, uma vez que seu container não tem isso por padrão, pois não foi feito pra esse função. Isso normalmente é feito pelo "init" em um sistema "Unix-like" padrão. Lembre-se, já estamos além dos limites das melhores práticas. 

Meu conselho é: Utilize (Supervisord)[http://supervisord.org/index.html]

Supervisor é um software, do tipo cliente/servidor, que permite usuários controlarem múltiplos processos em sistema operacional da família "Unix-like". Os processos controlados por ele são iniciados como sub-processo de sí.

Segue abaixo as vantagens de se utilizar Supervisor:

- **Conveniência**: Ele é responsável por iniciar os sub-processos, ou seja, você pode configurar para todos os múltiplos processos iniciarem quando se iniciar o supervisor, por exemplo.
- **Gerência**: Imagine múltiplos processos distintos, cada um com sua forma de garantir que está funcionando ou não, com diferentes comandos, formas e arquivos. O que o supervisor faz é garantir um determinado padrão e oferecer uma camada extra de abstração no que tange a gerência dos sub-processos.
- **Agrupamento**: Você pode agrupar processos e executar comandos em vários ao mesmo tempo, ao invés de executar diversos comandos manualmente.

# Componentes mais relevantes

- **supervisord** - Esse é o processo responsável por toda gerência dos sub-processos. Ele deverá ser o init 1 em seu container.
- **supervisorctl** - Esse é o client que permite gerência do **supervisord** atráves de comandos simples em seu terminal. A comunicação entre o **supervisorctl** e **supervisord** pode ser feita via *UNIX domain socket* (Arquivo local) ou *TCP socket* (comunicação de rede via endereço ip na rede).

# Como usar

Imaginando que estamos falando de um base image derivada de um "Debian-like"

## Adicionando em seu Dockerfile

Adicione a seguinte instrução:

```
RUN apt-get -qqy update && \
    apt-get -qqy install supervisor && \
    apt-get clean
```

E no final adicione:

```
CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]
```

## Configurando o serviço

Imagine que você deseja iniciar um **nginx**. É importante salientar que esse serviço não pode ser executado como *daemon*, ou seja, ele precisa ser executado em modo *foregroud*. Em termos visuais é aquele processo que quando executado ele não libera o terminal, ou seja, após executado você não consegue dar novos comandos até que ele seja terminado.

Segue abaixo o exemplo do **nginx** executado em foreground:

```
/usr/sbin/nginx -g "daemon off;"
```

Para configurar um processo no supervisord você precisa criar um arquivo dentro da pasta /etc/supervisor.d, que nesse caso vamos usar o exemplo do **nginx**:

/etc/supervisor.d/nginx.conf

```
[program:nginx]
autostart=true
autorestart=false
priority=100
command=/usr/sbin/nginx -g "daemon off;"
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0
```

Abaixo vou explicar linha a linha

```
[program:nginx]
```

Define o nome do sub-processo, que nesse caso será "nginx".

```
autostart=true
```

Informa que uma vez que o supervisor inicie, esse sub-processo será iniciado automaticamente. Essa opção é necessária para modelo de containers, uma vez que nessa abordagem usando docker você não deveria precisar acessar o container pra executar nenhum comando.

```
autorestart=false
```

Informa que o processo **não** será reiniciado em caso de falha, ou seja, caso ele falhe o supervisor não iniciará novamente. Lembre-se que não estamos atuando em uma máquina, isso quer dizer que se o processo falhou, ele precisa ficar desligado e outro recurso deverá ser responsável por verificar que não estar rodando e então desligar ou reiniciar o container inteiro. No caso do Docker com SWARM, isso é feito com a instrução [HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck), com Kubernetes, isso pode ser feito com [liveness/readiness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).

```
priority=100
```

Informa a ordem que o processo será iniciado e desligado. Um número menor indica que o processo começarã primeiro e será desligado por último. 

```
command=/usr/sbin/nginx -g "daemon off;"
```

Informa qual comando utilizando para executar o processo. Informe o caminho absoluto do comando. Você pode usar o comando **which** (# *which nginx*) pra achá-lo.

```
stdout_logfile=/dev/stdout
stderr_logfile=/dev/stderr
```

Informa a saída de padrão e de erro desse processo. Nesse caso estamos enviando para **stdout** e **stderr**, baseado nas melhores práticas de log em container. Uma vez que logs que são enviado para essas saídas são automaticamente gerenciado pela ferramenta de container em questão.

```
stdout_logfile_maxbytes=0
stderr_logfile_maxbytes=0
```

Informa que não há limites para geração de logs. Isso é necessário para [evitar problemas](http://veithen.github.io/2015/01/08/supervisord-redirecting-stdout.html) em alta utilização, mesmo para casos de envio para **stdout** e **stderr**.

## Falando em gerência de logs...

Normalmente processos em modo foreground enviam seus logs para **stdout** e **stderr** por padrão, mas não é o caso do **nginx**, por exemplo, ele precisa receber configurações específicas para tal. 

No arquivo **/etc/nginx/nginx.conf** Modifique as linhas abaixo:

```
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
```

Para as seguintes linhas:

```
access_log /dev/stdout;
error_log /dev/stdout;
```

Com isso por padrão o nginx enviará seu log para **stdout**, mas isso ainda não indica que os logs de cada site também seguirão esse padrão. Você precisa accessar o arquivo /etc/nginx/sites-available/*nome_do_site* e modificar as mesmas linhas informadas anteriormente:

```
access_log ...;
error_log ...;
```

Para as seguintes linhas:

```
access_log /dev/stdout;
error_log /dev/stdout;
```

Ou seja, você precisa entender qual processo está trabalhando e verificar como o log é gerenciado e por fim configurar para que seja enviado para **stdout** e **stderr**.

## Gerenciado processos

Agora que você já tem tudo configurado, você pode acessar seu container e executar o seguinte comando:

```
supervisorctl status
```

Esse comando mostrará todos os processo gerenciados e seu estado, que podem ser as opções abaixo:

- **STOPPED**: O processo foi finalizado ou nunca iniciado
- **STARTING**: O processo recebeu requisição para inicio e está inicializando
- **RUNNING**: O processo está em execução
- **BACKOFF**: O processo entrou no estado *STARTING*, mas foi terminou antes de entrar no estado *RUNNING*
- **STOPPING**: O processo recebeu requisição para término e está terminando
- **EXITED**: O processo saiu do estado *RUNNING* (Fique atento ao *exit code*)
- **FATAL**: O processo não pode ser inicializado com sucesso
- **UNKNOWN**: O processo está em estado desconhecido (Normalmente um erro do supervisor)

<img alt="supervisor-subprocess-transitions" src="http://supervisord.org/_images/subprocess-transitions.png" />

Os comandos mais usados são

```
supervisorctl start nginx
supervisorctl stop nginx
supervisorctl restart nginx
```

Caso você modifique alguma configuração dentro de /etc/supervisor.d/nginx.conf o comando *restart* não será o suficiente para aplicar suas alterações. Você precisa executar o comando abaixo:

```
supervisorctl update
```

Esse comando aplicará todas as mudanças e executará o *restart* nos processos que sofreram alteração.

### Agradecimentos

Meus colegas de trabalho da [Crossover](http://crossover.com) que colaboram em apresentar opções e especialmente para [Evgeny Udalov]() que atuou bastante nesse debate.

### Fontes

- [Supervisor](http://supervisord.org)
- [Redirecting to stdout](http://veithen.github.io/2015/01/08/supervisord-redirecting-stdout.html)
