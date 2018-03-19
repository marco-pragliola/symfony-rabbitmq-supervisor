# symfony-rabbitmq-supervisor

A starting example project for a Symfony 3.4 API with RabbitMQ and Supervisor managed workers

## Description

This is a skeleton for a scalable API with messaging.
The API is based on a typical Symfony 3.4 installation.
The message broking is managed with RabbitMQ and Supervisord.

## What it is all about (for beginners)

### The async way

If your application has to do very resource-intensive tasks (long computations, sending emails, 
processing a picture, other batch things ...) it is always better to handle these operations
**asynchronously** (i.e. without letting the request-response cycle to wait for our task's
completion), so we do not slow down or freeze the server.

To do this, you can employ **workers**, which act as independent processes (thus enabling us to
scale them, control their processing status, parallelize them, balancing the load, put them on
different servers).

In order to know what to do, these workers often rely on one or more **queues**. We could say,
oversimplifying things for clarity's sake, that:

- the main application needs to do a task (f.e. sending a mail)
- we push the task's data (f.e. sender, recipient, message body, ..) in the mail queue,
  thus **producing** a **message**
- workers will **consume** the queue; each worker will get one or more tasks, assigned with
  various strategies (simple round-robin, or balancing the load on some metrics...) and
  work them in parallel
  
