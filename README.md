#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int rollDice() {
    return (rand() % 6) + 1;
}

int main() {
    printf("Välkommen till kasinot!\n");
    printf("Satsa på om tärningarna kommer att ge ett högre (H) eller lägre (L) värde än 7.\n");

    srand(time(0)); // Seed för slumpmässiga nummer

    while (1) {
        printf("Välj H eller L: ");
        char playerChoice;
        scanf(" %c", &playerChoice);
        playerChoice = toupper(playerChoice);

        int dice1 = rollDice();
        int dice2 = rollDice();
        int total = dice1 + dice2;

        printf("Tärning 1: %d\n", dice1);
        printf("Tärning 2: %d\n", dice2);
        printf("Totalt värde: %d\n", total);

        if ((playerChoice == 'H' && total > 7) || (playerChoice == 'L' && total < 7)) {
            printf("Du vann!\n");
        } else {
            printf("Du förlorade.\n");
        }

        printf("Vill du spela igen? (Ja/Nej): ");
        char playAgain[5];
        scanf(" %s", playAgain);

        if (toupper(playAgain[0]) != 'J') {
            break;
        }
    }

    printf("Tack för att du spelade på kasinot!\n");
    return 0;
}
