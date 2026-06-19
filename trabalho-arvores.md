## Parte 1 — Tipos de Árvores

### Árvore AVL

#### Conceito

A árvore **AVL** é uma **Árvore Binária de Busca (BST) auto-balanceada**, proposta em 1962 pelos matemáticos soviéticos **A**delson-**V**elsky e **L**andis — daí o nome. Ela mantém, a cada inserção ou remoção, a propriedade de que a diferença de altura entre as subárvores esquerda e direita de **qualquer nó** nunca seja maior que 1. Quando essa propriedade é violada, a árvore se reorganiza automaticamente por meio de **rotações**.

#### Características

- **Fator de balanceamento (FB)** de cada nó: `FB = altura(subárvore esquerda) − altura(subárvore direita)`.
- O fator de balanceamento de todo nó deve sempre pertencer ao conjunto **{ −1, 0, +1 }**.
- Continua respeitando a regra fundamental da BST: à esquerda valores menores, à direita valores maiores.
- Após cada inserção/remoção, o caminho até a raiz é verificado e, se necessário, são aplicadas rotações (simples ou duplas).
- É uma árvore **rigorosamente balanceada**: sua altura máxima é de aproximadamente `1,44 · log₂(n)`.

#### Vantagens

- Garante busca, inserção e remoção em **O(log n)** mesmo no pior caso.
- Por ser fortemente balanceada, é **mais rápida em buscas** do que a Rubro-Negra.
- Excelente para aplicações com **muitas leituras** e poucas modificações.

#### Desvantagens

- Inserções e remoções podem exigir **mais rotações** para manter o balanceamento rígido, tornando-as mais custosas.
- Cada nó precisa armazenar informações adicionais (altura ou fator de balanceamento), gastando mais memória.
- Implementação mais trabalhosa que a BST simples.

#### Exemplo ilustrado

Inserindo os valores **30, 20, 10** nessa ordem, o nó `30` fica com FB = +2 (desbalanceado à esquerda). Aplica-se uma **rotação simples à direita**:

```
   Antes (desbalanceada)        Depois (balanceada)

        30 (FB = +2)                 20 (FB = 0)
       /                            /  \
     20 (FB = +1)        →         10   30
     /
   10 (FB = 0)
```

---

### Árvore Rubro-Negra

#### Conceito

A árvore **Rubro-Negra** (*Red-Black Tree*) é outra **BST auto-balanceada**, em que cada nó recebe uma **cor** (vermelho ou preto). Em vez de manter um balanceamento perfeito como a AVL, ela garante um **balanceamento aproximado**: o caminho mais longo da raiz até uma folha nunca é mais que o dobro do caminho mais curto. Isso é suficiente para assegurar complexidade **O(log n)** com **menos rotações** durante as alterações.

#### Regras de coloração

1. Todo nó é **vermelho** ou **preto**.
2. A **raiz** é sempre **preta**.
3. Toda **folha nula (NIL)** é considerada **preta**.
4. Um nó **vermelho não pode ter filho vermelho** (não existem dois vermelhos seguidos).
5. Todo caminho de um nó até qualquer folha NIL descendente contém o **mesmo número de nós pretos** (a chamada *altura-preta*).

#### Vantagens

- Realiza **menos rotações** que a AVL nas operações de inserção e remoção (no máximo 2 a 3 rotações por operação).
- É mais eficiente em cenários com **muitas inserções e remoções**.
- Amplamente utilizada em bibliotecas e estruturas internas de sistemas reais (`std::map`/`std::set` do C++, `TreeMap`/`TreeSet` do Java, escalonador do kernel Linux).

#### Desvantagens

- O balanceamento é **menos rígido** que o da AVL, então a árvore pode ficar um pouco mais alta (até `2 · log₂(n)`), deixando a busca levemente mais lenta.
- A lógica de coloração e recoloração torna a **implementação mais complexa**.

#### Exemplo ilustrado

`(P)` = preto, `(V)` = vermelho. Repare que a raiz é preta, não há vermelho com filho vermelho e todos os caminhos têm a mesma quantidade de nós pretos:

```
              13 (P)
            /        \
        8 (V)         17 (V)
       /    \         /     \
   1 (P)  11 (P)   15 (P)   25 (P)
```

---

### Árvore N-ária

#### Conceito

A árvore **N-ária** (ou árvore genérica) é uma estrutura em que **cada nó pode ter mais de dois filhos** — até **N** filhos. É uma generalização da árvore binária e é a forma natural de representar **hierarquias** em que cada elemento pode conter vários subelementos.

#### Diferenças em relação às árvores binárias

| Aspecto | Árvore Binária | Árvore N-ária |
|---|---|---|
| Filhos por nó | No máximo **2** | Até **N** filhos |
| Estrutura do nó | 2 ponteiros (esq./dir.) | Lista/vetor de ponteiros |
| Objetivo típico | Busca ordenada | Representar hierarquias amplas |
| Altura | Cresce mais rápido | Mais "larga" e mais baixa |

A árvore binária é, na prática, um **caso particular** da N-ária com N = 2.

#### Vantagens

- Representa de forma **fiel hierarquias com muitos níveis e muitos filhos** (pastas, menus, organogramas).
- Variações balanceadas (como as **Árvores B / B+**) reduzem a **altura** da árvore e, com isso, o número de acessos a disco — essenciais em bancos de dados e sistemas de arquivos.
- Maior fan-out (muitos filhos por nó) significa **menos níveis** para percorrer.

#### Desvantagens

- A estrutura do nó é **mais complexa** (precisa armazenar vários ponteiros/filhos).
- O percurso e algumas operações são **mais elaborados** que na árvore binária.
- Não é automaticamente balanceada, a menos que seja uma variação específica (B-tree, B+-tree).

#### Exemplo ilustrado

Hierarquia de um sistema de arquivos representada como árvore N-ária:

```
                 / (raiz)
        ┌──────────┼──────────┐
      home        etc         usr
        │                      │
      usuario                 bin
    ┌────┼────┐
  docs  imgs  musicas
```

#### Aplicações práticas

- **Sistemas de arquivos** (pastas e subpastas).
- **Bancos de dados** (índices com Árvores B e B+).
- **Tries** (autocompletar, dicionários, busca de strings).
- **DOM / XML / JSON** (estrutura de páginas e documentos).
- **Menus, taxonomias e categorias** de e-commerce.
- **Árvores de decisão** em IA e jogos.
