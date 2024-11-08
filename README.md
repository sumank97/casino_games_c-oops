# casino_games_c-oops
#include <iostream>
#include <string> // Needed to use strings
#include <cstdlib> // Needed to use random numbers
#include <ctime>
#include <string.h>

using namespace std;
//class for display related methods
class Display
{
public:
    void printMessageCenter(const char* message);

    void welcomeMessage();

    void rules();
};

void Display::rules()
{
   ("RULES OF THE GAME");
    cout << "\n\n\n\n";
    cout<<"THE BETTING RULES\n ";
    cout<<"------------------\n";
    cout << "\n1. Choose any number between 1 to 10\n";
    cout << "\n2. If you win you will get 10 times of money you bet\n";
    cout << "\n3. If you bet on wrong number you will lose your betting amount\n\n";
}
//Align the message
void Display::printMessageCenter(const char* message)
{
    int len =0;
    int pos = 0;
    //calculate how many space need to print
    len = (78 - strlen(message))/2;
    cout << "\t\t\t";
    for(pos =0 ; pos < len ; pos++)
    {
        //print space
        cout <<" ";
    }
    //print message
    cout << message;
}
//Head message







//Display message
void Display::welcomeMessage()
{


    cout << "\       WELCOME\n";
    cout << "\t TO \n";
    cout << "\     CASINO GAME";


}
//Main class of the project
class PlayerInfo:public Display
{
public:
    int getGuessNumber();
    void setGuessNumber();
    void setAmount();
    float getAmount();
    int getdice();
    void updateAmount(const bool isWin);
    void setBettingAmount();
    bool isPlayerWin();
    void init();
    void displayMessageAfterPlay(const bool isWin);
    PlayerInfo():m_amount(0.00),m_bettingAmount(0.00),m_guessNumber(-1)
    {
    }
private:
    float m_amount; //Total balance of player
    float m_bettingAmount; //Betting Amount
    int m_guessNumber; //Number guessed by player
};
//Set Wallet Amount
void PlayerInfo::setAmount()
{

    do
    {
        cout << "\n\nEnter Deposit amount to play game : $";
        cin >> m_amount;
        if(m_amount < 0.01f)
        {
            cout << "\n\nPlease Enter valid amount to play the Game!!";
        }
    }
    while(m_amount < 0.01f);
}
//Get wallet Amount
float PlayerInfo::getAmount()
{
    return m_amount;
}
int PlayerInfo::getGuessNumber()
{
    return m_guessNumber;
}
//Get number from player
void PlayerInfo::setGuessNumber()
{

    do
    {
        cout << "\n\nGuess your number to bet between 1 to 10 :";
        cin >> m_guessNumber;
        if(m_guessNumber <= 0 || m_guessNumber > 10)
            cout << "\n\nPlease check the number!! should be between 1 to 10\n"
                 <<"\nRe-enter the number\n ";
    }
    while(m_guessNumber <= 0 || m_guessNumber > 10);
}
//Update wallet amount
void PlayerInfo::updateAmount(const bool isWin)
{
    m_amount = isWin ? (m_amount + (m_bettingAmount *10)): (m_amount - m_bettingAmount);
}
//Set betting amount
void PlayerInfo::setBettingAmount()
{

    do
    {
        cout <<"\n\nEnter money to bet : $";
        cin >> m_bettingAmount;
        if(m_bettingAmount > m_amount)
        {
            cout << "\n\nYour wallet amount is $" << m_amount;
            cout << "\n\nYour betting amount is more than your current balance";
        }
    }
    while(m_bettingAmount > m_amount);
}
//Check is player w
bool PlayerInfo::isPlayerWin()
{
    // Will hold the randomly generated integer between 1 and 10
    const int dice = rand()%10 + 1;
    return ((dice == getGuessNumber())? true:false);
}
//Init the game.
void PlayerInfo::init()
{
    welcomeMessage();
    // "Seed" the random generator
    srand(time(0));
    rules();
}
//Display message after each play
void PlayerInfo::displayMessageAfterPlay(const bool isWin)
{
    if(isWin)
    {
        cout << "\n\nGood Luck!! You won $" << m_bettingAmount * 10;
        cout << "\n\nNow update Amount is $" << m_amount;
    }
    else
    {
        cout << "\n\nBad Luck this time !! You lost $"<< m_bettingAmount <<"\n";
        cout << "\n\nNow update Amount is $" << m_amount;
    }
}
int main()
{
    class PlayerInfo obj_player ;
    char choice;
    //init game
    obj_player.init();
    //Set wallet amount
    obj_player.setAmount();
    do
    {
        cout << "\n\nYour current balance is $" << obj_player.getAmount() << "\n";
        //Set bet amount
        obj_player.setBettingAmount();
        //Set guess number
        obj_player.setGuessNumber();
        //Check whether player lose or win the game
        const bool isPlayerWin = obj_player.isPlayerWin();
        //Update wallet amount
        obj_player.updateAmount(isPlayerWin);
        //Display the result after each play
        obj_player.displayMessageAfterPlay(isPlayerWin);
        //Check wallet amount and accordingly ask the player
        //to play again
        if(obj_player.getAmount() == 0.00f)
        {
            cout << "You have no money to play, Good Bye..";
            break;
        }
        //Ask use choice for replay
        cout << "\n-->Do you want to play again (y/n)? ";
        cin >> choice;
    }
    while(choice =='Y'|| choice=='y');


    cout << "\n\nThanks for playing game. Your balance amount is $" << obj_player.getAmount() << "\n\n";

    return 0;
}
