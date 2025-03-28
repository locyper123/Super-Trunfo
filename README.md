#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

// Estrutura para representar uma carta de carro
typedef struct {
    char nome[50];
    int velocidadeMaxima;
    int aceleracao;
    int peso;
    int potencia;
} Carta;

// Função para criar o baralho de cartas
void criarBaralho(Carta baralho[], int *numCartas) {
    *numCartas = 6;
    
    strcpy(baralho[0].nome, "Porsche 911");
    baralho[0].velocidadeMaxima = 310;
    baralho[0].aceleracao = 280;
    baralho[0].peso = 1450;
    baralho[0].potencia = 385;

    strcpy(baralho[1].nome, "Ferrari 488");
    baralho[1].velocidadeMaxima = 330;
    baralho[1].aceleracao = 300;
    baralho[1].peso = 1420;
    baralho[1].potencia = 670;

    strcpy(baralho[2].nome, "Lamborghini Aventador");
    baralho[2].velocidadeMaxima = 350;
    baralho[2].aceleracao = 290;
    baralho[2].peso = 1600;
    baralho[2].potencia = 740;

    strcpy(baralho[3].nome, "BMW M5");
    baralho[3].velocidadeMaxima = 305;
    baralho[3].aceleracao = 270;
    baralho[3].peso = 1950;
    baralho[3].potencia = 600;

    strcpy(baralho[4].nome, "Mercedes AMG GT");
    baralho[4].velocidadeMaxima = 315;
    baralho[4].aceleracao = 275;
    baralho[4].peso = 1540;
    baralho[4].potencia = 522;

    strcpy(baralho[5].nome, "Audi R8");
    baralho[5].velocidadeMaxima = 330;
    baralho[5].aceleracao = 285;
    baralho[5].peso = 1630;
    baralho[5].potencia = 610;
}

// Função para embaralhar as cartas
void embaralharCartas(Carta baralho[], int numCartas) {
    srand(time(NULL));
    for (int i = 0; i < numCartas * 2; i++) {
        int pos1 = rand() % numCartas;
        int pos2 = rand() % numCartas;
        Carta temp = baralho[pos1];
        baralho[pos1] = baralho[pos2];
        baralho[pos2] = temp;
    }
}

// Função para exibir carta com formatação
void exibirCarta(Carta carta, const char* proprietario) {
    printf("\n----- CARTA DO %s -----\n", proprietario);
    printf("Nome: %s\n", carta.nome);
    printf("Velocidade Máxima: %d km/h\n", carta.velocidadeMaxima);
    printf("Aceleração: %d (0-100 km/h)\n", carta.aceleracao);
    printf("Peso: %d kg\n", carta.peso);
    printf("Potência: %d cv\n", carta.potencia);
    printf("----------------------------\n");
}

// Função para comparar cartas
int compararCartas(Carta carta1, Carta carta2, int atributo) {
    switch(atributo) {
        case 1: return carta1.velocidadeMaxima > carta2.velocidadeMaxima;
        case 2: return carta1.aceleracao > carta2.aceleracao;
        case 3: return carta1.peso < carta2.peso;  // Menos peso é melhor
        case 4: return carta1.potencia > carta2.potencia;
        default: return 0;
    }
}

int main() {
    Carta baralho[10];
    int numCartas;
    criarBaralho(baralho, &numCartas);
    embaralharCartas(baralho, numCartas);

    Carta jogador[5], computador[5];
    int cartasJogador = 0, cartasComputador = 0;

    // Distribuir cartas
    for (int i = 0; i < numCartas; i++) {
        if (i % 2 == 0) {
            jogador[cartasJogador++] = baralho[i];
        } else {
            computador[cartasComputador++] = baralho[i];
        }
    }

    int rodada = 1;
    while (cartasJogador > 0 && cartasComputador > 0) {
        printf("\n=== RODADA %d ===\n", rodada);
        
        // Mostrar cartas
        exibirCarta(jogador[0], "JOGADOR");
        exibirCarta(computador[0], "COMPUTADOR");

        // Escolher atributo
        printf("\nEscolha um atributo para comparar:\n");
        printf("1. Velocidade Máxima\n");
        printf("2. Aceleração\n");
        printf("3. Peso\n");
        printf("4. Potência\n");
        
        int atributo;
        printf("Sua escolha: ");
        scanf("%d", &atributo);

        // Comparar cartas
        int resultado = compararCartas(jogador[0], computador[0], atributo);

        if (resultado) {
            printf("\n🏆 Você venceu a rodada! 🏆\n");
            // Passa carta do computador para o jogador
            jogador[cartasJogador++] = computador[0];
            
            // Remove primeira carta do computador
            for (int i = 0; i < cartasComputador - 1; i++) {
                computador[i] = computador[i+1];
            }
            cartasComputador--;
        } else {
            printf("\n❌ Computador venceu a rodada! ❌\n");
            // Passa carta do jogador para o computador
            computador[cartasComputador++] = jogador[0];
            
            // Remove primeira carta do jogador
            for (int i = 0; i < cartasJogador - 1; i++) {
                jogador[i] = jogador[i+1];
            }
            cartasJogador--;
        }

        printf("\nSuas cartas: %d | Cartas do Computador: %d\n", cartasJogador, cartasComputador);
        rodada++;

        printf("\nPressione Enter para continuar...");
        while(getchar() != '\n');
        getchar();
    }

    // Resultado final
    if (cartasJogador > 0) {
        printf("\n🎉 PARABÉNS! Você venceu o jogo de Super Trunfo! 🎉\n");
    } else {
        printf("\n❌ Que pena! O Computador venceu o jogo. ❌\n");
    }

    return 0;
}
