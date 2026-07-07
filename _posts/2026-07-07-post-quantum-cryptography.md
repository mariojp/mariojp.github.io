---
layout: post
title: "OpenSSH e o alerta pós-quântico: o que significa “store now, decrypt later”?"
subtitle: "Uma introdução didática à criptografia, à computação quântica e à criptografia pós-quântica"
tags: [OpenSSH, SSH, criptografia, computação quântica, pós-quântica, segurança]
---

![Alerta do OpenSSH sobre troca de chaves não pós-quântica](/img/openssh-alerta-pos-quantico.png){: .center-block :}

Ao acessar um servidor por SSH, você pode encontrar uma mensagem semelhante a esta:

```text
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
```

Em tradução livre:

> **Aviso:** a conexão não está usando um algoritmo pós-quântico para a troca de chaves.  
> Esta sessão pode estar vulnerável a ataques do tipo **“armazene agora e descriptografe depois”**.

A mensagem parece indicar que o servidor foi invadido ou que a criptografia deixou de funcionar. **Não é isso que o aviso significa.**

{: .box-note}
**Em uma frase:** a sessão continua criptografada contra os ataques convencionais conhecidos, mas a troca de chaves usada na conexão pode não proteger os dados contra um computador quântico suficientemente poderoso no futuro.

O alerta passou a ser exibido pelo OpenSSH para incentivar a adoção de algoritmos de troca de chaves resistentes a ataques quânticos. Segundo a documentação oficial, o OpenSSH oferece troca de chaves pós-quântica por padrão desde a versão 9.0, passou a oferecer `mlkem768x25519-sha256` na versão 9.9, tornou esse algoritmo o padrão na versão 10.0 e, a partir da versão 10.1, começou a avisar quando uma conexão negocia um método que não é pós-quântico.[^openssh-pq]

---

## 1. Antes de falar em computação quântica: o que a criptografia faz?

**Criptografia** (*cryptography*) é o conjunto de técnicas usadas para proteger informações. Em uma conexão SSH, ela ajuda a garantir principalmente:

- **confidencialidade:** terceiros não devem conseguir ler o conteúdo;
- **integridade:** alterações indevidas devem ser detectadas;
- **autenticidade:** cliente e servidor precisam confirmar com quem estão se comunicando.

É importante separar três funções que costumam ser confundidas.

| Função | Pergunta respondida | Exemplo no SSH |
|---|---|---|
| Troca ou estabelecimento de chaves | Como cliente e servidor criam um segredo compartilhado? | X25519, ML-KEM + X25519 |
| Criptografia simétrica | Como os dados da sessão serão protegidos? | AES-GCM ou ChaCha20-Poly1305 |
| Autenticação e assinatura | Como confirmar a identidade do servidor ou do usuário? | Ed25519, ECDSA, RSA |

O aviso apresentado neste artigo trata principalmente da **troca de chaves**, e não do algoritmo usado para criptografar cada pacote da sessão.

### Uma analogia

Imagine que duas pessoas precisam conversar por meio de um túnel:

1. primeiro, elas combinam uma chave secreta;
2. depois, usam essa chave para trancar e destrancar todas as caixas enviadas pelo túnel;
3. também verificam a identidade de quem está do outro lado.

A **troca de chaves** corresponde à primeira etapa. Se um invasor conseguir reconstruir essa chave, poderá tentar abrir as caixas que gravou anteriormente.

---

## 2. O que acontece no início de uma conexão SSH?

De maneira simplificada, uma conexão SSH realiza as seguintes etapas:

1. cliente e servidor informam os algoritmos que suportam;
2. escolhem um algoritmo de troca de chaves compatível;
3. produzem um segredo compartilhado;
4. derivam chaves simétricas a partir desse segredo;
5. autenticam o servidor;
6. protegem comandos, respostas e arquivos transferidos durante a sessão.

A chave usada para criptografar os dados não precisa ser enviada diretamente pela rede. Ela é **derivada** de valores calculados durante o protocolo.

Essa propriedade permite que duas máquinas estabeleçam uma sessão segura mesmo quando a rede entre elas não é confiável.

---

## 3. O que é X25519 e por que ele aparece no alerta?

O X25519 é um mecanismo moderno de acordo de chaves baseado em criptografia de curvas elípticas. No OpenSSH, ele aparece em algoritmos como:

```text
curve25519-sha256
```

Contra computadores clássicos e com os parâmetros adequados, o X25519 continua sendo considerado uma escolha forte. O problema discutido pelo alerta é outro: a segurança da criptografia de curvas elípticas depende da dificuldade do **problema do logaritmo discreto**.

Um computador quântico suficientemente grande e tolerante a falhas poderia aplicar o **algoritmo de Shor** para resolver esse tipo de problema com eficiência muito superior à dos melhores métodos clássicos conhecidos.[^shor]

{: .box-warning}
**Atenção:** dizer que uma sessão usou X25519 não significa que o X25519 foi quebrado hoje. Significa que ele, quando usado sozinho, não é considerado resistente ao modelo de ataque de um futuro computador quântico criptograficamente relevante.

---

## 4. O que é computação quântica?

Um computador clássico trabalha com **bits**, normalmente representados por `0` ou `1`.

Um computador quântico trabalha com **qubits**, que podem ser preparados em estados quânticos envolvendo:

- **superposição**;
- **interferência**;
- **emaranhamento**;
- **medição probabilística**.

Não é correto resumir um computador quântico como uma máquina que “testa todas as respostas ao mesmo tempo”. O ganho surge quando um algoritmo organiza a interferência entre estados para aumentar a probabilidade de obter informações úteis na medição.

Computadores quânticos também não são automaticamente mais rápidos para qualquer tarefa. Eles oferecem vantagens específicas para determinados problemas e algoritmos.

### Algoritmo de Shor

O algoritmo de Shor ameaça problemas matemáticos usados por grande parte da criptografia de chave pública, especialmente:

- fatoração de inteiros, associada ao RSA;
- logaritmo discreto, associado ao Diffie–Hellman;
- logaritmo discreto em curvas elípticas, associado a ECDH, ECDSA e outros mecanismos baseados em curvas.

### Algoritmo de Grover

O algoritmo de Grover oferece uma aceleração quadrática para buscas não estruturadas.[^grover] Seu impacto sobre criptografia simétrica é diferente do impacto de Shor sobre a criptografia assimétrica.

Por isso, algoritmos simétricos não são simplesmente “destruídos” da mesma maneira. Em muitos cenários, o impacto pode ser compensado com tamanhos de chave e parâmetros adequados.

---

## 5. Como funciona o ataque “store now, decrypt later”?

O ataque também é chamado de:

- **harvest now, decrypt later**;
- **capture now, decrypt later**;
- **SNDL** ou **HNDL**.

Ele pode ser entendido em quatro momentos.

### Momento 1 — captura

Um invasor posicionado em algum ponto da rede registra os pacotes de uma sessão SSH.

```text
Cliente  ===== tráfego criptografado =====>  Servidor
                    |
                    +----> cópia armazenada pelo invasor
```

### Momento 2 — armazenamento

O invasor não consegue ler os dados naquele momento, mas mantém a gravação por anos.

### Momento 3 — avanço tecnológico

No futuro, o invasor obtém acesso a um computador quântico criptograficamente relevante ou a algum outro avanço capaz de quebrar o estabelecimento de chaves utilizado.

### Momento 4 — descriptografia retrospectiva

Com os valores públicos registrados durante a negociação, o invasor tenta reconstruir o segredo da sessão e, a partir dele, as chaves usadas para proteger o tráfego.

```text
tráfego gravado + capacidade futura
                 |
                 v
      tentativa de recuperar a chave
                 |
                 v
       leitura retrospectiva da sessão
```

{: .box-note}
Esse ataque é especialmente relevante quando os dados precisam permanecer confidenciais por muitos anos: informações governamentais, dados de pesquisa, propriedade intelectual, informações médicas, documentos estratégicos e credenciais de longa duração.

---

## 6. A proteção de encaminhamento não resolve tudo?

Mecanismos efêmeros como ECDHE/X25519 oferecem **forward secrecy**, também chamada de sigilo futuro ou sigilo de encaminhamento.

Isso ajuda a impedir que o comprometimento posterior da chave permanente do servidor revele automaticamente todas as sessões antigas.

Entretanto, há uma diferença entre:

- roubar no futuro a chave permanente do servidor;
- usar um computador quântico para resolver o problema matemático presente em cada troca efêmera registrada.

Assim, a proteção de encaminhamento clássica continua importante, mas não elimina sozinha o risco de “store now, decrypt later” contra uma troca de chaves vulnerável a Shor.

---

## 7. O que é criptografia pós-quântica?

**Criptografia pós-quântica** (*Post-Quantum Cryptography — PQC*) é a área que desenvolve algoritmos executados em computadores convencionais, mas baseados em problemas matemáticos que, até o momento, não possuem ataques quânticos eficientes conhecidos.

Ela não deve ser confundida com:

- computador quântico;
- comunicação quântica;
- distribuição quântica de chaves, ou QKD.

A PQC é implementada em software e hardware convencionais e pode ser incorporada a protocolos como SSH, TLS, VPNs, aplicações e sistemas operacionais.

Em 2024, o NIST publicou os primeiros padrões principais de criptografia pós-quântica. O **FIPS 203** padronizou o ML-KEM, um mecanismo de encapsulamento de chaves baseado em problemas de reticulados modulares.[^fips203][^nist-pqc]

### O que é um KEM?

Um **Key Encapsulation Mechanism — KEM** permite criar e transportar, de forma protegida, um segredo compartilhado.

De forma simplificada:

```text
KeyGen   -> cria chave pública e chave privada
Encaps   -> produz um segredo compartilhado e um encapsulamento
Decaps   -> recupera o mesmo segredo usando a chave privada
```

O segredo resultante é usado para derivar as chaves simétricas da conexão.

---

## 8. Por que o OpenSSH usa algoritmos híbridos?

Os mecanismos pós-quânticos adotados pelo OpenSSH são **híbridos**: combinam um componente pós-quântico com um componente clássico.

Exemplo:

```text
mlkem768x25519-sha256
```

Esse nome indica a combinação de:

- **ML-KEM-768**, componente pós-quântico;
- **X25519**, componente clássico;
- **SHA-256**, função usada na construção.

O protocolo produz dois segredos e os combina:

```text
segredo pós-quântico + segredo clássico
                    |
                    v
          segredo final da sessão
```

A finalidade é reduzir o risco da transição:

- se o componente clássico permanecer seguro, ele continua contribuindo;
- se o componente pós-quântico permanecer seguro, ele protege contra o risco quântico;
- se uma falha futura for encontrada em um dos componentes, o outro ainda pode preservar a segurança da combinação.

O documento do IETF para o método híbrido com ML-KEM afirma que a segurança de cada componente é independente e que a combinação deve ser, pelo menos, tão segura quanto o componente mais seguro executado na troca.[^ietf-mlkem]

O OpenSSH também utiliza:

```text
sntrup761x25519-sha512
```

Nesse caso, o componente pós-quântico é o Streamlined NTRU Prime, combinado novamente com X25519.[^ietf-ntruprime]

---

## 9. Por que o aviso apareceu agora?

Segundo o OpenSSH:[^openssh-pq]

| Versão | Mudança relevante |
|---|---|
| OpenSSH 9.0 | `sntrup761x25519-sha512` passou a ser oferecido por padrão |
| OpenSSH 9.9 | inclusão de `mlkem768x25519-sha256` |
| OpenSSH 10.0 | `mlkem768x25519-sha256` tornou-se o método padrão |
| OpenSSH 10.1 | aviso quando a conexão negocia troca de chaves não pós-quântica |

O alerta normalmente indica uma destas situações:

1. o servidor usa uma versão antiga do OpenSSH;
2. o servidor usa outra implementação sem suporte aos métodos compatíveis;
3. os algoritmos pós-quânticos foram desativados em `sshd_config`;
4. cliente e servidor não encontraram um método pós-quântico em comum;
5. uma configuração específica forçou um algoritmo clássico.

---

## 10. Como verificar a conexão?

### 10.1 Verificar a versão do cliente

```bash
ssh -V
```

### 10.2 Verificar os algoritmos oferecidos pelo cliente

```bash
ssh -Q kex
```

Para filtrar os métodos pós-quânticos mais comuns no OpenSSH:

```bash
ssh -Q kex | grep -E 'mlkem|sntrup'
```

Uma saída compatível pode conter:

```text
mlkem768x25519-sha256
sntrup761x25519-sha512
```

### 10.3 Verificar o algoritmo realmente negociado

Use o modo verboso:

```bash
ssh -vv usuario@servidor
```

Procure uma linha semelhante a:

```text
debug1: kex: algorithm: mlkem768x25519-sha256
```

Ou filtre a saída:

```bash
ssh -vv usuario@servidor 2>&1 | grep -i 'kex: algorithm'
```

### 10.4 Verificar a configuração efetiva do servidor

No servidor:

```bash
sudo sshd -T | grep -i '^kexalgorithms'
```

Esse comando permite verificar se os métodos pós-quânticos estão presentes na configuração efetiva.

### 10.5 Testar explicitamente um método

Quando o cliente oferece suporte:

```bash
ssh -o KexAlgorithms=mlkem768x25519-sha256 usuario@servidor
```

Para testar o método disponível desde o OpenSSH 9.0:

```bash
ssh -o KexAlgorithms=sntrup761x25519-sha512 usuario@servidor
```

Se o cliente ou o servidor não oferecer o algoritmo, a negociação falhará. Isso ajuda a identificar qual lado precisa ser atualizado ou reconfigurado.

---

## 11. Como corrigir?

A solução preferencial é:

1. atualizar o cliente e o servidor SSH;
2. verificar se a versão instalada oferece os algoritmos híbridos;
3. revisar configurações que substituem a lista padrão de `KexAlgorithms`;
4. testar novamente com `ssh -vv`;
5. planejar a migração com compatibilidade para clientes legados.

{: .box-warning}
Evite copiar uma lista fixa de algoritmos para `KexAlgorithms` sem compreender o efeito. Uma configuração antiga pode substituir os padrões atuais e desativar, sem intenção, algoritmos mais novos e seguros.

Antes de editar:

```bash
sudo sshd -T | grep -i '^kexalgorithms'
```

Depois de editar, valide a configuração:

```bash
sudo sshd -t
```

Somente após o teste, recarregue o serviço conforme o sistema operacional utilizado.

---

## 12. Posso apenas esconder o aviso?

O OpenSSH permite desativar o alerta com `WarnWeakCrypto`, mas isso **não altera o algoritmo negociado** e não adiciona proteção pós-quântica.[^ssh-config]

Quando for necessário manter temporariamente um servidor legado, prefira uma exceção específica:

```sshconfig
Host legado.exemplo
    WarnWeakCrypto no-pq-kex
```

Evite desabilitar o aviso globalmente sem avaliar o risco.

{: .box-error}
**Silenciar o alerta não corrige a ausência de proteção pós-quântica.** Apenas informa ao cliente que o risco foi aceito para aquele destino.

---

## 13. Este aviso significa que houve invasão?

Não.

O aviso não prova que:

- alguém capturou o tráfego;
- a sessão foi descriptografada;
- o servidor foi comprometido;
- a senha foi roubada;
- existe hoje um computador quântico capaz de quebrar a conexão.

Ele informa que a sessão negociou um método de troca de chaves que não oferece a proteção pós-quântica recomendada pelo OpenSSH.

Uma descrição tecnicamente mais precisa seria:

> “A conexão está protegida pela criptografia clássica negociada, mas não está protegida contra o cenário futuro de quebra quântica da troca de chaves.”

---

## 14. Como avaliar a urgência?

A prioridade depende da combinação entre **probabilidade de captura**, **valor dos dados** e **tempo durante o qual os dados continuarão sensíveis**.

| Situação | Exemplo | Prioridade sugerida |
|---|---|---|
| Dados públicos ou rapidamente perecíveis | comandos sem conteúdo sensível em laboratório isolado | menor |
| Dados internos com vida útil de meses | administração rotineira sem segredos duradouros | moderada |
| Dados que precisam permanecer sigilosos por anos | pesquisa, contratos, dados pessoais, propriedade intelectual | alta |
| Infraestrutura crítica ou alvo de atores avançados | governo, defesa, saúde, finanças, identidade | muito alta |

Uma forma simples de pensar é:

```text
risco ≈ valor futuro dos dados
      × tempo de confidencialidade
      × possibilidade de captura
      × capacidade futura do adversário
```

Essa expressão não é uma fórmula quantitativa, mas ajuda a estruturar a análise.

---

## 15. Troca de chaves e assinaturas são o mesmo problema?

Não.

O OpenSSH destaca que muitos algoritmos atuais de assinatura, como RSA e ECDSA, também seriam afetados por computadores quânticos suficientemente poderosos. Entretanto, o risco retrospectivo é diferente.[^openssh-pq]

Na troca de chaves, uma gravação antiga pode ser usada futuramente para tentar recuperar o segredo de uma sessão passada.

Nas assinaturas, não existe exatamente o mesmo ataque de “gravar hoje e descriptografar depois”. A preocupação é substituir as chaves e algoritmos vulneráveis antes que um adversário consiga falsificar novas assinaturas.

Portanto, a migração envolve pelo menos duas trilhas:

- proteger hoje a **confidencialidade futura** das sessões;
- preparar a substituição futura dos mecanismos de **autenticação e assinatura**.

---

## 16. Erros comuns de interpretação

### “Computadores quânticos quebram qualquer criptografia”

Não. O impacto varia conforme o algoritmo, os parâmetros, o problema matemático e o modelo de ataque.

### “Pós-quântico significa usar um computador quântico”

Não. A criptografia pós-quântica é executada em computadores convencionais.

### “X25519 ficou inseguro”

Não no sentido clássico atual. Ele não é, porém, considerado resistente a um futuro ataque quântico baseado em Shor quando usado sozinho.

### “Forward secrecy impede qualquer descriptografia futura”

Ela protege contra determinados comprometimentos de chaves permanentes, mas não impede que um algoritmo quântico resolva o problema matemático de uma troca efêmera registrada.

### “Esconder o aviso resolve”

Não. A opção apenas silencia a mensagem.

### “A criptografia pós-quântica é definitivamente inquebrável”

Não existe garantia absoluta. Os algoritmos são selecionados com base no melhor conhecimento científico disponível e continuam sujeitos a análise, implementação correta, revisão e criptoanálise.


---

## Conclusão

O aviso do OpenSSH não deve ser interpretado como prova de invasão. Ele é um sinal de que a conexão negociou uma troca de chaves protegida pelos modelos clássicos, mas sem a camada pós-quântica recomendada para preservar a confidencialidade de longo prazo.

A principal ideia do ataque é simples:

```text
capturar hoje -> armazenar -> obter capacidade futura -> tentar descriptografar
```

A resposta recomendada também é direta:

```text
inventariar -> atualizar -> habilitar algoritmos híbridos -> testar -> monitorar
```

A transição para a criptografia pós-quântica não precisa esperar a chegada de um computador quântico capaz de quebrar a criptografia atual. Quando os dados precisam permanecer secretos por muitos anos, proteger a comunicação futura começa no presente.

---

## Referências

[^openssh-pq]: OpenSSH. [Post-Quantum Cryptography](https://www.openssh.org/pq.html). Página oficial sobre o alerta, versões e algoritmos pós-quânticos.
[^ssh-config]: OpenBSD. [ssh_config(5) — WarnWeakCrypto](https://man.openbsd.org/ssh_config#WarnWeakCrypto).
[^fips203]: NIST. [FIPS 203 — Module-Lattice-Based Key-Encapsulation Mechanism Standard](https://csrc.nist.gov/pubs/fips/203/final), 2024.
[^nist-pqc]: NIST. [Post-Quantum Cryptography Project](https://csrc.nist.gov/projects/post-quantum-cryptography).
[^shor]: SHOR, Peter W. [Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer](https://arxiv.org/abs/quant-ph/9508027).
[^grover]: GROVER, Lov K. [A Fast Quantum Mechanical Algorithm for Database Search](https://arxiv.org/abs/quant-ph/9605043).
[^ietf-mlkem]: IETF. [PQ/T Hybrid Key Exchange with ML-KEM in SSH](https://datatracker.ietf.org/doc/draft-ietf-sshm-mlkem-hybrid-kex/). Documento em processo de padronização.
[^ietf-ntruprime]: IETF. [SSH Key Exchange Using Hybrid Streamlined NTRU Prime and X25519](https://datatracker.ietf.org/doc/draft-josefsson-ntruprime-ssh/).
