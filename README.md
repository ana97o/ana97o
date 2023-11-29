
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h>

#define MAX_BET 1000

// Function to calculate and update the player's balance based on the game rules
int calculate_balance(int balance, int bet, int player_total, int dealer_total) {
    if (player_total == 21) {
        balance += bet * 2;
        printf("Blackjack! You won $%d. Your balance now is $%d\n\n\n", bet * 2, balance);
    } else if (player_total > 21) {
        balance -= bet;
        printf("Bust! You lost $%d. Your balance now is $%d\n\n\n", bet, balance);
    } else if (dealer_total > 21 || player_total > dealer_total) {
        balance += bet;
        printf("You won $%d. Your balance now is $%d\n\n\n", bet, balance);
    } else if (player_total < dealer_total) {
        balance -= bet;
        printf("You lost $%d. Your balance now is $%d\n\n\n", bet, balance);
    } else {
        printf("It's a draw. Your balance remains $%d\n\n\n", balance);
    }

    return balance;
}

// Function to get the value of a card
int get_card_value(int card) {
    if (card > 10) {
        return 10; // Face cards (J, Q, K) have a value of 10
    } else if (card == 1) {
        return 11; // Ace can be 1 or 11, we start with 11
    } else {
        return card; // Number cards have their face value
    }
}

// Function to draw a card
int draw_card() {
    return rand() % 13 + 1; // Random card value between 1 and 13
}

int main() {
    int balance = 1000;
    int bet, player_total, dealer_total, card;

    srand(time(NULL)); // Seed for random numbers

    printf("Welcome to the Simple Blackjack Game!\n");

    do {
        printf("Your current balance: $%d\n", balance);
        printf("Place your bet (max $%d, enter 0 to quit): ", MAX_BET);

        // Read bet amount from the user
        if (scanf("%d", &bet) != 1) {
            printf("Error reading the bet amount. Closing the game.\n");
            return -1;
        }

        // Close the game if the bet equals zero
        if (bet == 0) {
            printf("Thank you for playing! Exiting the game.\n");
            return 0;
        }

        // Validate the bet
        if (bet < 0 || bet > MAX_BET) {
            printf("Invalid bet. Please enter an amount between 1 and %d.\n\n", MAX_BET);
            continue;
        }

        // Initialize totals for player and dealer
        player_total = 0;
        dealer_total = 0;

        // Deal two cards to the player and one to the dealer
        player_total += get_card_value(card = draw_card());
        printf("Your first card: %d\n", card);
        player_total += get_card_value(card = draw_card());
        printf("Your second card: %d\n", card);

        dealer_total += get_card_value(card = draw_card());
        printf("Dealer's card: %d\n", card);

        // Player's turn
        char choice;
        do {
            printf("Your current total: %d\n", player_total);
            printf("Do you want to hit (h) or stand (s)? ");
            scanf(" %c", &choice);

            if (choice == 'h') {
                player_total += get_card_value(card = draw_card());
                printf("You drew a %d\n", card);
            }

        } while (choice == 'h' && player_total < 21);

        // Dealer's turn
        while (dealer_total < 17) {
            dealer_total += get_card_value(card = draw_card());
            printf("Dealer drew a %d\n", card);
        }

        // Calculate and update the balance based on the game result
        balance = calculate_balance(balance, bet, player_total, dealer_total);

    } while (balance > 0);

    printf("Game over! You're out of money.\n");
    return 0;
}
