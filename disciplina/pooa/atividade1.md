# ATIVIDADE 1

## Questão 1: Encapsulamento (Getters e Setters)

### CONTEXTO 
Encapsulamento é um princípio fundamental da orientação a objetos que consiste em esconder os detalhes internos de 
uma classe e expor apenas o necessário por meio de uma interface pública controlada. 
Em Java, isso é alcançado definindo-se atributos como privados e fornecendo métodos públicos 
para acessá-los (getters) ou modificá-los (setters). 
Dessa forma, a classe tem controle sobre como seus dados são manipulados, 
possibilitando validações e garantindo a integridade do estado do objeto.

### PROBLEMA 
Implemente uma classe **ContaBancaria** usando encapsulamento para proteger seu estado interno.
A classe deve possuir um atributo privado **saldo** (por exemplo, do tipo double) 
e métodos públicos **depositar(double valor)** e **sacar(double valor)**. 
O método depositar deve adicionar o valor ao saldo se for positivo; 
o método sacar deve subtrair o valor do saldo apenas se houver saldo suficiente, 
garantindo que o saldo nunca se torne negativo. 
Forneça também um método getSaldo() para consultar o saldo atual. 

Demonstre o uso da classe criando uma instância, realizando operações de depósito e saque, 
e exibindo o saldo após essas operações como com o codogo a seguir.

```java

// Classe de teste para demonstrar o uso de ContaBancaria

class TesteContaBancaria {
    public static void main(String[] args) {
        ContaBancaria conta = new ContaBancaria(1000.0); // saldo inicial de 1000
        conta.depositar(500.0);
        System.out.println("Saldo após depósito: " + conta.getSaldo()); // Esperado 1500.0

        // Tentativa de saque acima do saldo disponível
        boolean resultadoSaque = conta.sacar(2000.0);
        System.out.println("Saque de 2000 realizado? " + resultadoSaque); // Esperado false
        System.out.println("Saldo após tentativa de saque: " + conta.getSaldo()); // Saldo permanece 1500.0

        // Saque dentro do limite do saldo
        boolean resultadoSaque2 = conta.sacar(1000.0);
        System.out.println("Saque de 1000 realizado? " + resultadoSaque2); // Esperado true
        System.out.println("Saldo final: " + conta.getSaldo()); // Esperado 500.0
    }
}

```

## Questão 2: Encapsulamento e Herança (modificador protected)

### CONTEXTO 

Além do uso de atributos privados, Java oferece o modificador de acesso protected para cenários de herança. 
Um membro protected de uma classe é visível para todas as subclasses (e também para classes do mesmo pacote), 
permitindo que classes derivadas acessem diretamente esses atributos ou métodos. Em contraste, 
um membro private permanece inacessível às subclasses, exigindo métodos acessores (getters/setters) para interação. 
O uso de protected proporciona um meio termo entre o encapsulamento estrito de private e a acessibilidade
total de public, sendo útil quando queremos compartilhar parte da implementação com as subclasses, 
mas ainda esconder dos demais componentes externos.

### PROBLEMA 
Implemente duas classes para observar o 
efeito de diferentes níveis de acesso. Crie uma classe base Animal com um atributo nome. 
Inicialmente, defina nome como protected. Em seguida, crie uma subclasse 
Cachorro que estende Animal e adiciona um método latir() que exibe uma mensagem usando 
o nome do animal (por exemplo, imprimindo algo como "<nome> está latindo!"). 
No método main, instancie um objeto Cachorro e chame seu método latir(). 
Verifique que, com nome declarado como protected em Animal, a subclasse Cachorro consegue acessar nome diretamente. 
Discuta o que aconteceria se nome fosse declarado como private na classe Animal (dica: a subclasse não conseguiria acessar 
o nome diretamente e seria necessário usar um getter público na classe base para obter o nome).


```java
class TesteAnimal {
    public static void main(String[] args) {
        Cachorro dog = new Cachorro("Rex");
        dog.latir();  // Saída esperada: "Rex está latindo!"
    }
}
```

## Questão 3:  Classes Abstratas (Conceito e Implementação)


### CONTEXTO 
Uma classe abstrata é uma classe que não pode ser instanciada diretamente, servindo como modelo para outras classes. Ela pode definir métodos abstratos (sem corpo, apenas assinatura) que obrigatoriamente devem ser implementados pelas subclasses concretas, além de poder conter métodos concretos (com implementação) compartilhados. As classes abstratas permitem estabelecer uma generalização de conceitos, garantindo que certas funcionalidades existam nas subclasses, ao mesmo tempo em que adiamos detalhes da implementação para essas classes derivadas.

### PROBLEMA 
Crie uma classe abstrata Forma que declare um método abstrato calcularArea(). Em seguida, implemente duas subclasses concretas de Forma: Retangulo e Circulo. A classe Retangulo deve ter atributos largura e altura e implementar o método calcularArea() retornando o produto largura * altura. A classe Circulo deve ter o atributo raio e implementar calcularArea() retornando Math.PI * raio * raio. Forneça construtores adequados para inicializar os atributos de cada subclasse. Por fim, escreva um método main que instancia um Retangulo e um Circulo, ambos referenciados pelo tipo abstrato Forma, e exibe suas áreas calculadas chamando calcularArea() em cada um. Isso deve demonstrar o polimorfismo proporcionado pelas classes abstratas.

```java
// Classe de teste para demonstrar o uso das classes abstratas e suas subclasses
class TesteFormas {
    public static void main(String[] args) {
        Forma f1 = new Retangulo(5.0, 4.0);
        Forma f2 = new Circulo(3.0);
        System.out.println("Área do Retângulo: " + f1.calcularArea());  // 5*4 = 20.0
        System.out.println("Área do Círculo: " + f2.calcularArea());    // π*3^2 ≈ 28.27
    }
}
```

## Questão 4: Classe Abstrata Avançada (Modelo de Funcionário)

### CONTEXTO 
Classes abstratas podem combinar membros abstratos e concretos para fornecer uma estrutura flexível às subclasses. Isso significa que além de declarar métodos abstratos (a serem implementados pelas subclasses), a classe abstrata pode implementar diretamente alguns métodos e incluir atributos comuns. Essa estratégia permite compartilhar código entre as subclasses (evitando repetição) e, ao mesmo tempo, garantir que partes específicas sejam definidas individualmente em cada subclasse. Em outras palavras, a classe abstrata pode fornecer um "esqueleto" de funcionalidade, deixando detalhes para as subclasses preencherem.

### PROBLEMA 
Desenvolva uma hierarquia de classes para representar funcionários com diferentes tipos de bonificação. Crie uma classe abstrata Funcionario que possui um campo salarioBase (por exemplo, double) e um método abstrato calcularBonus(). Essa classe também deve ter um método concreto calcularSalarioTotal(), que retorna a soma do salário base com o bônus (obtido chamando o método abstrato calcularBonus()). Em seguida, implemente duas subclasses concretas de Funcionario: Gerente e Vendedor. A classe Gerente deve calcular o bônus como 20% do salário base, enquanto a classe Vendedor calcula o bônus como 10% do salário base. Forneça construtores para inicializar o salarioBase em ambas as subclasses. No método main, crie instâncias de Gerente e Vendedor com salários base distintos e exiba, para cada um, o salário total calculado (base + bônus), demonstrando o reuso da lógica fornecida pela classe abstrata.

```java 
class TesteFuncionarios {
    public static void main(String[] args) {
        Funcionario func1 = new Gerente(5000.0);
        Funcionario func2 = new Vendedor(3000.0);
        System.out.println("Salário total do Gerente: " + func1.calcularSalarioTotal());  // 5000 + 1000
        System.out.println("Salário total do Vendedor: " + func2.calcularSalarioTotal()); // 3000 + 300
    }
}
```



## Questão 5: Interfaces (Definindo Contratos)

### CONTEXTO 
Uma interface é um conjunto de declarações de métodos (e constantes) que estabelece um contrato que as classes podem implementar. Ao implementar uma interface, uma classe se compromete a fornecer código para todos os métodos definidos por ela. Interfaces permitem que classes de hierarquias diferentes compartilhem comportamentos através de um tipo comum, e uma classe pode implementar múltiplas interfaces (diferentemente da herança de classes, que é única), promovendo flexibilidade e polimorfismo.

### PROBLEMA 
Defina uma interface chamada Voador contendo um método void voar(). Em seguida, implemente duas classes distintas que realizam esse contrato: Passaro e Aviao, ambas implementando Voador. Na classe Passaro, faça o método voar() exibir uma mensagem indicando que o pássaro está voando (por exemplo, usando as asas). Na classe Aviao, faça voar() exibir uma mensagem apropriada para um avião (por exemplo, informando que está decolando ou voando com motores). Por fim, no método main, crie instâncias de Passaro e Aviao e invoque o método voar() em cada uma, preferencialmente através de variáveis do tipo da interface Voador, evidenciando que objetos diferentes podem ser tratados de forma uniforme pelo tipo de interface.

```java
class TesteVoo {
    public static void main(String[] args) {
        Voador v1 = new Passaro();
        Voador v2 = new Aviao();
        v1.voar();  // Chama voar() do Passaro
        v2.voar();  // Chama voar() do Aviao
    }
}

```


## Questão 6: Interfaces Avançadas (Implementação Múltipla)

### CONTEXTO 
Uma classe pode implementar várias interfaces, viabilizando uma forma de "herança múltipla" de comportamentos. Isso significa que um único objeto pode ser tratado como instância de diferentes interfaces, agregando capacidades diversas. Além disso, desde o Java 8, interfaces podem declarar métodos com implementação padrão (default methods), fornecendo alguma funcionalidade pronta para as classes que as implementam. Caso uma classe implemente duas interfaces que definem o mesmo método default, ela deverá sobrescrevê-lo, resolvendo explicitamente o conflito. Esse mecanismo torna as interfaces ainda mais flexíveis, embora seu uso principal continue sendo definir contratos que as classes devem cumprir.

### PROBLEMA 
Demonstre o uso de múltiplas interfaces em uma única classe. Crie duas interfaces, Corredor (com um método void correr()) e Nadador (com um método void nadar()). Então, implemente uma classe Triatleta que implementa tanto Corredor quanto Nadador, fornecendo as implementações dos métodos correr() e nadar(). No método main, instancie um Triatleta e invoque ambos os métodos, mostrando que ele pode realizar as duas ações. Além disso, mostre que o objeto Triatleta pode ser referenciado tanto como Corredor quanto como Nadador (atribuindo-o a variáveis desses tipos) e, ainda assim, executar corretamente o método correspondente. Explique as vantagens dessa abordagem em comparação com a herança simples de classes (por exemplo, flexibilidade para compor comportamentos diversos).


```java
class TesteAtleta {
    public static void main(String[] args) {
        Triatleta atleta = new Triatleta();
        atleta.correr();
        atleta.nadar();

        // O mesmo objeto pode ser visto como Corredor ou Nadador
        Corredor comoCorredor = atleta;
        Nadador comoNadador = atleta;
        comoCorredor.correr(); // via referência Corredor
        comoNadador.nadar();   // via referência Nadador
    }
}

```

## Questão 7: Coleções (List e ArrayList)

### CONTEXTO 
O framework de coleções do Java fornece estruturas de dados prontas para uso, como listas, conjuntos e mapas, que facilitam o armazenamento e manipulação de grupos de objetos. Dentre elas, a interface List representa uma coleção ordenada que permite elementos duplicados e acesso por índice. Uma implementação comum de List é a classe ArrayList, que funciona como um array dinâmico (redimensionável automaticamente conforme elementos são adicionados ou removidos). Usando ArrayList, podemos adicionar e remover elementos facilmente e iterar sobre eles com loops, sem nos preocupar com limites de tamanho fixo.

### PROBLEMA 
Escreva um programa em Java que utilize uma ArrayList para gerenciar uma lista de nomes. Primeiro, crie uma lista vazia de String usando ArrayList. Em seguida, adicione alguns nomes à lista (por exemplo: "Ana", "Bruno", "Carlos", "Daniela"). Exiba todos os nomes presentes na lista após as inserções. Então, remova um nome específico (por exemplo, "Bruno") da lista. Por fim, exiba novamente o conteúdo da lista para verificar que o nome foi removido com sucesso. Use os métodos da lista (add, remove, etc.) e um laço de repetição para mostrar os elementos.


## Questão 8: Coleções Avançadas (Map e HashMap)

### CONTEXTO 
Além de listas e conjuntos, o framework de coleções do Java inclui mapas (Maps), que armazenam pares chave-valor. A interface Map<K,V> define esse comportamento, e uma implementação popular é a classe HashMap. Um HashMap usa uma tabela de espalhamento (hash table) para organizar os dados, permitindo inserções e buscas rápidas por chave. Cada chave no mapa é única, e está associada a exatamente um valor. Os mapas são muito úteis para estabelecer relacionamentos entre conjuntos de dados, por exemplo, para contar frequências de palavras, associar identificadores a objetos, etc.

### PROBLEMA 
Implemente um programa que conte a frequência de cada palavra em uma frase. Para isso, utilize um HashMap<String, Integer> onde a chave será a palavra e o valor o contador de ocorrências. Por exemplo, dada a frase "o tempo voa o tempo é relativo", o programa deve determinar quantas vezes cada palavra aparece (no caso, "o" aparece 2 vezes, "tempo" 2 vezes, "voa" 1 vez, "é" 1 vez, "relativo" 1 vez). Sua solução deve percorrer cada palavra da frase, usar o mapa para atualizar a contagem (iniciando em 1 quando a palavra surgir pela primeira vez e incrementando o contador nas ocorrências subsequentes) e, ao final, exibir cada palavra acompanhada de sua frequência.

Saída Esperada para frase "o tempo voa o tempo é relativo"
```java
o: 2  
tempo: 2  
voa: 1  
é: 1  
relativo: 1  

```

## Questão 9: Threads (Concorrência Básica com Runnable)

### CONTEXTO 
Threads são unidades de execução que permitem rodar várias tarefas simultaneamente dentro de um programa (paralelismo ou multithreading). Em Java, podemos criar threads de duas maneiras principais: estendendo a classe Thread ou implementando a interface Runnable. A abordagem com Runnable envolve definir a tarefa dentro do método run() e então passar essa instância para um objeto Thread. Ao chamar thread.start(), uma nova linha de execução é iniciada e o método run() é executado em paralelo ao fluxo principal. O agendamento das threads é feito pela JVM e pelo sistema operacional, podendo intercalar a execução das mesmas de forma imprevisível (mas gerenciada).

### PROBLEMA 
Implemente um programa que execute duas tarefas concorrentes usando threads. Para isso, crie uma classe que implemente Runnable cujo método run() imprime uma contagem de 1 a 5, pausando por meio segundo entre cada número (use Thread.sleep(500)). No método main, instancie duas tarefas Runnable (podendo usar a mesma classe para ambas, diferenciando-as por um identificador, como um nome da thread) e crie duas instâncias de Thread, passando cada Runnable para seu construtor. Inicie ambas as threads com o método start() e observe como as mensagens de saída das duas tarefas podem se intercalar, demonstrando a execução paralela. Certifique-se de tratar a exceção de interrupção que pode ser lançada pelo sleep.

```java
class TesteThreads {
    public static void main(String[] args) {
        Runnable tarefa1 = new MinhaTarefa("A");
        Runnable tarefa2 = new MinhaTarefa("B");

        Thread t1 = new Thread(tarefa1);
        Thread t2 = new Thread(tarefa2);

        t1.start();
        t2.start();

        System.out.println("Threads iniciadas!");
    }
}

```

## Questão 10: Threads Avançadas (Sincronização de Recursos Compartilhados)
### CONTEXTO 
Quando múltiplas threads acessam e modificam um mesmo recurso compartilhado sem coordenação, surgem as chamadas condições de corrida (race conditions). Isso pode levar a resultados incorretos ou comportamento imprevisível, pois as operações das threads podem interferir umas nas outras. Para evitar esse problema, Java oferece mecanismos de sincronização. A palavra-chave synchronized, por exemplo, pode ser usada em métodos ou blocos de código para garantir que apenas uma thread por vez execute aquela seção crítica, sob um determinado monitor (lock). Com sincronização adequada, asseguramos a integridade dos dados compartilhados, pois sequências de operações críticas tornam-se atômicas do ponto de vista das threads (ou seja, uma thread completa sua operação antes que outra entre na mesma região sincronizada).
### PROBLEMA 
Implemente um exemplo de condição de corrida e sua correção. Crie uma classe Contador com um campo inteiro count e um método incrementar() que incrementa esse campo. Em seguida, crie múltiplas threads que chamem incrementar() muitas vezes (por exemplo, cada thread incrementa o contador em um laço de 1000 iterações). Sem nenhum mecanismo de sincronização, é provável que o valor final de count seja menor do que o esperado (total de incrementos), devido a operações de incremento interrompidas no meio pelas threads (resultando em algumas contagens "perdidas"). Para resolver isso, modifique o método incrementar() para ser synchronized. Execute novamente as threads e verifique que agora o valor final de count condiz com o esperado. No código, mostre o valor final do contador após todas as threads terem terminado suas atualizações.

```java
class TesteSincronizacao {
    public static void main(String[] args) {
        Contador contadorCompartilhado = new Contador();
        int numThreads = 4;
        int incPorThread = 1000;

        Thread[] threads = new Thread[numThreads];
        // Cria e inicia 4 threads, cada uma executando 1000 incrementos
        for (int i = 0; i < numThreads; i++) {
            threads[i] = new Thread(new TarefaIncremento(contadorCompartilhado, incPorThread));
            threads[i].start();
        }

        // Aguarda todas as threads terminarem (join)
        for (int i = 0; i < numThreads; i++) {
            try {
                threads[i].join();
            } catch (InterruptedException e) {
                System.out.println("Thread interrompida durante join.");
            }
        }

        // Exibe o resultado esperado e o obtido
        int esperado = numThreads * incPorThread;
        int obtido = contadorCompartilhado.getCount();
        System.out.println("Valor esperado: " + esperado);
        System.out.println("Valor obtido: " + obtido);
    }
}
```
