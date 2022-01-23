+++
title = "Do you need to execute more than one process per container?"
date = "2018-07-21"
draft = false
Categories = ["english", "devops", "docker", "supervisord"]
Tags = ["english", "devops", "docker", "supervisord"]
+++

## TL;DR

You need more than one process in a container, so you need to follow some best practices to your container does not become just a "lightweight machine".

### Problem

In my current job, I need to run many processes in the same container. This is a warning sign of problem in your service architecture and this will be a problem for a better usage of Cloud features, but if you can't modify this structure, you shouldn't introduce new problems on your project.

### Premise

We need to point out that a service running in container model isn't a machine. Container Docker is virtualization at the operating system level that is another level of abstraction. It is tempting to use the Docker container as a machine, but avoid bravely because this will cause indirect damage to resource management resource in the future.

# Supervisor

If you will use more than one process per container, you need a process manager, since your container does not have this by default because it wasn't made for this usage. This is usually done by "init" in a standard "Unix-like" operation system. Remember, we are already beyond the limits of best practices.

My advice is: (Supervisord)[http://supervisord.org/index.html]

Supervisor is a software client/server type that allows you control multiple processes in the Unix-like family operating system. Each process managed by supervisor is started as a subprocess.

Here are the advantages of using Supervisor:

- **Convenience**: It is responsible for starting subprocesses. You can configure for all multiple processes to start when you start the supervisor, for example.
- **Management**: When you have multiple distinct processes, different ways to ensuring that it is working or not, with distinct commands, shapes, and files. Supervisor is responsible for enforcing standard by ensuring a layer of process management abstraction.
- **Grouping**: You can group processes and execute commands to the group rather than executing manually one by one.

# Most relevant components

- **supervisord** - This is the process responsible for all subprocess management. It should be pid 1 in your container.
- **supervisorctl** - This is the client responsible for **supervisord** management through simple commands in your terminal. Communication between the **supervisorctl** and **supervisord** can be done by UNIX domain socket (local file) or TCP socket (network communication).

# How to use

We will use "Debian-like" base image.

## Adding in your Dockerfile

Add the following instructions:

```
RUN apt-get -qqy update && \
    apt-get -qqy install python-pip supervisor && \
    pip install supervisor-stdout && \
    apt-get clean
```

Add in the bottom:

```
CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]
```

## Setting up the supervisor

Open the **/etc/supervisor/supervisord.conf** file and in the **[supervisorord]** session you must add the following line:

```
nodaemon=true
``` 

Now create a new session on **/etc/supervisor/supervisord.conf** file:

```
[eventlistener:stdout]
priority = 1
command = supervisor_stdout
buffer_size = 100
events = PROCESS_LOG
result_handler = supervisor_stdout:event_handler
```

[Eventlistener](http://supervisord.org/configuration.html#eventlistener-x-section-settings) is a **supervisor** feature that aims to enable a process to listen to [subprocess events](http://supervisord.org/events.html#events) and handle it properly.

[supervisor_stdout](https://github.com/coderanger/supervisor-stdout) is a script created for redirecting sub process logs to the standard and error output of the supervisor.

The advantage of eventlistener usage lies in the fact that logs treated in this way are displayed in the supervisor log with a prefix, making it easier to figure out which subprocess is that log line.

## Setting up the process

We will use **nginx** as an example. It is important to note that this service can not be run as a daemon, it must be run in foreground mode. That means when you execute a process and it "lock" your terminal and you can't run new commands until it is finished.

Here is the example of **nginx** running in foreground:

```
/usr/sbin/nginx -g "daemon off;"
```

To set up a process in the supervisor you need to create a file inside the /etc/supervisor.d folder:

/etc/supervisor.d/nginx.conf

```
[program:nginx]
autostart=true
autorestart=false
priority=100
command=/usr/sbin/nginx -g "daemon off;"
stdout_events_enabled = true
stderr_events_enabled = true
```

Explaining line by line

```
[program:nginx]
```

Informs the name of subprocess.

```
autostart=true
```

Informs that once the supervisor starts, this subprocess will start automatically. This option is required for containers model, since you shouldn't have to access the container to execute any commands.

```
autorestart=false
```

Informs that your subprocess won't restart in case of failure. Remember that we aren't using a machine, it means that if the process failed, it needs to be turned off and another resource should be responsible for. If you use Docker with [Swarm](https://docs.docker.com/engine/swarm/) [HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck) instruction can handle this, but if your Docker cluster is Kubernetes you should use [liveness/readiness probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).

```
priority=100
```

Informs the order that the process will start and shut down. A smaller number indicates that the process will start first and will be turned off last.

```
command=/usr/sbin/nginx -g "daemon off;"
```

Informs which command to use to run the process. Enter the absolute path of the command. You can use the **which** (# which nginx ) command to find it.

```
stdout_events_enabled = true
stderr_events_enabled = true
```

Informs that the standard output or error will issue an event, which will be captured by the **eventlisterner** previously configured in that article. In this case the **eventlisterner** will sent to the stdout and stderr of the supervisor, with due prefix, based on the best practices of log in container. Remember that logs sent to these outputs are automatically managed by the container tool.

## Log management

Usually processes in foreground send their logs to stdout and stderr by default, but that is not the case for all logs from nginx, for example, it needs to receive specific settings for it.

In the file **/etc/nginx/nginx.conf** you need to modify the lines below:

```
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
```

For the following lines:

```
access_log /dev/stdout;
error_log /dev/stdout;
```

By default, nginx will send its log to stdout, but that still doesn't indicate that the logs for each site will also follow this pattern. You need to access the /etc/nginx/sites-available/*site_name* file and modify the same lines as previously reported:

```
access_log ...;
error_log ...;
```

For the following lines:

```
access_log /dev/stdout;
error_log /dev/stdout;
```

That is, you need to understand what process is working and verify how the log is managed and finally set it to be sent to **stdout** and **stderr**.

## Managing subprocesses

Now that you have everything configured, you can access your container and execute the following command:

```
supervisorctl status
```

This command will show all managed processes and their status, which can be the following:

- **STOPPED**: Subprocess terminated or never started
- **STARTING**: Subprocess has been requested to start and it is initializing
- **RUNNING**: Subprocess is running
- **BACKOFF**: Subprocess entered the *STARTING* state, but was terminated before entering the *RUNNING* state
- **STOPPING**: Subprocess has received a request to terminate and is terminating
- **EXITED**: Subprocess has exited the RUNNING state (Keep an eye on the *exit code* )
- **FATAL**: Subprocess can't be successfully initialized
- **UNKNOWN**: Subprocess is in an unknown state (Normally a supervisor error)

<img alt="supervisor-subprocess-transitions" src="http://supervisord.org/_images/subprocess-transitions.png" />

The most commonly used commands are:

```
supervisorctl start nginx
supervisorctl stop nginx
supervisorctl restart nginx
```

If you modify any configuration within /etc/supervisor.d/nginx.conf the *restart* command will not be enough to apply your changes. You need to execute the command below:

```
supervisorctl update
```

This command will apply all the changes and will perform the *restart* on processes that have changed.

### Thanks

My co-workers from Crossover](http://crossover.com) who collaborate in presenting options and especially to [Evgeny Udalov](https://www.linkedin.com/in/evgenyudalov/) who played a lot in this debate.

### Sources

- [Supervisor](http://supervisord.org)
- [Redirecting to stdout](http://veithen.github.io/2015/01/08/supervisord-redirecting-stdout.html)
- [Multi-process Docker Service](https://rcaguilar.wordpress.com/2017/11/06/multi-process-docker-service/)
