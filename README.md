//Casino_Game-CS_Project
//blackjack game with max points of 21

#include <iostream>
#include<ctime>
#include<cstdlib>
#include<cstring>

using namespace std;

const char* num[]={"King", "Queen", "Ace", "2", "3", "4", "5", "6", "7", "8", "9", "10"}; //using arrays to make suits
const char* suit[]={"Spades", "Hearts", "Daimond", "Clubs"};
const int points[]={2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10, 11}; //using arrays to give points

void shuffle(int* deck, int size){
	for(int i=0; i<size i++){
		int r = i+(rand()%(size-i));
		swap(deck[i], deck[r]); //using swap and rand to shuffle in a way the shuffle is not the same each time
	}
}

void deck52(int* deck){ //choosing a card
	for (int i=0; i<52; i++){
		gdeck[i]=i;
	}
}


