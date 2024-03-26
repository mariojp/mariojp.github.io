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


