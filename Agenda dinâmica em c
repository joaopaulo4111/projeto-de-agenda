#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Função para limpar o buffer do teclado
void limpar_buffer() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {}
}

typedef struct contato {
    long long int telefone;
    char nome[50];
} contato;

typedef struct agenda {
    int capacidade_agenda;
    int qtd_contatos_preenchidos;
    contato *contatos;
} agenda;

void mostrar_contato(contato *x);
agenda *cria_agenda(int capacidade_agenda);
void mostrar_agenda(agenda *a);
void menu(agenda *a);
void incluir_contato(contato *novo);
int excluir_contato(agenda *a);
int buscar_contato(agenda *a);

int main() {
    agenda *minha_agenda;

    minha_agenda = cria_agenda(2);
    menu(minha_agenda);

    free(minha_agenda->contatos);
    free(minha_agenda);

    return 0;
}

void menu(agenda *a) {
    int opcao;

    do {
        printf("\nQuantidade de contatos na agenda: %d\n", a->qtd_contatos_preenchidos);
        printf("Capacidade da agenda: %d\n", a->capacidade_agenda);
        printf("1 - Incluir Contato\n");
        printf("2 - Buscar Contato\n");
        printf("3 - Excluir Contato\n");
        printf("4 - Mostrar Agenda Inteira\n");
        printf("5 - Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);
        limpar_buffer();  // limpa o '\n' deixado no buffer

        switch (opcao) {
            case 1:
                if (a->qtd_contatos_preenchidos >= a->capacidade_agenda) {
                    a->contatos = realloc(a->contatos, sizeof(contato) * a->capacidade_agenda * 2);
                    a->capacidade_agenda *= 2;
                }

                incluir_contato(&a->contatos[a->qtd_contatos_preenchidos]);
                a->qtd_contatos_preenchidos++;
                break;

            case 2: {
                int busca = buscar_contato(a);
                if (busca == -1) {
                    printf("\nContato nao encontrado.\n");
                } else {
                    mostrar_contato(&a->contatos[busca]);
                }
                break;
            }

            case 3:
                excluir_contato(a);
                if (a->qtd_contatos_preenchidos <= a->capacidade_agenda * 0.2 && a->capacidade_agenda > 2) {
                    a->capacidade_agenda /= 2;
                    a->contatos = realloc(a->contatos, sizeof(contato) * a->capacidade_agenda);
                }
                break;

            case 4:
                mostrar_agenda(a);
                break;

            case 5:
                printf("\nSaindo do programa...\n");
                break;

            default:
                printf("\nOpcao invalida.\n");
        }

    } while (opcao != 5);
}

void incluir_contato(contato *novo) {
    printf("Digite o nome do contato: ");
    fgets(novo->nome, sizeof(novo->nome), stdin);
    novo->nome[strcspn(novo->nome, "\n")] = '\0';  // remove newline

    printf("Digite o telefone do contato: ");
    scanf("%lld", &novo->telefone);
    limpar_buffer();
}

int buscar_contato(agenda *a) {
    char nome_de_busca[50];

    printf("Digite o nome que deseja buscar: ");
    fgets(nome_de_busca, sizeof(nome_de_busca), stdin);
    nome_de_busca[strcspn(nome_de_busca, "\n")] = '\0';

    for (int i = 0; i < a->qtd_contatos_preenchidos; i++) {
        if (strcmp(nome_de_busca, a->contatos[i].nome) == 0) {
            return i;
        }
    }

    return -1;
}

void mostrar_contato(contato *x) {
    printf("\nNome: %s\n", x->nome);
    printf("Telefone: %lld\n", x->telefone);
}

agenda *cria_agenda(int capacidade_agenda) {
    agenda *nova = malloc(sizeof(agenda));
    nova->contatos = malloc(sizeof(contato) * capacidade_agenda);
    nova->capacidade_agenda = capacidade_agenda;
    nova->qtd_contatos_preenchidos = 0;
    return nova;
}

void mostrar_agenda(agenda *a) {
    if (a->qtd_contatos_preenchidos == 0) {
        printf("\nAgenda vazia.\n");
    } else {
        for (int i = 0; i < a->qtd_contatos_preenchidos; i++) {
            printf("\nContato %d\n", i + 1);
            mostrar_contato(&a->contatos[i]);
        }
    }
}

int excluir_contato(agenda *a) {
    int busca = buscar_contato(a);

    if (busca == -1) {
        printf("\nContato nao esta na agenda.\n");
        return 0;
    }

    for (int i = busca; i < a->qtd_contatos_preenchidos - 1; i++) {
        a->contatos[i] = a->contatos[i + 1];
    }

    a->qtd_contatos_preenchidos--;
    printf("Contato excluido com sucesso.\n");
    return 1;
}
