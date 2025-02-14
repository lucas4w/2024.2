# Tutorial: Criando um Servidor TCP Simples em Python

## Informações gerais
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- texto gerado pelo [Microsoft Copilot](https://copilot.microsoft.com/)

## Sumário
1. Servidor HTTP sem thread
2. Experimento 1 - usando thread no servidor
2. Experimento 2 - usando containeres

### 1. Servidor HTTP sem thread


### 2. Experimento 1
1. executar o servidor http, código abaixo **sem thread**, subseção 2.1.
2. executar cliente, código abaixo, subseção 2.3.
   1. apenas 1 cliente
   2. 2 clientes simultâneos
   3. 5 clientes simultâneos
   4. 10 cliente simultâneos
3. analizar e explicar o comportamento do cliente e do **servidor sem thread** para cada um dos 4 casos acima
4. parar servidor sem thread e executar o servidor http, código abaixo **com thread**, subseção 2.2.
5. executar cliente, código abaixo, subseção 2.3.
   1. apenas 1 cliente
   2. 2 clientes simultâneos
   3. 5 clientes simultâneos
   4. 10 cliente simultâneos
6. analizar e explicar o comportamento do cliente e do **servidor com thread** para cada um dos 4 casos acima
7. se tiver diferença no funcionamento dos servidores **sem** e **com** threads, analisar a diferença


# Servidor Sem Threads

## Caso 1: Apenas 1 Cliente

**Comportamento do Cliente:** O cliente consegue se conectar ao servidor e receber a resposta HTML sem problemas. A latência é mínima, pois não há concorrência.

**Comportamento do Servidor:** O servidor processa a requisição de forma sequencial. Como há apenas um cliente, o servidor responde rapidamente e fica ocioso até a próxima requisição.

## Caso 2: 2 Clientes Simultâneos

**Comportamento do Cliente:** O primeiro cliente recebe a resposta rapidamente, mas o segundo cliente pode experimentar um atraso, pois o servidor processa as requisições uma de cada vez.

**Comportamento do Servidor:** O servidor processa a primeira requisição e só então começa a processar a segunda. Isso pode causar um atraso perceptível para o segundo cliente.

## Caso 3: 5 Clientes Simultâneos

**Comportamento do Cliente:** Os primeiros clientes podem receber respostas rapidamente, mas os últimos experimentarão atrasos significativos, pois o servidor processa cada requisição sequencialmente.

**Comportamento do Servidor:** O servidor fica sobrecarregado, processando uma requisição por vez. Isso pode levar a tempos de resposta longos para os clientes que chegam depois.

## Caso 4: 10 Clientes Simultâneos

**Comportamento do Cliente:** A maioria dos clientes experimentará atrasos consideráveis, especialmente os que chegam por último. Alguns clientes podem até atingir o timeout.

**Comportamento do Servidor:** O servidor fica extremamente sobrecarregado, processando uma requisição por vez. Isso pode levar a tempos de resposta muito longos e possíveis falhas para alguns clientes.

---

# Servidor Com Threads

## Caso 1: Apenas 1 Cliente

**Comportamento do Cliente:** O cliente recebe a resposta rapidamente, sem atrasos perceptíveis.

**Comportamento do Servidor:** O servidor cria uma nova thread para lidar com a requisição, permitindo que o cliente seja atendido imediatamente.

## Caso 2: 2 Clientes Simultâneos

**Comportamento do Cliente:** Ambos os clientes recebem respostas rapidamente, sem atrasos perceptíveis.

**Comportamento do Servidor:** O servidor cria uma nova thread para cada requisição, permitindo que ambas sejam processadas simultaneamente.

## Caso 3: 5 Clientes Simultâneos

**Comportamento do Cliente:** Todos os clientes recebem respostas rapidamente, sem atrasos perceptíveis.

**Comportamento do Servidor:** O servidor cria uma nova thread para cada requisição, permitindo que todas sejam processadas simultaneamente.

## Caso 4: 10 Clientes Simultâneos

**Comportamento do Cliente:** Todos os clientes recebem respostas rapidamente, sem atrasos perceptíveis.

**Comportamento do Servidor:** O servidor cria uma nova thread para cada requisição, permitindo que todas sejam processadas simultaneamente. O servidor pode lidar com a carga sem problemas.

---

# Diferenças Entre Servidores Sem e Com Threads

- **Desempenho:** O servidor com threads apresenta um desempenho significativamente melhor em cenários com múltiplos clientes simultâneos. Ele consegue processar várias requisições ao mesmo tempo, enquanto o servidor sem threads processa uma requisição por vez, causando atrasos.

- **Escalabilidade:** O servidor com threads é mais escalável, podendo lidar com um maior número de clientes simultâneos sem degradação significativa do desempenho. O servidor sem threads não escala bem, pois o tempo de resposta aumenta proporcionalmente ao número de clientes.

- **Complexidade:** O servidor com threads é mais complexo de implementar e gerenciar, devido à necessidade de lidar com concorrência e possíveis problemas de sincronização. O servidor sem threads é mais simples, mas menos eficiente em cenários de alta carga.

---

# Conclusão

O uso de threads no servidor HTTP melhora significativamente o desempenho e a escalabilidade, especialmente em cenários com múltiplos clientes simultâneos. Enquanto o servidor sem threads pode ser suficiente para cargas muito leves, ele não é adequado para cenários de alta concorrência. Portanto, a escolha entre usar ou não threads deve ser baseada na carga esperada e nos requisitos de desempenho do sistema.

