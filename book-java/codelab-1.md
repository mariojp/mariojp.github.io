---
layout: disciplina
title: CODELAB 1
subtitle: COMPONENTE DE TAREFAS
published: true
---


# Codelab: Construindo um Compoente de Gerenciador de Tarefas em Java

## Introdução
Neste codelab, você aprenderá a criar um componente de software básico em Java para gerenciar uma lista de tarefas. Você implementará um sistema que permite adicionar, completar, remover e listar tarefas através de um aplicativo de console.

Acesse o link: https://classroom.github.com/a/rH9A4tRh

Clone o repositorio

```
git clone <url-do-projeto>
```

## Passo 1: Criando a Classe Task

Crie um arquivo chamado Task.java.

Adicione o seguinte código ao arquivo:
```java
public class Task { 
    private String name;
    private boolean isComplete;

    public Task(String name) {
        this.name = name;
        this.isComplete = false;
    }

    public String getName() {
        return name;
    }

    public boolean isComplete() {
        return isComplete;
    }

    public void complete() {
        this.isComplete = true;
    }

    @Override
    public String toString() {
        return "Task{" +
               "name='" + name + '\'' +
               ", isComplete=" + isComplete +
               '}';
    }
}

```

## Passo 2: Criando a Classe TaskManager

Crie um arquivo chamado TaskManager.java.

Insira o código a seguir:

```java
import java.util.ArrayList;
import java.util.List;

public class TaskManager {
    private List<Task> tasks;

    public TaskManager() {
        this.tasks = new ArrayList<>();
    }

    public void addTask(Task task) {
        tasks.add(task);
    }

    public void completeTask(String taskName) {
        for (Task task : tasks) {
            if (task.getName().equals(taskName)) {
                task.complete();
                return;
            }
        }
    }

    public void removeTask(String taskName) {
        tasks.removeIf(task -> task.getName().equals(taskName));
    }

    public List<Task> getTasks() {
        return tasks;
    }
}
```

Use:
```
mvn deploy
```


## Passo 3: Crie um novo projeto e use a Biblioteca em Projetos Maven

```xml
<repositories>
    <repository>
        <id>github</id>
        <name>My Maven Repo on GitHub</name>
        <url>https://raw.githubusercontent.com/[repo]/</url>
    </repository>
</repositories>
```

Usando a biblioteca
```xml
<dependencies>
    <dependency>
        <groupId>br.com.mariojp.easytask</groupId>
        <artifactId>easytask</artifactId>
        <version>1.0</version>
    </dependency>
</dependencies>
```


```java
import java.util.Scanner;

public class TaskApp {
    private static final Scanner scanner = new Scanner(System.in);
    private static final TaskManager taskManager = new TaskManager();

    public static void main(String[] args) {
        boolean running = true;
        while (running) {
            System.out.println("\nGerenciador de Tarefas:");
            System.out.println("1. Adicionar Tarefa");
            System.out.println("2. Completar Tarefa");
            System.out.println("3. Remover Tarefa");
            System.out.println("4. Listar Tarefas");
            System.out.println("5. Sair");
            System.out.print("Escolha uma opção: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline left-over

            switch (choice) {
                case 1:
                    addTask();
                    break;
                case 2:
                    completeTask();
                    break;
                case 3:
                    removeTask();
                    break;
                case 4:
                    listTasks();
                    break;
                case 5:
                    running = false;
                    break;
                default:
                    System.out.println("Opção inválida, tente novamente.");
            }
        }
    }

    private static void addTask() {
        System.out.print("Nome da Tarefa: ");
        String name = scanner.nextLine();
        Task task = new Task(name);
        taskManager.addTask(task);
        System.out.println("Tarefa adicionada com sucesso.");
    }

    private static void completeTask() {
        System.out.print("Nome da Tarefa a completar: ");
        String name = scanner.nextLine();
        taskManager.completeTask(name);
        System.out.println("Tarefa completada.");
    }

    private static void removeTask() {
        System.out.print("Nome da Tarefa a remover: ");
        String name = scanner.nextLine();
        taskManager.removeTask(name);
        System.out.println("Tarefa removida.");
    }

    private static void listTasks() {
        System.out.println("Tarefas:");
        for (Task task : taskManager.getTasks()) {
            System.out.println(task);
        }
    }
}

```


## Desafios Adicionais

Adicione a funcionalidade para editar o nome de uma tarefa.

Implemente a persistência de tarefas usando arquivos ou banco de dados.

Crie uma interface gráfica de usuário (GUI) para o aplicativo.

