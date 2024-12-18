#include <iostream>
#include <ctime>
#include <cstdlib>
#include <cstring>

using namespace std;

const char* ranks[] = {"King", "Queen", "Jack", "Ace", "2", "3", "4", "5", "6", "7", "8", "9", "10"}; // added 13 cards 
const char* suits[] = {"Spades", "Hearts", "Diamond", "Clubs"}; // adding 4 suits
const int points[] = {10, 10, 10, 11, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // assigning values to cards 

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
    cout <<"\t\t" << ranks[rank] << " of " << suits[suit] << " with the points: " << points[rank] << '\n'; //printing card and its respective value 
}

void distribute(int* deck, int& deckindex, int* hand, int& cardnum) { // distributing the cards
    hand[cardnum] = deck[deckindex];
    cardnum++;
    deckindex++;
}

int handValue(const int* hand, int cardnum) { //determining the total value of hand 
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

void showHand(const int* hand, int cardnum, const char* player) { // displaying the hand 
    cout << "\t\t\t" << player << "'s hand: " << '\n';
    for (int i = 0; i < cardnum; i++) {
        printCard(hand[i]);
    }
    cout << endl;
    cout << "\t\tTotal value: " << handValue(hand, cardnum) << '\n';
    cout << endl;
    cout << endl;
}

int main() {
    srand(time(0)); // so that the value changes with time
    
    // declaring variables and assigning them values

    int deck[52];
    deck52(deck);
    shuffle(deck, 52);

    const int max = 11;
    int player[max];
    int dealer[max];
    int playercard = 0;
    int dealercard = 0;
    int deckindex = 0;
    double playerbet = 0;

    // distributing 2 cards to player and 1 to dealer
    distribute(deck, deckindex, player, playercard);
    distribute(deck, deckindex, dealer, dealercard);
    distribute(deck, deckindex, player, playercard);

    char chc;

    cout<< "\t Welcome to the house of fortune, where luck is just a spin away.\n \t\tPlace your bets and get ready to win big! \n";
    cout<<"\t ================================================================= \n";
    cout<<endl;
    
    //placing bets
    do
    {
        cout << "\t\t" << "Place your bets! $";
        cin >> playerbet;
        cout << endl;
        cout << "\t\t" << "You placed a bet of $" << playerbet << ".\n \t\tPress Y/y to confirm and N/n to change your bet: "; // confirming bet
        cin >> chc;
        while (chc != 'n' && chc != 'N' && chc != 'y' && chc != 'Y') // handling wrong options
        {
            cout << "\t\tPlease choose y/n! You have money on the line! ";
            cin >> chc;
        }
        cout<< endl;
        cout << endl;
    } while (chc == 'n' || chc == 'N');
    
    double dealerbet = playerbet; //equating dealer's bet and player's bet
// players turn 
    do 
    {
        showHand(player, playercard, "Player"); // displaying player's cards
        if (handValue(player, playercard) == 21) 
            {  // condition to check blackjack 
            cout << "\t\t"<<"--Blackjack! You win $" << playerbet + dealerbet << " !--\n"; // displaying win
            return 0;
            }
        showHand (dealer, dealercard, "Dealer");
        cout << "\t\t"<<"Do you want to stand or hit or double down by increasing your bet by 100% ? (s/h/d): ";  // providing options
        cin >> chc;
        while (chc != 's' && chc != 'S' && chc != 'h' && chc != 'H' && chc != 'd' && chc != 'D')  // handing wrong options 
        {
            cout << "\t\tPlease choose s/h/d! You have money on the line! ";
            cin >> chc;
        }
        cout<<endl;
        cout << endl;
        if (chc == 'h' || chc == 'H') // dealing another card
            {   
            distribute(deck, deckindex, player, playercard);
            if (handValue(player, playercard) > 21) {  // condition to check if the player busts 
            showHand (player, playercard, "Player");
            cout << "\t\t"<<"--Player busts! You lose. $" << playerbet << " lost!--\n"; // displaying loss
            return 0;
            }
            if (handValue(player, playercard) == 21)  // condition to check blackjack 
            { 
            showHand (player, playercard, "Player");
            cout << "\t\t"<<"--Blackjack! You win $" << playerbet + dealerbet << " !--\n"; // displaying win
            return 0;
            }
        } else if (chc == 'd' || chc == 'D') //doubling the bet
        {
            playerbet = playerbet * 2;
            dealerbet = playerbet;
            cout << "\t\t" << "Bet increased to $" << playerbet << endl; 
            cout << endl;
        }
        
    } while (chc == 'h' || chc == 'H'); //looping as long as player chooses to hit 
    distribute(deck, deckindex, dealer, dealercard); // giving the dealer 2nd card
// Determine the winner
    int playerValue = handValue(player, playercard);
    int dealerValue = handValue(dealer, dealercard);

    
    showHand(player, playercard, "Player"); // displaying player's final hand 
    showHand(dealer, dealercard, "Dealer"); // displaying dealer's complete hand 

    if (dealerValue > 21)  // condition for the dealer to bust 
    {
        cout << "\t\t" << "--Dealer busts! You win $" << playerbet + dealerbet << "!--\n";
    } else if (playerValue > dealerValue)  // condition for the player to win
    {
        cout << "\t\t" << "--You win $" << playerbet + dealerbet << "!--\n";;
    } else if (playerValue < dealerValue)  // condition for the player to win
    {
        cout << "\t\t" << "--You lose. $" << playerbet << " lost!--\n";
    } else 
    {
        cout << "\t\t" << "--It's a tie!--" << endl; // declaring a tie 
    }

    return 0;
}
