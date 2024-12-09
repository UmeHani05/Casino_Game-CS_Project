//blackjack casino game

#include <iostream>
#include <ctime>
#include <cstdlib>
#include <cstring>

using namespace std;

const char* ranks[] = {"King", "Queen", "Ace", "2", "3", "4", "5", "6", "7", "8", "9", "10"}; // using arrays to make ranks
const char* suits[] = {"Spades", "Hearts", "Diamond", "Clubs"};
const int points[] = {10, 10, 11, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // using arrays to give points

void shuffle(int* deck, int size) {
    for (int i = 0; i < size; i++) {
        int r = i + (rand() % (size - i));
        swap(deck[i], deck[r]); // using swap and rand to shuffle in a way the shuffle is not the same each time
    }
}

void deck52(int* deck) { // initializing the deck
    for (int i = 0; i < 52; i++) {
        deck[i] = i;
    }
}

void printCard(int card) {
    int rank = card % 13;
    int suit = card / 13;
    cout << ranks[rank] << " of " << suits[suit] << " with the points: " << points[rank] << '\n';
}

void distribute(int* deck, int& deckindex, int* hand, int& cardnum) {
    hand[cardnum] = deck[deckindex];
    cardnum++;
    deckindex++;
}

int handValue(const int* hand, int cardnum) {
    int value = 0;
    int aces = 0;
    for (int i = 0; i < cardnum; i++) {
        int rank = hand[i] % 13;
        value += points[rank];
        if (ranks[rank] == "Ace") {
            aces++;
        }
    }
    while (value > 21 && aces > 0) {
        value -= 10;
        aces--;
    }
    return value;
}

void showHand(const int* hand, int cardnum, const char* player) {
    cout << player << "'s hand: " << '\n';
    for (int i = 0; i < cardnum; i++) {
        printCard(hand[i]);
    }
    cout << "Total value: " << handValue(hand, cardnum) << '\n';
}

int main() {
    srand(time(0));

    int deck[52];
    deck52(deck);
    shuffle(deck, 52);

    const int max = 11;
    int player[max];
    int dealer[max];
    int playercard = 0;
    int dealercard = 0;
    int deckindex = 0;

    distribute(deck, deckindex, player, playercard);
    distribute(deck, deckindex, dealer, dealercard);
    distribute(deck, deckindex, player, playercard);
    distribute(deck, deckindex, dealer, dealercard);

    char chc;
    do {
        showHand(player, playercard, "Player");
        cout << "Do you want to hit or stand? (h/s): ";
        cin >> chc;
        if (chc == 'h' || chc == 'H') {
            distribute(deck, deckindex, player, playercard);
            if (handValue(player, playercard) > 21) {
                cout << "Player busts! You lose.\n";
                return 0;
            }
        }
    } while (chc == 'h' || chc == 'H');
    
    return 0;
}
