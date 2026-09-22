# Lista Dinâmica Encadeada - Gerenciador de Playlist

Este projeto implementa uma **Lista Encadeada Simples** construída manualmente em linguagem C para o gerenciamento de uma playlist de músicas. O projeto não faz uso de bibliotecas de estruturas de dados prontas, manipulando nós e ponteiros dinamicamente através de alocação de memória (`malloc` e `free`).

## Estrutura de Dados

A estrutura é composta por dois blocos fundamentais:

* **`No`**: Contém a estrutura da `Musica` (ID, Título, Artista, Álbum e Duração em segundos) e o ponteiro `proximo` para o nó seguinte.
* **`Lista`**: Mantém a referência para o nó `primeiro` e o contador `quantidade`.

```text
[Lista] -> [No 1 (Musica 1)] -> [No 2 (Musica 2)] -> [No 3 (Musica 3)] -> NULL
```

---

## Instruções de Compilação e Execução

### Pré-requisitos
* Compilador C (como `gcc` ou `clang`)

### Compilação
No terminal, execute:

```bash
gcc -std=c99 main.c -o playlist
```

### Execução
* **Linux / macOS:**
  ```bash
  ./playlist
  ```
* **Windows (PowerShell / CMD):**
  ```bash
  .\playlist.exe
  ```

---

## Exemplos de Execução

### 1. Inserir Músicas na Playlist
O programa oferece três formas de inserção: no início da lista ($O(1)$), no final ($O(n)$) ou em uma posição específica determinada pelo usuário ($O(n)$).

### 2. Impressão da Playlist
Ao selecionar a opção de exibição, a lista encadeada é percorrida a partir do ponteiro `primeiro` até encontrar a referência `NULL`:

```text
======================= PLAYLIST (2 MÚSICAS) =======================
1. ID: 1 | Título: Bohemian Rhapsody | Artista: Queen | Álbum: A Night at the Opera | Duração: 05:54
2. ID: 2 | Título: Hotel California | Artista: Eagles | Álbum: Hotel California | Duração: 06:30
====================================================================
```

---

## Bateria de Testes Realizada

A aplicação possui uma suíte de testes automatizados integrada (Opção 10 do Menu), que cobre rigorosamente todos os cenários de borda exigidos pela especificação:

| Caso de Teste | Operação Avaliada | Resultado Esperado |
| :--- | :--- | :--- |
| **1. Lista Vazia** | `inicializar_lista` | `primeiro == NULL`, `quantidade == 0` |
| **2. Primeira Inserção** | `inserir` em playlist vazia | Nó se torna o `primeiro` da lista |
| **3. Inserções variadas** | `inserir_inicio`, `inserir`, `inserir_posicao` | Nós encadeados nas ordens exatas |
| **4. Buscas** | `buscar` por ID e `buscar_por_artista` | Retorna o nó/dados corretos ou `NULL` quando inexistente |
| **5. Remoção do Início** | `remover` no primeiro elemento | Ponteiro `primeiro` aponta para o $2º$ nó; $1º$ nó liberado |
| **6. Remoção do Meio** | `remover` no elemento intermediário | Nó anterior passa a apontar para o posterior; nó liberado |
| **7. Remoção do Fim** | `remover` no último elemento | Nó penúltimo passa a apontar para `NULL` |
| **8. Remoção Única** | `remover` quando `quantidade == 1` | Lista volta ao estado inicial vazia (`primeiro == NULL`) |
| **9. Remoção Inexistente**| `remover` ID que não existe na lista | Operação rejeitada com retorno `0` |
| **10. Métricas** | `duracao_total` e `quantidade` | Contagem e soma de tempos corretas |
