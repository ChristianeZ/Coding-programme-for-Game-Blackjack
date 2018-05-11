# Coding-programme-for-Game-Blackjack
echo "# Coding-programme-for-Game-Blackjack" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/ChristianeZ/Coding-programme-for-Game-Blackjack.git
git push -u origin master
"""
Blackjack, also known as twenty-one, is a comparing card game. 
It can be played with one deck of 52 cards. The objective of the game is to reach a final score higher than the dealer 
without exceeding 21; or Let the dealer draw additional cards until their hand exceeds 21. 
We are going to implement this game using Python. 
"""


class Queue:
  def __init__(self):
    self.items = []
  def is_empty(self):
    return self.items == []
  def size(self):
    return len(self.items)
  def enqueue(self, item):
    self.items.insert(0, item)
  def dequeue(self):
    try:
      result = self.items.pop()
    except IndexError:
      result = 'The queue is empty.'
    return result

"""
The purpose of the Queue is able to achieve the similar functionality as a
general list with less complexity by applying the rule "First in first out".
Items in queue can only be accessed in the begining and in the end. Items will be
added (enqueue) in the end and will be access/delete(dequeue) from the beginning.
"""

class Stack:
  def __init__(self):
      self.items = []
  def is_empty(self):
      return (self.items == [])
  def size(self):
      return len(self.items)
  def push(self, item):
      self.items.append(item)
  def pop(self):
      try:
        result = self.items.pop()
      except IndexError:
        result = 'Pop Error: The stack is empty!'
      return result

"""
Stack is a class designed by user which allow the similar functionality as a list
with less complexity. The stack( an example of ADT) can only be access in the beginning
and the end. The stack is implying the rule "Last in First out". Both push and pop
will only happen at the end of the list, so is O(1).
"""
	
class PokerCard:
  def __init__(self, item):
      self.cards = item
      self.value = self.__evaluate(item)
  def __evaluate(self,item):
    value = item[0]
    number = [str(x) for x in range(10)]
    value_10 = ["K","Q","J","T"]
    if value in number:
      value = int(value)
    else:
      if value == "A":
        value = 11 #1 or 11 need to check out
      elif value in value_10:
        value = 10
    return str(value)
  
  def __str__(self):
    return str(self.cards)
        
"""
The PokerCard class allow cards to be evaluated based on the first character of the card
__str__ allowed the object to be printed in understandable form. __init__ is the
constructor of the class. 

"""
class Deck:
  def __init__(self):
    self.cards = Stack()
    cardlist = ['AS', 'AH', 'AC', 'AD', '2S', '2H', '2C', '2D', '3S','3H', '3C', '3D', '4S', '4H', '4C', '4D', '5S', '5H', '5C', '5D', '6S', '6H', '6C', '6D', '7S', '7H', '7C', '7D', '8S', '8H', '8C', '8D', '9S', '9H', '9C', '9D', 'TS', 'TH', 'TC', 'TD', 'JS', 'JH', 'JC', 'JD', 'QS', 'QH', 'QC', 'QD', 'KS', 'KH', 'KC', 'KD']
    for i in range(52): 
        self.cards.push(cardlist[i])
  def size(self):
      return self.cards.size()
  def pop(self):
      value = self.cards.pop()
      return value
    
  def __str__(self):
    
    row = 4
    string1 = ""
    start_num = 0 
    list1 = []

       
    while self.cards.size() != 0:
      value = self.cards.pop()
      list1.insert(0,value)
   

    while row > 1:
      for index in range(start_num,start_num + 13):
        string1 += list1[index] + "  "
        string1 = string1[:-1]
      string1 += "\n"
      start_num += 13
      row -=1
    for index in range(start_num, len(list1)):
      string1 += list1[index] + "  "
      string1 = string1[:-1]

    return string1

 """
 There are three types of shuffle functions provided below. All are very similar but using either Queue and Stack. As mentioned above, 
 Queue and Stack are having different initial characterstics, which can help us to produce sequences with different order if we use both together. 
 """
  def shuffleONE(self,no):
    
    s1 = Stack()
    s2 = Stack()
    no_copy = no
    
    while no_copy > 0:
      value = self.cards.pop()                 
      s1.push(value)
      no_copy -= 1


    while self.cards.size() > 0:
      value = self.cards.pop()
      s2.push(value)

                
    common_value = min(s1.size(),s2.size())

    while common_value > 0:
      value1 = s1.pop()
      value2 = s2.pop()
      self.cards.push(value1)
      self.cards.push(value2)
      common_value -=1

    if s1.size() != s2.size():
      if s1.size() > s2.size():
        while s1.size() > 0:
          value = s1.pop()
          self.cards.push(value)
      else:
        while s2.size() > 0:
          value = s2.pop()
          self.cards.push(value) 
    
    return self.cards

  def shuffleTHREE(self,no):
    q1 = Queue()
    q2 = Queue()
    
    while no >0:
      value = self.cards.pop()
      q1.enqueue(value)
      no-=1
    while self.cards.size() > 0:
      value = self.cards.pop()
      q2.enqueue(value)

    common = min(q1.size(),q2.size())
    while common > 0:
      value1 = q1.dequeue()
      value2 = q2.dequeue()
      self.cards.push(value1)
      self.cards.push(value2)
      common -=1
    if q1.size() != q2.size():
      if q1.size() > q2.size():
        while q1.size() > 0:
          value = q1.dequeue()
          self.cards.push(value)
      else:
        while q2.size() >0:
          value = q2.dequeue()
          self.cards.push(value)
    
    return self.cards

  def shuffleTWO(self,no):
    q1 = Queue()
    s2 = Stack()
    
    while no >0:
      value = self.cards.pop()
      q1.enqueue(value)
      no-=1
    while self.cards.size() > 0:
      value = self.cards.pop()
      s2.push(value)

    common = min(q1.size(),s2.size())
    while common > 0:
      value1 = q1.dequeue()
      value2 = s2.pop()
      self.cards.push(value1)
      self.cards.push(value2)
      common -=1
    if q1.size() != s2.size():
      if q1.size() > s2.size():
        while q1.size() > 0:
          value = q1.dequeue()
          self.cards.push(value)
      else:
        while s2.size() >0:
          value = s2.pop()
          self.cards.push(value)

    return self.cards    
      
"""
This class have shuffle1,2,3. We shuffle the order of the item by using Stack &
Queue. As they are applying different rules, we are able to reverse the order.
Stack- last in first out
Queue- first in last out
"""

class Player:
  def __init__(self):
    self.cards = Stack()
  def points_eval(self):
    value = 0
    number = [str(x) for x in range(11)]
    value_10 = ["K","Q","J", "T"]

    s_backup = Stack()
    ases = 0
    while self.cards.size() > 0:
      item = self.cards.pop()
      s_backup.push(item)
      value1 = item[0]
      if value1 in number:
        value1 = int(value1)
      else:
        if value1 == "A":
          ases +=1
          value1 = 0
        elif value1 in value_10:
          value1 = 10
      value += value1
    if ases > 0:
      test1 = value + 11 + (ases -1)
      test2 = value + ases
      diff1 = 21-test1
      diff2 = 21-test2
      if diff1 >= 0 and diff2 >= 0:
        if diff1 < diff2:
          value = test1
        else:
          value = test2
      else:
        value = test2
    while s_backup.size() > 0:
      self.cards.push(s_backup.pop())
    return str(value)
      
  def is_empty(self):
      return (self.items == [])
  def size(self):
      return len(self.items)
  def push(self, item):
      self.items.append(item)
  def pop(self):
      try:
        result = self.items.pop()
      except IndexError:
        result = 'Pop Error: The stack is empty!'
      return result
    
  def __str__(self):
    list1 = []
    for i in range(self.cards.size()):
      item = self.cards.items[i]
      list1.append(item)
      
    return str(list1)
    
"""
This is a complex version of the game. "A" can be acted as both 1 or 11, depends on the
situation of the player. Except "Point_eval", other function is basically a stack
class's functions. The value 11 can only be used for once no matter how many "A"s
the player has. Therefore, we will can tried to analysis two options. Option 1 is
where we got one A = 11 and the rest of the A's are acting as 1; option 2 is all
the A's are counted as 1. We will compared which options of the two are closer to 21
but not greater than 21. 
"""

endgame = False
Gdeck = Deck()
Gdeck.shuffleTHREE(12)
P1 = Player()
P1.cards.push(Gdeck.cards.pop())
P1.cards.push(Gdeck.cards.pop())
print(P1)
print(P1.points_eval())

while (endgame == False):
    answer = str(input("Do you want one more card? (y/n)"))
    if answer == "n":
        endgame = True
        
    elif answer == "y":
        P1.cards.push(Gdeck.cards.pop())
        print(P1)
        score = int(P1.points_eval())
        print(score)
        if score > 21:
            endgame = True
            print("GAME OVER!")

"""
The player can input y or n to choose whether they will continues to play in the game
If they do, the programme will pop a card and push into the player's stack and we will
evaluate their total score each time. If it excess 21, the game is over. 
"""
