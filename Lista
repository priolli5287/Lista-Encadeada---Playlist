#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int id;
    char titulo[100];
    char artista[100];
    char album[100];
    int duracao; // em segundos
} Musica;

typedef struct No {
    Musica musica;
    struct No *proximo;
} No;

typedef struct {
    No *primeiro;
    int quantidade;
} Lista;

// OPERAÇÕES OBRIGATÓRIAS DA ESTRUTURA DE DADOS

Lista* inicializar_lista() {
    Lista *lista = (Lista*) malloc(sizeof(Lista));
    if (lista != NULL) {
        lista->primeiro = NULL;
        lista->quantidade = 0;
    }
    return lista;
}

// Inserção genérica (adiciona ao final da lista)
int inserir(Lista *lista, Musica m) {
    No *novo = (No*) malloc(sizeof(No));
    if (novo == NULL) return 0; // Falha na alocacao

    novo->musica = m;
    novo->proximo = NULL;

    if (lista->primeiro == NULL) {
        lista->primeiro = novo;
    } else {
        No *atual = lista->primeiro;
        while (atual->proximo != NULL) {
            atual = atual->proximo;
        }
        atual->proximo = novo;
    }
    lista->quantidade++;
    return 1;
}

void imprimir(Lista *lista) {
    if (lista == NULL || lista->primeiro == NULL) {
        printf("\n[Playlist Vazia]\n");
        return;
    }

    printf("\n======================= PLAYLIST (%d MÚSICAS) =======================\n", lista->quantidade);
    No *atual = lista->primeiro;
    int pos = 1;
    while (atual != NULL) {
        int min = atual->musica.duracao / 60;
        int seg = atual->musica.duracao % 60;
        printf("%d. ID: %d | Título: %s | Artista: %s | Álbum: %s | Duração: %02d:%02d\n",
               pos++, atual->musica.id, atual->musica.titulo,
               atual->musica.artista, atual->musica.album, min, seg);
        atual = atual->proximo;
    }
    printf("====================================================================\n");
}

No* buscar(Lista *lista, int id) {
    if (lista == NULL) return NULL;
    No *atual = lista->primeiro;
    while (atual != NULL) {
        if (atual->musica.id == id) {
            return atual; // Retorna o ponteiro para o nó encontrado
        }
        atual = atual->proximo;
    }
    return NULL; // Não encontrado
}

int remover(Lista *lista, int id) {
    if (lista == NULL || lista->primeiro == NULL) return 0;

    No *atual = lista->primeiro;
    No *anterior = NULL;

    while (atual != NULL && atual->musica.id != id) {
        anterior = atual;
        atual = atual->proximo;
    }

    if (atual == NULL) return 0; // Música não encontrada

    if (anterior == NULL) {
        // Remoção da primeira música
        lista->primeiro = atual->proximo;
    } else {
        // Remoção de nó intermediário ou final
        anterior->proximo = atual->proximo;
    }

    free(atual);
    lista->quantidade--;
    return 1;
}

// FUNCIONALIDADES ADICIONAIS DO SISTEMA

int inserir_inicio(Lista *lista, Musica m) {
    No *novo = (No*) malloc(sizeof(No));
    if (novo == NULL) return 0;

    novo->musica = m;
    novo->proximo = lista->primeiro;
    lista->primeiro = novo;
    lista->quantidade++;
    return 1;
}

int inserir_posicao(Lista *lista, Musica m, int posicao) {
    if (posicao < 1 || posicao > lista->quantidade + 1) return 0; // Posicao invalida
    if (posicao == 1) return inserir_inicio(lista, m);
    if (posicao == lista->quantidade + 1) return inserir(lista, m);

    No *novo = (No*) malloc(sizeof(No));
    if (novo == NULL) return 0;

    novo->musica = m;
    No *atual = lista->primeiro;
    for (int i = 1; i < posicao - 1; i++) {
        atual = atual->proximo;
    }

    novo->proximo = atual->proximo;
    atual->proximo = novo;
    lista->quantidade++;
    return 1;
}

void buscar_por_artista(Lista *lista, const char *artista) {
    if (lista == NULL || lista->primeiro == NULL) {
        printf("\n[Playlist Vazia]\n");
        return;
    }

    printf("\n--- Músicas do artista '%s' ---\n", artista);
    No *atual = lista->primeiro;
    int encontradas = 0;
    while (atual != NULL) {
        if (strcasecmp(atual->musica.artista, artista) == 0) {
            int min = atual->musica.duracao / 60;
            int seg = atual->musica.duracao % 60;
            printf("ID: %d | Título: %s | Álbum: %s | Duração: %02d:%02d\n",
                   atual->musica.id, atual->musica.titulo,
                   atual->musica.album, min, seg);
            encontradas++;
        }
        atual = atual->proximo;
    }
    if (encontradas == 0) {
        printf("Nenhuma música encontrada para este artista.\n");
    }
}

int duracao_total(Lista *lista) {
    if (lista == NULL) return 0;
    int total = 0;
    No *atual = lista->primeiro;
    while (atual != NULL) {
        total += atual->musica.duracao;
        atual = atual->proximo;
    }
    return total;
}

void liberar_lista(Lista *lista) {
    if (lista == NULL) return;
    No *atual = lista->primeiro;
    while (atual != NULL) {
        No *temp = atual;
        atual = atual->proximo;
        free(temp);
    }
    free(lista);
}

// BATERIA AUTOMATIZADA DE CASOS DE TESTE OBRIGATÓRIOS


void executar_testes_obrigatorios() {
    printf("====================================================\n");
    printf("     INICIANDO SUÍTE DE TESTES OBRIGATÓRIOS         \n");
    printf("====================================================\n\n");

    // 1. Criação de uma playlist vazia
    Lista *pl = inicializar_lista();
    printf("[Teste 1] Playlist vazia criada. Quantidade esperada: 0 | Obtida: %d\n", pl->quantidade);

    // 2. Inserção da primeira música
    Musica m1 = {1, "Bohemian Rhapsody", "Queen", "A Night at the Opera", 354};
    inserir(pl, m1);
    printf("[Teste 2] Inserção da primeira música (ID 1). Quantidade esperada: 1 | Obtida: %d\n", pl->quantidade);

    // 3. Inserção no início
    Musica m0 = {0, "Radio Ga Ga", "Queen", "The Works", 348};
    inserir_inicio(pl, m0);
    printf("[Teste 3] Inserção no início (ID 0). Primeiro elemento ID esperado: 0 | Obtido: %d\n", pl->primeiro->musica.id);

    // 4. Inserção no final
    Musica m3 = {3, "Hotel California", "Eagles", "Hotel California", 390};
    inserir(pl, m3);

    // 5. Inserção no meio
    Musica m2 = {2, "Stairway to Heaven", "Led Zeppelin", "Led Zeppelin IV", 482};
    inserir_posicao(pl, m2, 3); // Insere na 3ª posição
    printf("[Teste 4 e 5] Várias músicas inseridas (Início, Meio e Fim).\n");
    imprimir(pl);

    // 6. Busca de música existente
    No *busca1 = buscar(pl, 2);
    printf("[Teste 6] Busca ID 2 existente: %s (Encontrado: %s)\n",
           busca1 ? "SUCESSO" : "FALHA", busca1 ? busca1->musica.titulo : "");

    // 7. Busca de música inexistente
    No *busca2 = buscar(pl, 99);
    printf("[Teste 7] Busca ID 99 inexistente: %s\n", busca2 == NULL ? "SUCESSO (Retornou NULL)" : "FALHA");

    // 8. Busca por artista
    printf("[Teste 8] Busca por Artista 'Queen':\n");
    buscar_por_artista(pl, "Queen");

    // 9. Cálculo da quantidade e duração total
    int dur_sec = duracao_total(pl);
    printf("\n[Teste 9] Quantidade de músicas: %d | Duração Total: %d:%02d minutos\n",
           pl->quantidade, dur_sec / 60, dur_sec % 60);

    // 10. Remoção de música intermediária (ID 2)
    remover(pl, 2);
    printf("\n[Teste 10] Remoção de música intermediária (ID 2). Nova quantidade esperada: 3 | Obtida: %d\n", pl->quantidade);

    // 11. Remoção da primeira música (ID 0)
    remover(pl, 0);
    printf("[Teste 11] Remoção da primeira música (ID 0). Novo ID do primeiro esperado: 1 | Obtido: %d\n", pl->primeiro->musica.id);

    // 12. Remoção da última música (ID 3)
    remover(pl, 3);
    printf("[Teste 12] Remoção da última música (ID 3). Sobrou ID: %d\n", pl->primeiro->musica.id);

    // 13. Remoção da única música da playlist
    remover(pl, 1);
    printf("[Teste 13] Remoção da única música restante (ID 1). Quantidade esperada: 0 | Obtida: %d\n", pl->quantidade);

    // 14. Tentativa de remoção em playlist vazia/inexistente
    int rem_invalida = remover(pl, 999);
    printf("[Teste 14] Tentativa de remoção de ID inexistente: %s\n", rem_invalida == 0 ? "SUCESSO (Rejeitada)" : "FALHA");

    liberar_lista(pl);
    printf("\n====================================================\n");
    printf("     SUÍTE DE TESTES CONCLUÍDA COM SUCESSO!         \n");
    printf("====================================================\n\n");
}

// MENU INTERATIVO

void menu_interativo() {
    Lista *playlist = inicializar_lista();
    int opcao = -1;

    while (opcao != 0) {
        printf("\n================ GERENCIADOR DE PLAYLIST ================\n");
        printf("1. Cadastrar/Inserir música no final\n");
        printf("2. Inserir música no início\n");
        printf("3. Inserir música em uma posição determinada\n");
        printf("4. Exibir todas as músicas (Imprimir Playlist)\n");
        printf("5. Buscar música pelo ID\n");
        printf("6. Buscar músicas pelo nome do Artista\n");
        printf("7. Remover música pelo ID\n");
        printf("8. Exibir quantidade de músicas\n");
        printf("9. Exibir duração total da playlist\n");
        printf("10. Executar Bateria de Testes Automáticos\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        getchar(); // Limpar buffer do teclado

        switch (opcao) {
            case 1:
            case 2:
            case 3: {
                Musica m;
                int pos = 0;
                printf("\nID da música: ");
                scanf("%d", &m.id);
                getchar();
                printf("Título: ");
                fgets(m.titulo, 100, stdin); m.titulo[strcspn(m.titulo, "\n")] = 0;
                printf("Artista: ");
                fgets(m.artista, 100, stdin); m.artista[strcspn(m.artista, "\n")] = 0;
                printf("Álbum: ");
                fgets(m.album, 100, stdin); m.album[strcspn(m.album, "\n")] = 0;
                printf("Duração (em segundos): ");
                scanf("%d", &m.duracao);

                if (opcao == 1) {
                    inserir(playlist, m);
                } else if (opcao == 2) {
                    inserir_inicio(playlist, m);
                } else {
                    printf("Informe a posição (1 até %d): ", playlist->quantidade + 1);
                    scanf("%d", &pos);
                    if (!inserir_posicao(playlist, m, pos)) {
                        printf("Posição inválida!\n");
                        break;
                    }
                }
                printf("Música cadastrada com sucesso!\n");
                break;
            }
            case 4:
                imprimir(playlist);
                break;
            case 5: {
                int id;
                printf("Digite o ID para busca: ");
                scanf("%d", &id);
                No *res = buscar(playlist, id);
                if (res != NULL) {
                    int min = res->musica.duracao / 60;
                    int seg = res->musica.duracao % 60;
                    printf("\n[Encontrado] ID: %d | Título: %s | Artista: %s | Álbum: %s | Duração: %02d:%02d\n",
                           res->musica.id, res->musica.titulo, res->musica.artista, res->musica.album, min, seg);
                } else {
                    printf("\nMúsica com ID %d não encontrada.\n", id);
                }
                break;
            }
            case 6: {
                char artista[100];
                printf("Digite o nome do artista: ");
                fgets(artista, 100, stdin);
                artista[strcspn(artista, "\n")] = 0;
                buscar_por_artista(playlist, artista);
                break;
            }
            case 7: {
                int id;
                printf("Digite o ID da música a ser removida: ");
                scanf("%d", &id);
                if (remover(playlist, id)) {
                    printf("Música removida com sucesso!\n");
                } else {
                    printf("Música com ID %d não encontrada ou playlist vazia.\n", id);
                }
                break;
            }
            case 8:
                printf("\nQuantidade total de músicas armazenadas: %d\n", playlist->quantidade);
                break;
            case 9: {
                int total_seg = duracao_total(playlist);
                printf("\nDuração total da playlist: %d minutos e %d segundos (%d segundos)\n",
                       total_seg / 60, total_seg % 60, total_seg);
                break;
            }
            case 10:
                executar_testes_obrigatorios();
                break;
            case 0:
                printf("\nEncerrando e liberando memória...\n");
                break;
            default:
                printf("\nOpção inválida!\n");
        }
    }

    liberar_lista(playlist);
}

int main() {
    menu_interativo();
    return 0;
}
