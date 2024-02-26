---
layout: disciplina
title: JAVA BOOK
subtitle: MARIO JORGE PEREIRA
published: true
---

# Programação Genérica e Classes de Coleção

As funcionalidades de programação genérica do Java passaram por várias etapas de desenvolvimento. 
As versões iniciais do Java não tinham tipos parametrizados, 
mas tinham classes para representar estruturas de dados comuns. 
Essas classes foram projetadas para trabalhar com Objetos; isto é, elas poderiam conter objetos de qualquer tipo, 
e não havia como restringir os tipos de objetos que poderiam ser armazenados em uma determinada estrutura de dados.
Por exemplo, `ArrayList` originalmente não era um tipo parametrizado, então qualquer `ArrayList` poderia 
conter qualquer tipo de objeto. Isso significa que se `list` fosse um `ArrayList`, então `list.get(i)` 
retornaria um valor do tipo `Object`. 
Se o programador estivesse usando a lista para armazenar Strings, o valor retornado por `list.get(i)` 
teria que ser convertido para tratar como uma string:


```java
String item = (String)list.get(i);
```

Isso ainda é um tipo de programação genérica, já que uma classe pode funcionar para qualquer tipo de objeto, 
mas estava mais próximo em espírito ao Smalltalk do que ao C++, já que não havia como fazer verificações 
de tipo em tempo de compilação. Infelizmente, como no Smalltalk, o resultado é uma categoria de erros que 
aparecem apenas em tempo de execução, em vez de em tempo de compilação. Se um programador assume que todos 
os itens em uma estrutura de dados são strings e tenta processar esses itens como strings, 
um erro de execução ocorrerá se outros tipos de dados foram adicionados inadvertidamente à estrutura de dados. 
No Java, o erro provavelmente ocorrerá quando o programa recuperar um Object da estrutura de dados e tentar 
convertê-lo para o tipo String. Se o objeto não for realmente do tipo String, 
a conversão de tipo ilegal lançará um erro do tipo `ClassCastException`.

O Java 5.0 introduziu tipos parametrizados, o que tornou possível criar estruturas de dados genéricas 
que podem ser verificadas em tempo de compilação em vez de em tempo de execução. 
Por exemplo, se list for do tipo ArrayList<String>, então o compilador só permitirá objetos do tipo String 
serem adicionados à list. Além disso, o tipo de retorno de list.get(i) é String, então a conversão de tipo 
não é necessária. 
As classes parametrizadas do Java são semelhantes às classes de template em C++ 
(embora a implementação seja muito diferente), e sua introdução move o modelo de programação 
genérica do Java mais próximo do C++ e mais distante do Smalltalk. 

Usarei exclusivamente os tipos parametrizados, mas você deve se lembrar de que seu uso não é obrigatório. 
Ainda é legal usar uma classe parametrizada como um tipo não parametrizado, como um ArrayList simples. 
Nesse caso, qualquer tipo de objeto pode ser armazenado na estrutura de dados. 
(Mas, se isso é o que você realmente deseja fazer, seria preferível usar o tipo ArrayList<Object>).

Uma classe parametrizada do Java, existe apenas um arquivo de classe compilado. 
Por exemplo, existe apenas um arquivo de classe compilado, ArrayList.class, para a classe parametrizada ArrayList.
Os tipos parametrizados ArrayList<String> e ArrayList<Integer> usam o mesmo arquivo de classe compilado, 
assim como o tipo ArrayList simples. 
O parâmetro de tipo — String ou Integer — apenas diz ao compilador para limitar o tipo de objeto que pode
ser armazenado na estrutura de dados. O parâmetro de tipo não tem efeito em tempo de execução e 
nem mesmo é conhecido em tempo de execução. 
Diz-se que as informações de tipo são "apagadas" em tempo de execução. 
Essa eliminação de tipo introduz uma certa quantidade de estranheza. 
Por exemplo, você não pode testar "if (list instanceof ArrayList<String>)" 
porque o operador instanceof é avaliado em tempo de execução, e em tempo de execução apenas o ArrayList 
simples existe. Da mesma forma, você não pode fazer cast para o tipo ArrayList<String>. 
Ainda pior, você não pode criar um array que tenha o tipo base ArrayList<String> usando o operador new, 
como em "new ArrayList<String>[N]". Isso ocorre porque o operador new é avaliado em tempo de execução, 
e em tempo de execução não existe algo como "ArrayList<String>"; 
apenas o tipo ArrayList não parametrizado existe em tempo de execução. 
(No entanto, embora você não possa ter um array de ArrayList<String>, você pode ter um ArrayList de 
ArrayList<String> — com o tipo escrito como ArrayList<ArrayList<String>> — que é tão bom ou melhor.)

Felizmente, a maioria dos programadores não precisa lidar com tais problemas, 
já que eles aparecem apenas em programação bastante avançada. 
A maioria das pessoas que usam tipos parametrizados não encontrará os problemas, 
e elas obterão os benefícios da programação genérica segura de tipo com pouca dificuldade.

Vale a pena notar que, se o parâmetro de tipo em um tipo parametrizado puder ser deduzido pelo compilador, 
então o nome do parâmetro de tipo pode ser omitido. 
Por exemplo, a palavra "String" é opcional no construtor na seguinte declaração, 
porque o ArrayList que é criado deve ser um ArrayList<String> para corresponder ao tipo da variável:

```java
ArrayList<String> palavras = new ArrayList<>();
```

## O Java Collection Framework (JCF)

Java oferece uma série de tipos parametrizados que implementam estruturas de dados comuns, 
agrupadas no que chamamos de Java Collection Framework (JCF). 
O JCF divide-se em duas categorias principais: coleções e mapas.

### Coleções
Uma coleção é simplesmente um agrupamento de objetos. No JCF, 
coleções são representadas pela interface parametrizada `Collection<T>`, onde "T" representa qualquer 
tipo de objeto, exceto tipos primitivos.

Existem dois tipos principais de coleções:
- **Listas (`List<T>`):** Sequências lineares de elementos onde cada elemento tem uma posição. As listas permitem elementos duplicados e são ordenadas pela posição dos elementos.
- **Conjuntos (`Set<T>`):** Coleções que não permitem elementos duplicados. Ao contrário das listas, os conjuntos geralmente não são ordenados de maneira específica.

### Mapas
Mapas (`Map<K,V>`) associam chaves a valores, semelhante a como um dicionário associa definições a palavras. As chaves são únicas, mas os valores associados a elas podem ser duplicados.

### Operações Comuns em Coleções
A interface `Collection<T>` define operações básicas aplicáveis a qualquer tipo de coleção, incluindo:

- `size()`: Retorna o número de elementos na coleção.
- `isEmpty()`: Verifica se a coleção está vazia.
- `add(T elemento)`: Adiciona um elemento à coleção.
- `remove(Object o)`: Remove um elemento da coleção.
- `contains(Object o)`: Verifica se um elemento está na coleção.
- `clear()`: Remove todos os elementos da coleção.

### Implementações comuns de Listas e Conjuntos
- **ArrayList<T>**: Implementa a interface `List<T>` e oferece uma lista baseada em um array que é redimensionável.
- **HashSet<T>**: Implementa a interface `Set<T>` e usa uma tabela hash para armazenar os elementos, oferecendo operações rápidas de adição, remoção e verificação de existência.

### Eficiência e Uso
A eficiência das operações varia de acordo com a implementação específica da coleção. Por exemplo, `ArrayList` oferece acesso rápido a elementos por índice, enquanto `LinkedList` é mais eficiente para adicionar ou remover elementos no início ou meio da lista. Escolher a implementação correta é crucial para a eficiência do programa.

### Exemplo de Uso

```java
// Criando e manipulando uma ArrayList de Strings
ArrayList<String> frutas = new ArrayList<>();
frutas.add("Maçã");
frutas.add("Banana");
frutas.add("Cereja");

// Iterando sobre a lista
for (String fruta : frutas) {
    System.out.println(fruta);
}

// Criando e manipulando um HashSet de inteiros
HashSet<Integer> numeros = new HashSet<>();
numeros.add(1);
numeros.add(2);
numeros.add(3);

// Verificando a presença de um elemento
if (numeros.contains(2)) {
    System.out.println("O número 2 está no conjunto.");
}
```

O JCF oferece uma ampla gama de estruturas de dados e algoritmos genéricos para facilitar a programação em Java, 
permitindo que desenvolvedores escolham as coleções mais adequadas para suas necessidades específicas.

Métodos de Fábrica Estáticos para Coleções: Introduzidos para List, Set, Map e Map.Entry, esses métodos permitem criar coleções imutáveis de maneira mais concisa.

Exemplo para List:
```java
List<String> friends = List.of("Alice", "Bob", "Charlie");
```
Exemplo para Set:
```java
Set<String> countries = Set.of("Brazil", "France", "Japan");
```
Exemplo para Map:
```java
Map<String, Integer> ageOfFriends = Map.of("Alice", 30, "Bob", 25, "Charlie", 22);
```
Melhorias Locais na Inferência de Tipo com var: Embora não seja uma mudança direta na API de coleções, o uso de var pode simplificar a declaração de variáveis para coleções.
```java
var list = List.of("Java", "Kotlin", "Scala");
```
Método stream() em Map: Facilita a criação de streams diretamente de entradas de mapas, sem a necessidade de usar entrySet() ou keySet().

Exemplo:
```java
Map<String, Integer> map = Map.of("a", 1, "b", 2, "c", 3);
map.stream()
   .forEach(entry -> System.out.println(entry.getKey() + ":" + entry.getValue()));
```
