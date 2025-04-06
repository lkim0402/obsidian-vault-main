### My solution
```python
from random import shuffle

class Card:
    suit = ("Hearts", "Diamonds", "Clubs","Spades")
    value = ("A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K")

    def __init__(self, suit, value):
        if suit not in Card.suit:
            raise ValueError("Invalid suit!")
        self.suit = suit    
        if value not in Card.value:
            raise ValueError("Invalid Value!")
        self.value = value

    def __repr__(self):
        return f"{self.value} of {self.suit}"
    

class Deck:
    def __init__(self):
        # list of tuples (ex. ("Hearts","A"), ("Hearts",2)...("Spades","K"))
        self.cards = [Card(s,v) for v in Card.value for s in Card.suit]

    def __repr__(self):
        return f"Deck of {self.count()} cards"

    def count(self):
        return len(self.cards)

    # removes num from deck
    def _deal(self, num):
        count = self.count()
        if count == 0:
            raise ValueError("All cards have been dealt")
        
        if count >= num: #4 cards in deck and count = 2
            # reverse = self.cards[::-1]
            # new = reverse[:num]
            # self.cards = self.cards[:-num]
            # ========= a waaaay better solution =========
            return [self.cards.pop() for x in range(num)]
        else: # num > count, 2 cards in deck, and count = 4
            remaining = self.cards[::]
            print(f"num of self.cards: {self.cards}")
            self.cards.clear()
            return remaining
    
    def shuffle(self):
        if self.count() != 52:
            raise ValueError("Only full decks can be shuffled")
        return shuffle(self.cards)
    
    def deal_card(self):
        return self._deal(1)[0]
    
    def deal_hand(self, num):
        return self._deal(num)
```

#### Reworking on the deal method!!
```python
def _deal(self, num):
	count = self.count()
	if count == 0:
		raise ValueError("All cards have been dealt")
	
	if num > count:
		num = self.count()
	
	return [self.cards.pop() for x in range(num)]
```
