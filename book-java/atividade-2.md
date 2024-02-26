# Exercícios:


## PARTE 1

Para o exercício proposto, vamos criar uma classe em Java que lê o endereço de e-mail do console, 
e depois chama um método para simular o envio desse e-mail usando `Thread.sleep(5000)` para simular o tempo de envio. 
Introduzindo a interação com o usuário e a simulação de operações de longa duração, como o envio de um e-mail.

```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String email;
        do {
            System.out.println("Digite os endereços de e-mail para envio (digite 'SAIR' para finalizar):");
            email = scanner.nextLine();

            // Apenas enviar o e-mail se o usuário não digitou "SAIR"
            if (!email.equalsIgnoreCase("SAIR")) {
                enviarEmail(email);
            }
            
        } while (!email.equalsIgnoreCase("SAIR"));

        System.out.println("Finalizado. Todos os e-mails foram enviados (ou pelo menos tentamos).");
    }

    private static void enviarEmail(String email) {
        System.out.println("Preparando para enviar e-mail para: " + email);
        try {
            // Simular o tempo de envio do e-mail
            Thread.sleep(5000);
            System.err.println("E-mail enviado com sucesso para: " + email);
        } catch (InterruptedException e) {
            System.err.println("A thread foi interrompida durante a espera.");
        }
    }
}


```

## PARTE 2

Com base no código, altere de modo que o usuário não precise aguardar o envio de cada e-mail individualmente, 
mas sim colete todos os endereços de e-mail em uma lista e posteriormente percorra essa lista chamando o método 
de envio para cada endereço.


## PARTE 3
Separe as responsabilidades para uma classe de EmailSender.







