class Deck; // class forward initialization

namespace Settings {

    const int bust = 21;
    const int stopDealerDraw = 17;

    void dealerBust() {

        cout << "The dealer went bust!\n";

    }

    void playerBust() {

        cout << "You went bust!\n";

    }

}

struct Card {

    enum CardRank {

        ace = 1,
        two,
        three,
        four,
        five,
        six,
        seven,
        eight,
        nine,
        ten,
        jack,
        queen,
        king,
        max_cards = 13,

    };

    enum Suit {

        clubs,
        diamonds,
        hearts,
        spades,
        max_suits,

    };

    CardRank rank {};
    Suit suit {};



    friend class Deck; // class Deck is a friend of struct Card

    friend std::ostream& operator<<(std::ostream& out, const Card& card) {

        switch (card.rank) {

            case ten:
            case jack:
            case queen:
            case king:
                out << 10;
                break;
            case ace:
                out << 11;
                break;
            default:
                out << static_cast<int>(card.rank);
                break;

        }

        switch (card.suit) {

            case clubs:
                out << 'C';
                break;
            case diamonds:
                out << 'D';
                break;
            case hearts:
                out << 'H';
                break;
            case spades:
                out <<'S';
                break;
            default:
                out << static_cast<char>(card.suit);
                break;

        }

        return out;

    }

    int returnCardValue(CardRank rank) {

        switch (rank) {

            case Card::jack:
            case Card::queen:
            case Card::king:
                return 10;
            case Card::ace:
                return 11;
            default:
                return rank;

        }

    }

    static constexpr std::array<CardRank, max_cards> allRanks {

        ace,
        two,
        three,
        four,
        five,
        six,
        seven,
        eight,
        nine,
        ten,
        jack,
        queen,
        king,

    };

    static constexpr std::array<Suit, max_suits> allSuits {

        clubs,
        diamonds,
        hearts,
        spades,

    };

};

constexpr std::array<Card::CardRank, Card::max_cards> Card::allRanks;
constexpr std::array<Card::Suit, Card::max_suits> Card::allSuits;

class Deck {

private:

    std::array<Card, 52> m_deck {};
    Card* m_nextCard = &m_deck[1];

public:

    Deck() { // default constructor to initialize the array of m_cards

        int index = 0;
        for (Card::Suit suit : Card::allSuits) {
            for (Card::CardRank rank : Card::allRanks) {
                m_deck[index].rank = rank;
                m_deck[index].suit = suit;
                ++index;
            }
        }
        m_nextCard = &m_deck[0]; // reset pointer to beginning of the deck

    }

    Card dealCard() {

        assert(!m_deck.empty() && "Deck is empty.\n");

        Card nextCard = *m_nextCard;
        ++m_nextCard;

        if (m_nextCard == m_deck.end()) {
            // Reset the pointer to the beginning of the deck
            m_nextCard = &m_deck[0];
        }

        return nextCard;

    }

    void shuffle() {

        std::shuffle(m_deck.begin(), m_deck.end(), Random::mt);

        m_nextCard = &m_deck[0]; // Reset pointer to beginning of deck

    }

};

struct Player {

    bool drawCard(Deck& deck) {

        int playerScore = 0;
        int dealerScore = 0;

        // Dealer draws 1 card

        cout << "The dealer is showing: ";
        Card dealersCard = deck.dealCard(); // Deal card to dealer
        dealerScore += dealersCard.returnCardValue(dealersCard.rank);
        cout << dealersCard << " (" << dealerScore << ")" << '\n';

        // Players turn

        cout << "You are showing: ";
        // Deal 2 initial cards to the player
        Card playersCard1 = deck.dealCard();
        Card playersCard2 = deck.dealCard();
        playerScore += playersCard1.returnCardValue(playersCard1.rank);
        playerScore += playersCard2.returnCardValue(playersCard2.rank);

        if (playersCard1.rank == Card::ace && playersCard2.rank == Card::ace) {
            if (playerScore == 22) { // If dealt Ace/Ace
                playerScore = 12; // Player score is 12
            }
        }
        cout << playersCard1 << ' ' << playersCard2
        << " (" << playerScore << ")" << '\n'; // Print player score

        // dealersCard will deal another card for Dealer
        // playersCard will deal another card for Player
        Card playersCard = deck.dealCard();


        // Handle logic for players turn here
        bool endTurn = false;
        while (!endTurn || playerScore < Settings::bust) {
            cout << "(h) to hit, or (s) to stand: ";
            char input;
            cin >> input;

            if (!(std::cin) || (!(std::isalpha(input)))) {
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
            }

            switch (input) {

                case 'h':
                {
                    Card playersCard = deck.dealCard();
                    playerScore += playersCard.returnCardValue(playersCard.rank);
                    cout << "You were dealt " << playersCard << ". ";
                    cout << "You now have " << playerScore << '\n';
                    break;
                }
                case 's':
                    endTurn = true;
                    break;
                default:
                    cout << "Invalid option. Try again.\n";
                    break;

            }

            if (playerScore > Settings::bust) {
                Settings::playerBust();
                break;
            }

            if (endTurn) {
                break;
            }

        }


        // Handle logic for dealers turn here
        while ((dealerScore <= playerScore || dealerScore == playerScore) && dealerScore < Settings::stopDealerDraw && dealerScore <= Settings::bust && playerScore <= Settings::bust) {
            Card dealersCard = deck.dealCard(); // Deal another card to dealer
            cout << "The dealer flips a " << dealersCard << ". ";
            // Add the card value to dealerScore
            dealerScore += dealersCard.returnCardValue(dealersCard.rank);
            // Print the updated dealerScore
            cout << "They now have: " << dealerScore << '\n';

        }


        // Win / Lose Logic --
        if (dealerScore > Settings::bust) {
            // Dealer busts
            Settings::dealerBust();
            return true;
        } else if (playerScore > Settings::bust) {
            // Player loses if they bust
            return false;
        }  else if (dealerScore > playerScore) {
            // Dealer wins
            return false;
        } else if (dealerScore == playerScore) {
            cout << "Hand was a push.\n";
            std::exit(0);
        }
        // Player wins if dealer did not win or bust
        return true;

    }

};

int main() {

    Deck deck {};

    deck.shuffle();

    Player player {};

    bool playerWins = player.drawCard(deck);

    cout << (playerWins ? "You win!" : "You Lose!");

}
