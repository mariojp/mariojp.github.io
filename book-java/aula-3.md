---
layout: disciplina
title: JAVA BOOK
subtitle: MARIO JORGE PEREIRA
published: true
---

# Introdução às Threads
Threads permitem a execução de múltiplas tarefas em paralelo dentro de um mesmo processo. Isso é especialmente útil em aplicações que necessitam realizar operações simultâneas, como processamento de dados em background enquanto a interface do usuário permanece responsiva.


```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Programa iniciado.");
        // Executa uma tarefa em uma nova thread
        new Thread(() -> {
            System.out.println("Executando em uma nova thread.");
        }).start();
        System.out.println("Retornou ao método main.");
    }
}
```

## Criação de Threads em Java

Existem duas maneiras principais de criar uma thread em Java: implementando a interface Runnable ou estendendo a classe Thread.

### Implementando a interface Runnable:

```java
public class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Executando em uma thread Runnable.");
    }
}

// Na classe principal
public class Main {
    public static void main(String[] args) {
        Thread myThread = new Thread(new MyRunnable());
        myThread.start();
    }
}

```

### Estendendo a classe Thread:

```java
public class MyThread extends Thread {
    public void run() {
        System.out.println("Executando em uma thread estendida.");
    }
}

// Na classe principal
public class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
    }
}

```

## Dormindo e Interrupção
Threads em Java podem ser pausadas utilizando o método sleep(). A interrupção de uma thread pode ser feita chamando o método interrupt().


```java
public class SleepExample implements Runnable {
    public void run() {
        try {
            Thread.sleep(1000); // Pausa a thread por 1 segundo
            System.out.println("Thread acordada.");
        } catch (InterruptedException e) {
            System.out.println("Thread interrompida.");
        }
    }

    public static void main(String[] args) {
        Thread thread = new Thread(new SleepExample());
        thread.start();
        thread.interrupt(); // Interrompe a thread durante o sono
    }
}

```

## Prática com Threads: Envio de Emails em Paralelo
Podemos usar threads para simular o envio de emails em paralelo. Cada thread simulará o envio de um email.

```java
public class EmailSender implements Runnable {
    private String recipient;

    public EmailSender(String recipient) {
        this.recipient = recipient;
    }

    public void run() {
        System.out.println("Enviando email para " + recipient);
        // Simula o tempo de envio
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            System.out.println("Envio interrompido para " + recipient);
        }
        System.out.println("Email enviado para " + recipient);
    }

    public static void main(String[] args) {
        String[] recipients = {"alice@example.com", "bob@example.com", "charlie@example.com"};

        for (String recipient : recipients) {
            new Thread(new EmailSender(recipient)).start();
        }
    }
}
```

## Classes Anônimas e Lambdas
Java permite a criação de threads de maneira concisa usando classes anônimas e expressões lambda, facilitando a escrita de código para tarefas concorrentes.

### Exemplo com Classe Anônima:

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Executando com classe anônima.");
    }
}).start();

```

### Exemplo com Expressão Lambda:

```java
new Thread(() -> System.out.println("Executando com lambda.")).start();
```

## Soluções e Boas Práticas

A melhor forma de usar threads em Java depende do contexto específico da aplicação, mas algumas práticas e abordagens modernas podem ajudar a otimizar o desempenho, a escalabilidade e a manutenção do código. Aqui estão algumas recomendações:

### Utilizar Pools de Threads
Em vez de criar novas threads manualmente para cada tarefa, é preferível utilizar um pool de threads, como os fornecidos pelo Executor Framework (java.util.concurrent). Os pools de threads ajudam a limitar o número total de threads ativas, reutilizam threads para múltiplas tarefas e reduzem o overhead de criação e destruição de threads.

Exemplo:
```java
ExecutorService executor = Executors.newFixedThreadPool(10); // Cria um pool com 10 threads

executor.execute(() -> {
    System.out.println("Tarefa executada no pool de threads.");
});

executor.shutdown(); // Inicia o processo de encerramento, não aceitando novas tarefas.
```



