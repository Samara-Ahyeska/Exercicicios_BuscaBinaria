# Algoritmos de Busca: Análise Comparativa e Pesquisa Binária Avançada

Este repositório contém uma análise detalhada, teórica e conceitual sobre os fundamentos da **Pesquisa Binária (Binary Search)**, abordando desde os cuidados necessários na implementação da busca clássica até o uso de variantes avançadas para o tratamento de elementos duplicados.

---

## 📋 Visão Geral

A pesquisa binária é um algoritmo do tipo *divisão e conquista* projetado para buscar elementos em estruturas de dados devidamente **ordenadas**. A cada iteração ou chamada recursiva, o algoritmo divide o espaço de busca pela metade, resultando em uma complexidade de tempo logarítmica $\mathcal{O}(\log n)$.

Apesar da simplicidade conceitual, a implementação da pesquisa binária exige atenção a detalhes críticos de borda para evitar estouro de memória, estouro de inteiros ou *loops* infinitos.

---

## 🔍 Exercício 1: Busca Clássica e Armadilhas de Implementação

### O Problema
Dada uma coleção de elementos ordenados em ordem crescente e um valor alvo (*target*), o objetivo é verificar se o valor está presente na estrutura e retornar o seu índice (ou um código de erro/retorno negativo como `-1` caso o valor não exista).

### Armadilhas Críticas de Implementação e Soluções

1. **Estouro de Inteiro (*Integer Overflow*)**
   * **Problema:** A fórmula intuitiva para calcular o ponto médio, $(baixo + alto) / 2$, pode falhar em linguagens estaticamente tipadas caso a soma dos índices ultrapasse o limite máximo representável do tipo inteiro (ex: `INT_MAX`).
   * **Solução:** Empregar a forma algébrica equivalente que calcula a distância entre os limites antes de somar: 
     $$\text{meio} = \text{baixo} + \frac{\text{alto} - \text{baixo}}{2}$$

2. **Condição de Parada Incorreta no Laço**
   * **Problema:** Utilizar estritamente $\text{baixo} < \text{alto}$ faz com que o laço encerre precocemente quando restar apenas um único elemento no subintervalo (momento em que $\text{baixo} == \text{alto}$), ignorando a checagem desse elemento.
   * **Solução:** Definir a condição do laço como $\text{baixo} \le \text{alto}$, garantindo a avaliação do intervalo até a sua completa exaustão.

3. **Loops Infinitos por Atualização de Ponteiros Inadequada**
   * **Problema:** Atualizar o limite inferior ou superior atribuindo diretamente o índice central ($\text{baixo} = \text{meio}$ ou $\text{alto} = \text{meio}$) impede o encolhimento do intervalo quando a distância entre os ponteiros é de apenas uma unidade, gerando um ciclo infinito.
   * **Solução:** Descartar explicitamente o elemento central já testado atualizando os limites com deslocamento:
     $$\text{baixo} = \text{meio} + 1 \quad \text{ou} \quad \text{alto} = \text{meio} - 1$$

### Estratégias de Abordagem

* **Abordagem Iterativa:** Utiliza um laço de repetição contínuo. Mantém o consumo de memória principal constante com complexidade de espaço $\mathcal{O}(1)$.
* **Abordagem Recursiva:** Requer uma função auxiliar que passa os limites atualizados a cada nova chamada. Consome memória proporcional à profundidade da árvore de recursão na pilha de chamadas (*call stack*), resultando em complexidade de espaço $\mathcal{O}(\log n)$.

---

## 🚀 Exercício 2: Variante Avançada — Lower Bound e Upper Bound

### O Problema
Quando o conjunto de dados possui **elementos duplicados**, a busca binária clássica retorna o índice da *primeira ocorrência que ela encontra aleatoriamente*, sem garantia de ser o início ou o fim da sequência repetida. 

Para delimitar com precisão todas as ocorrências de um determinado valor, utilizam-se duas variantes conhecidas como **Lower Bound** (Limite Inferior) e **Upper Bound** (Limite Superior).

### Conceitos e Estratégias de Resolução

1. **Lower Bound (Limite Inferior)**
   * **Conceito:** Localiza o primeiro índice onde o elemento é **maior ou igual ($\ge$)** ao valor procurado. Se o elemento estiver no array, o Lower Bound aponta para a sua **primeira ocorrência**.
   * **Estratégia:** Quando o valor na posição central atende à condição ($\text{arr}[\text{meio}] \ge \text{alvo}$), guarda-se esse índice como um candidato válido e encolhe-se o limite superior para o meio ($\text{alto} = \text{meio}$). Isso força o algoritmo a continuar investigando a metade esquerda em busca de ocorrências anteriores.

2. **Upper Bound (Limite Superior)**
   * **Conceito:** Localiza o primeiro índice onde o elemento é **estritamente maior ($>$)** que o valor procurado. Ele aponta para a primeira posição imediatamente **após** a última ocorrência do elemento alvo.
   * **Estratégia:** Quando a condição é satisfeita ($\text{arr}[\text{meio}] > \text{alvo}$), o índice central é registrado e a busca é restringida à esquerda ($\text{alto} = \text{meio}$). Caso contrário, avança-se para a direita ($\text{baixo} = \text{meio} + 1$).

### Mapeamento do Intervalo de Ocorrências

Combinando ambas as variantes, obtém-se o intervalo fechado $[\text{início}, \text{fim}]$ de um elemento duplicado através das seguintes posições:

$$\text{Início} = \text{Lower Bound}(\text{alvo})$$
$$\text{Fim} = \text{Upper Bound}(\text{alvo}) - 1$$

Se $\text{Início} \le \text{Fim}$ e o elemento na posição $\text{Início}$ for igual ao alvo, o intervalo é válido; caso contrário, conclui-se que o elemento não está presente no conjunto.

---

## 📊 Tabela Comparativa dos Algoritmos

| Algoritmo | Condição de Manutenção | Comportamento com Duplicatas | Complexidade de Tempo | Complexidade de Espaço | Finalidade Principal |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Busca Binária Iterativa** | $\text{baixo} \le \text{alto}$ | Retorna uma ocorrência arbitrária | $\mathcal{O}(\log n)$ | $\mathcal{O}(1)$ | Checagem de existência simples e busca pontual. |
| **Busca Binária Recursiva** | $\text{baixo} \le \text{alto}$ | Retorna uma ocorrência arbitrária | $\mathcal{O}(\log n)$ | $\mathcal{O}(\log n)$ | Abordagem funcional estruturada em pilha. |
| **Lower Bound** | $\text{baixo} < \text{alto}$ | Localiza a **primeira** ocorrência | $\mathcal{O}(\log n)$ | $\mathcal{O}(1)$ | Encontrar o início de sequências e ponto de inserção ordenado. |
| **Upper Bound** | $\text{baixo} < \text{alto}$ | Localiza a posição **após a última** ocorrência | $\mathcal{O}(\log n)$ | $\mathcal{O}(1)$ | Definir limite superior para contagem e mapeamento de intervalos. |
