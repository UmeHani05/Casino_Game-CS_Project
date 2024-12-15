#include <iostream>
#include <ctime>
#include <cstdlib>
#include <cstring>

using namespace std;

const char* ranks[] = {"King", "Queen", "Jack", "Ace", "2", "3", "4", "5", "6", "7", "8", "9", "10"}; // added Jack
const char* suits[] = {"Spades", "Hearts", "Diamond", "Clubs"};
const int points[] = {10, 10, 10, 11, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // Jack has a value of 10

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
        if (strcmp(ranks[rank], "Ace") == 0) {
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
    cout << endl;
    cout << "Total value: " << handValue(hand, cardnum) << '\n';
    cout << endl;
    cout << endl;
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

    cout<< "\t Welcome to the house of fortune, where luck is just a spin away.\n \t\t   Place your bets and get ready to win big! \n";
    cout<<"\t ================================================================= \n";
    cout<<endl;
    
    //placing bets
    do
    {
        cout << "\t\t" << "Place your bets! $ ";
        cin >> playerbet;
        cout << endl;
        cout << "\t\t" << "You placed a bet of $" << playerbet << ".\n \t\tPress Y/y to confirm and N/n to change your bet: ";
        cin >> chc;
        cout<< endl;
        cout << endl;
    } while (chc == 'n' || chc == 'N');
    
    double dealerbet = playerbet;
// players turn 
do {
    showHand(player, playercard, "Player");
        showHand (dealer, dealercard, "Dealer");
        cout << "\t\t"<<"Do you want to stand or hit or double down by increasing your bet by 100% ? (s/h/d): ";
        cin >> chc;
        cout<<endl;
        cout << endl;
        if (chc == 'h' || chc == 'H') {
            distribute(deck, deckindex, player, playercard);
            if (handValue(player, playercard) > 21) {
            showHand (player, playercard, "Player");
            cout << "\t\t"<<"--Player busts! You lose. $" << playerbet << " lost!--\n";
            return 0;
            }
        } else if (chc == 'd' || chc == 'D')
        {
            playerbet = playerbet * 2;
            dealerbet = playerbet;
            cout << "\t\t" << "Bet increased to $" << playerbet << endl; 
            cout << endl;
        }
        
    } while (chc == 'h' || chc == 'H');
// Determine the winner
    int playerValue = handValue(player, playercard);
    int dealerValue = handValue(dealer, dealercard);

    distribute(deck, deckindex, dealer, dealercard);
    showHand(player, playercard, "Player");
    showHand(dealer, dealercard, "Dealer");

    if (dealerValue > 21) {
        cout << "\t\t" << "--Dealer busts! You win $" << playerbet + dealerbet << "!--\n";
    } else if (playerValue > dealerValue) {
        cout << "\t\t" << "--You win $" << playerbet + dealerbet << "!--\n";;
    } else if (playerValue < dealerValue) {
        cout << "\t\t" << "--You lose. $" << playerbet << " lost!--\n";
    } else {
        cout << "\t\t" << "--It's a tie!--" << endl;
    }

    return 0;
}
