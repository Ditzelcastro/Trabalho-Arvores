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

## Parte 2 — Operações em Árvores

As rotações são operações **locais** que reorganizam alguns nós para restaurar o balanceamento **sem violar** a ordenação da BST.

### Rotação Simples à Direita

- **Objetivo:** corrigir um desbalanceamento causado por excesso de altura na **subárvore esquerda**.
- **Situação em que é utilizada:** caso **Esquerda-Esquerda (LL)** — o nó está desbalanceado à esquerda e seu filho esquerdo também pende para a esquerda (FB do nó = +2 e FB do filho ≥ 0).
- **Exemplo antes e depois:**

```
        Antes (FB = +2)            Depois (FB = 0)

             30                         20
            /                          /  \
          20             →           10    30
         /
        10
```

### Rotação Simples à Esquerda

- **Objetivo:** corrigir um desbalanceamento causado por excesso de altura na **subárvore direita**.
- **Situação em que é utilizada:** caso **Direita-Direita (RR)** — o nó está desbalanceado à direita e seu filho direito também pende para a direita (FB do nó = −2 e FB do filho ≤ 0).
- **Exemplo antes e depois:**

```
     Antes (FB = −2)            Depois (FB = 0)

       10                            20
         \                          /  \
          20           →          10    30
            \
             30
```

### Rotação Dupla

Usada quando o desbalanceamento está "em zigue-zague". Combina duas rotações simples.

#### Esquerda-Direita (LR)

Ocorre quando o nó pende para a **esquerda**, mas o filho esquerdo pende para a **direita**. Solução: **rotação à esquerda no filho** + **rotação à direita no nó**.

```
   Inicial         1ª rotação (esq. no 10)    2ª rotação (dir. no 30)

     30                  30                          20
    /                   /                           /  \
  10        →         20            →             10    30
    \                /
     20            10
```

#### Direita-Esquerda (RL)

Ocorre quando o nó pende para a **direita**, mas o filho direito pende para a **esquerda**. Solução: **rotação à direita no filho** + **rotação à esquerda no nó**.

```
   Inicial         1ª rotação (dir. no 30)    2ª rotação (esq. no 10)

   10                  10                          20
     \                   \                        /  \
      30      →           20          →         10    30
     /                      \
    20                       30
```

### Inversão (Espelhamento)

- **Conceito:** operação que **troca recursivamente** a subárvore esquerda pela direita de **todos** os nós, produzindo a imagem espelhada da árvore. Diferente das rotações, **não tem o objetivo de balancear** — ela inverte a ordem.
- **Aplicação:** exercícios e entrevistas de algoritmos (o famoso "*invert a binary tree*"), processamento de imagens hierárquicas, geração de estruturas simétricas e testes de manipulação de árvores.
- **Exemplo antes e depois:**

```
        Antes                       Depois (espelhada)

          1                              1
        /   \                          /   \
       2     3          →             3     2
      / \                                  / \
     4   5                                5   4
```

Em pseudocódigo:

```
inverter(no):
    se no == nulo: retorne
    troca(no.esquerda, no.direita)
    inverter(no.esquerda)
    inverter(no.direita)```

## Parte 3 - Aplicação Prática

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
#define MAX_NOME 64
#define MAX_FILHOS 32
 
typedef enum { PASTA, ARQUIVO } TipoNo;
 
typedef struct No {
    char nome[MAX_NOME];
    TipoNo tipo;
    struct No *filhos[MAX_FILHOS];
    int totalFilhos;
} No;
 
/* Cria um novo no (pasta ou arquivo) */
No *criarNo(const char *nome, TipoNo tipo) {
    No *novo = (No *) malloc(sizeof(No));
    if (novo == NULL) {
        fprintf(stderr, "Erro: falha ao alocar memoria para '%s'\n", nome);
        exit(EXIT_FAILURE);
    }
    strncpy(novo->nome, nome, MAX_NOME - 1);
    novo->nome[MAX_NOME - 1] = '\0';
    novo->tipo = tipo;
    novo->totalFilhos = 0;
    return novo;
}
 
/* Insere um filho (arquivo ou subpasta) dentro de uma pasta */
int inserirFilho(No *pai, No *filho) {
    if (pai->tipo != PASTA) {
        fprintf(stderr, "Erro: '%s' nao e uma pasta, nao pode ter filhos.\n", pai->nome);
        return 0;
    }
    if (pai->totalFilhos >= MAX_FILHOS) {
        fprintf(stderr, "Erro: '%s' atingiu o limite de filhos.\n", pai->nome);
        return 0;
    }
    pai->filhos[pai->totalFilhos++] = filho;
    return 1;
}
 
/* Percorre a arvore em pre-ordem, exibindo a hierarquia com indentacao */
void listarArvore(No *no, int nivel) {
    if (no == NULL) return;
 
    for (int i = 0; i < nivel; i++) {
        printf("  ");
    }
 
    if (no->tipo == PASTA) {
        printf("[PASTA] %s/\n", no->nome);
    } else {
        printf("[ARQUIVO] %s\n", no->nome);
    }
 
    for (int i = 0; i < no->totalFilhos; i++) {
        listarArvore(no->filhos[i], nivel + 1);
    }
}
 
/* Busca recursiva por nome em toda a arvore (retorna o primeiro no encontrado) */
No *buscarPorNome(No *no, const char *nome) {
    if (no == NULL) return NULL;
 
    if (strcmp(no->nome, nome) == 0) {
        return no;
    }
 
    for (int i = 0; i < no->totalFilhos; i++) {
        No *encontrado = buscarPorNome(no->filhos[i], nome);
        if (encontrado != NULL) {
            return encontrado;
        }
    }
 
    return NULL;
}
 
/* Libera recursivamente toda a memoria de uma subarvore */
void liberarArvore(No *no) {
    if (no == NULL) return;
 
    for (int i = 0; i < no->totalFilhos; i++) {
        liberarArvore(no->filhos[i]);
    }
 
    free(no);
}
 
/* Remove (e libera) um filho de uma pasta, pelo nome */
int removerFilho(No *pai, const char *nome) {
    if (pai == NULL || pai->tipo != PASTA) return 0;
 
    for (int i = 0; i < pai->totalFilhos; i++) {
        if (strcmp(pai->filhos[i]->nome, nome) == 0) {
            liberarArvore(pai->filhos[i]);
 
            /* reorganiza o vetor de filhos, preenchendo o espaco removido */
            for (int j = i; j < pai->totalFilhos - 1; j++) {
                pai->filhos[j] = pai->filhos[j + 1];
            }
            pai->totalFilhos--;
            return 1;
        }
    }
 
    return 0;
}
 
/* Conta recursivamente quantos arquivos (nao pastas) existem na arvore */
int contarArquivos(No *no) {
    if (no == NULL) return 0;
 
    int total = (no->tipo == ARQUIVO) ? 1 : 0;
 
    for (int i = 0; i < no->totalFilhos; i++) {
        total += contarArquivos(no->filhos[i]);
    }
 
    return total;
}
 
int main(void) {
    /* Monta a hierarquia:
     *
     * raiz/
     *   home/
     *     usuario/
     *       documentos/
     *         relatorio.pdf
     *         apresentacao.pptx
     *       imagens/
     *         foto1.png
     *   etc/
     *     config.txt
     */
 
    No *raiz = criarNo("raiz", PASTA);
 
    No *home = criarNo("home", PASTA);
    No *usuario = criarNo("usuario", PASTA);
    No *documentos = criarNo("documentos", PASTA);
    No *imagens = criarNo("imagens", PASTA);
    No *etc = criarNo("etc", PASTA);
 
    No *relatorio = criarNo("relatorio.pdf", ARQUIVO);
    No *apresentacao = criarNo("apresentacao.pptx", ARQUIVO);
    No *foto1 = criarNo("foto1.png", ARQUIVO);
    No *config = criarNo("config.txt", ARQUIVO);
 
    inserirFilho(raiz, home);
    inserirFilho(raiz, etc);
 
    inserirFilho(home, usuario);
 
    inserirFilho(usuario, documentos);
    inserirFilho(usuario, imagens);
 
    inserirFilho(documentos, relatorio);
    inserirFilho(documentos, apresentacao);
 
    inserirFilho(imagens, foto1);
 
    inserirFilho(etc, config);
 
    printf("===== Estrutura inicial do sistema de arquivos =====\n");
    listarArvore(raiz, 0);
 
    printf("\nTotal de arquivos na arvore: %d\n", contarArquivos(raiz));
 
    printf("\n===== Buscando 'foto1.png' =====\n");
    No *encontrado = buscarPorNome(raiz, "foto1.png");
    if (encontrado != NULL) {
        printf("Encontrado: %s (tipo: %s)\n", encontrado->nome,
               encontrado->tipo == ARQUIVO ? "ARQUIVO" : "PASTA");
    } else {
        printf("Nao encontrado.\n");
    }
 
    printf("\n===== Removendo 'apresentacao.pptx' de 'documentos' =====\n");
    if (removerFilho(documentos, "apresentacao.pptx")) {
        printf("Removido com sucesso.\n");
    } else {
        printf("Arquivo nao encontrado para remocao.\n");
    }
 
    printf("\n===== Estrutura apos remocao =====\n");
    listarArvore(raiz, 0);
 
    printf("\nTotal de arquivos na arvore: %d\n", contarArquivos(raiz));
 
    return 0;
}


 ## Parte 4 — Comparação entre Estruturas

| Estrutura | Nº Máx. de Filhos | Balanceamento | Complexidade de Busca | Complexidade de Inserção | Vantagem Principal | Desvantagem Principal | Exemplo de Aplicação |
|---|---|---|---|---|---|---|---|
| *BST* | 2 | Não possui balanceamento automático. Pode ficar desbalanceada conforme a ordem de inserção. | O(log n) no melhor caso e O(n) no pior caso | O(log n) no melhor caso e O(n) no pior caso | Estrutura simples de entender e implementar | Pode perder desempenho se ficar desbalanceada | Árvores de busca básicas |
| *AVL* | 2 | Sim. Mantém o fator de balanceamento com rotações | O(log n) | O(log n) | Busca previsível e eficiente | Inserção e remoção mais custosas por causa das rotações | Índices e estruturas que exigem leitura frequente |
| *Rubro-Negra* | 2 | Sim. Usa coloração e rotações para manter o balanceamento aproximado | O(log n) | O(log n) | Balanceamento eficiente com menos rotações que a AVL | Implementação mais complexa | Bibliotecas e estruturas internas de sistemas |
| *N-ária* | N (vários filhos) | Pode ou não possuir mecanismos de balanceamento, dependendo da variação | O(log n) em estruturas balanceadas, mas depende da aplicação | O(log n) ou proporcional à estrutura da árvore | Representa melhor hierarquias com muitos filhos | Não é tão simples quanto a árvore binária em alguns contextos | Sistema de arquivos, menus e taxonomias |

### Explicação das informações apresentadas

- *Nº Máximo de Filhos:* BST, AVL e Rubro-Negra são *binárias* (no máximo 2 filhos por nó). A N-ária permite *N filhos*, o que a torna ideal para hierarquias amplas.
- *Balanceamento:* a BST *não* se balanceia sozinha e pode degenerar. AVL e Rubro-Negra *se balanceiam automaticamente* — a AVL de forma rígida (rotações) e a Rubro-Negra de forma aproximada (cores + rotações). A N-ária só se balanceia em variações específicas, como as Árvores B/B+.
- *Complexidade de Busca:* as estruturas balanceadas garantem *O(log n); a BST só atinge isso no melhor caso, caindo para **O(n)* quando degenera em "lista".
- *Complexidade de Inserção:* segue a mesma lógica da busca — O(log n) nas balanceadas e O(n) no pior caso da BST.
- *Vantagem Principal:* cada estrutura otimiza algo diferente — a BST a simplicidade, a AVL a velocidade de busca, a Rubro-Negra o custo de escrita, e a N-ária a representação de hierarquias.
- *Desvantagem Principal:* geralmente um trade-off entre *simplicidade* e *garantia de desempenho* — quanto mais garantias, mais complexa a implementação.
- *Exemplo de Aplicação:* mostra onde cada estrutura é usada na prática, da teoria (BST) até sistemas reais (índices, bibliotecas, sistemas de arquivos).

#### Síntese

> A BST é a base conceitual; a AVL e a Rubro-Negra resolvem o problema do *desbalanceamento* em memória (a AVL favorece leitura, a Rubro-Negra favorece escrita); e as estruturas N-árias balanceadas resolvem o problema do *armazenamento em disco* e da *representação de hierarquias*. A escolha depende sempre do perfil de operações (mais leitura ou mais escrita), do volume de dados e de onde eles ficam armazenados.

---
    liberarArvore(raiz);
