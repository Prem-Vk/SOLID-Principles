# SOLID: The First 5 Principles of Object Oriented Design


## **SOLID** is acronym for the first five object-oriented design (ODD) principles by *`Robert C. Martin`*.

## `SOLID` principles are used for:
* These principles establish practices that lend to developing software with considerations for maintaining and extending as the project grows. 
* Adopting these practices can also contribute to avoiding code smells, refactoring code, and Agile or Adaptive software development.

In general, the SOLID principles are basic learning steps for every code developer but are usually ignored by those who does not consider the highest quality of code their absolute priority.

### `SOLID` stands for:
* **S** - Single-responsibilty Princple
* **O** - Open-closed Principle
* **L** - Liskov Substitution Principle
* **I** - Interface Segregation Principle
* **D** - Dependency Inversion Principle

#### 1. Single-responsibilty Principle
---
> "A class should have one, and only one, reason to chnage"

Every component of your code(class or function) should have one and only one resposibility.
So that there would be, one and only reason to change it.

Let's understand this by an example:
Too often you see a piece of code that takes care of an entire process all at once. I.e., A function that loads data, modifies and, plots them, all before returning its result.

Let’s take a simpler example, where we have a list of number L = [n1, n2, …, nx] and we compute maximum and minimum number from it.

A bad approach would be to have a single function doing all the work:
```
from random import randrange


def max_min(list1):
    maximum = max(list1)
    minimum = min(list1)

    print("Maximum number is :", maximum)
    print("Minimum number is :", minimum)


l = [78,45,12,98,63,2,52,36]

max_min(l)
```
The make above code more SRP compliant, we have split the max_min function into atomic or mini functions. So that every function perform only one task or process.

A good approach will be:
```
def maximum(list1):
    print("Maximum number is :", max(list1))


def minimum(list1):
    print("Minimum number is :", min(list1))


def main(list1):
    maximum(list1)
    minimum(list1)


list1 = [78, 45, 12, 98, 63, 2, 52, 36]

main(list1)
```
As per the above code, every function is performing only one task and you have now only one single reason to change each function connected with "main".

The result of this simple action is that now:
1. It is easier to localize errors. Any error in execution will point out to a smaller section of your code, accelerating your debug phase.
2. Any part of the code is reusable in other section of your code.
3. Moreover and, often overlooked, is that it is easier to create testing for each function of your code. Side note on testing: You should write tests before you actually write the script. But, this is often ignored in favour of creating some nice result to be shown to the stakeholders instead.
   



#### 2. The Open-closed principle(OCP)
---
> “Software entities … should be open for extension but closed for modification”

In genral, you should not need to modify the code you have already written to accommodate new functionality, but simply add what you now need.

Consider a shop as an example in which as VIP customers get a discount:
```
class Discount:
   """Demo customer discount class"""
   def __init__(self, customer, price):
       self.customer = customer
       self.price = price
   def give_discount(self):
       """A discount method"""
       if self.customer == 'normal':
           return self.price * 0.2
       elif self.customer == 'vip':
           return self.price * 0.4
```
As per the above code you can see that normal, VIP customers get 0.2% and 0.4% discount.
Assume, we have a super VIP customer and we want to give a discount of 0.8%. Maybe we will solve the problem this way.

```
    def give_discount(self):
       """A discount method"""
       if self.customer == 'normal':
           return self.price * 0.2
       elif self.customer == 'vip':
           return self.price * 0.4
       elif self.customer ==  'supvip':
           return self.price * 0.8
```

But this solution violates the OCP. Because we can’t modify the give_discount method. Only we can extend the method.

The right solution will be:
```
class Discount:
   """Demo customer discount class"""
   def __init__(self, customer, price):
       self.customer = customer
       self.price = price
   def get_discount(self):
       """A discount method"""
       return self.price * 0.2
class VIPDiscount(Discount):
   """Demo VIP customer discount class"""
   def get_discount(self):
       """A discount method"""
       return super().get_discount() * 2
class SuperVIPDiscount(VIPDiscount):
   """Demo super vip customer discount class"""
   def get_discount(self):
       """A discount method"""
       return super().get_discount() * 2
```

As per the above code, you can see we have created a new class for every type of code. This will help us to extend the code without modifying previous code.
Above code use multiple inheritance to get the discounted value and, as per the customer type the discount will be provided.

#### 3. Liskov Substitution Principle
---

>Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it

* In simpler words, if a subclass redefines a function also present in the parent class, a client-user should not be noticing any difference in behaviour, and it is a substitute for the base class.
* For example, if you are using a function and your colleague change the base class, you should not notice any difference in the function that you are using.

Let's understand using below example:

Violation of LSP:
```
class Vehicle:
   """A demo Vehicle class"""

   def __init__(self, name: str, speed: float):
       self.name = name
       self.speed = speed

   def get_name(self) -> str:
       """Get vehicle name"""
       return f"The vehicle name {self.name}"

   def get_speed(self) -> str:
       """Get vehicle speed"""
       return f"The vehicle speed {self.speed}"

   def engine(self):
       """A vehicle engine"""
       pass

   def start_engine(self):
       """A vehicle engine start"""
       self.engine()


class Car(Vehicle):
   """A demo Car Vehicle class"""
   def start_engine(self):
       pass


class Bicycle(Vehicle):
   """A demo Bicycle Vehicle class"""
   def start_engine(self):
       pass
```

Bicycle class violates the LSP. Cause in the Vehicle class has an engine method. But naturally, a bicycle has no engine. So we could not start any engine.

The better way to use LSP priciple:

```
class Vehicle:
   """A demo Vehicle class"""
   def __init__(self, name: str, speed: float):
       self.name = name
       self.speed = speed

   def get_name(self) -> str:
       """Get vehicle name"""
       return f"The vehicle name {self.name}"

   def get_speed(self) -> str:
       """Get vehicle speed"""
       return f"The vehicle speed {self.speed}"


class VehicleWithoutEngine(Vehicle):
   """A demo Vehicle without engine class"""
   def start_moving(self):
      """Moving"""
      raise NotImplemented


class VehicleWithEngine(Vehicle):
   """A demo Vehicle engine class"""
   def engine(self):
      """A vehicle engine"""
      pass

   def start_engine(self):
      """A vehicle engine start"""
      self.engine()


class Car(VehicleWithEngine):
   """A demo Car Vehicle class"""
   def start_engine(self):
       pass


class Bicycle(VehicleWithoutEngine):
   """A demo Bicycle Vehicle class""" 
   def start_moving(self):
       pass
```
If in a subclass, you redefine a function that is also present in the base class, the two functions ought to have the same behaviour. This, though, does not mean that they must be mandatorily equal, but that the user, should expect that the same type of result, given the same input.

##### **Note**
Actually, LSP is a concept that applies to all kinds of polymorphism. Only if you don’t use polymorphism of all you don’t need to care about the LSP.

#### The Interface Segregation Principle (ISP)
---
> Many client-specific interfaces are better than one general-purpose interface

* In the contest of classes, an interface is considered, all the methods and properties “exposed”, thus, everything that a user can interact with that belongs to that class.
* In this sense, the IS principles tell us that a class should only have the interface needed (SRP) and avoid methods that won’t work or that have no reason to be part of that class.
* This problem arises, primarily, when, a subclass inherits methods from a base class that it does not need.

Let's see an example:
```
import numpy as np
from abc import ABC, abstractmethod

class Mammals(ABC):
    @abstractmethod
    def swim() -> bool:
        print("Can Swim") 

    @abstractmethod
    def walk() -> bool:
        print("Can Walk") 

class Human(Mammals):
    def swim():
        return print("Humans can swim") 

    def walk():
        return print("Humans can walk") 

class Whale(Mammals):
    def swim():
        return print("Whales can swim")
```

And indeed, if we run this code we could have:
```
Human.swim()
Human.walk()

Whale.swim()
Whale.walk()

# Humans can swim
# Humans can walk
# Whales can swim
# Can Walk
```

As per the output, we can see that walk function will also run for whale class. And as we all know, whales can't walk.

The sub-class whale can still invoke the method “walk” but it shouldn’t, and we must avoid it.
The way suggested by ISP is to create more `client-specific interfaces` rather than one `general-purpose interface`. 
So, our code example becomes:

```
from abc import ABC, abstractmethod

class Walker(ABC):
  @abstractmethod
  def walk() -> bool:
    return print("Can Walk") 

class Swimmer(ABC):
  @abstractmethod
  def swim() -> bool:
    return print("Can Swim") 

class Human(Walker, Swimmer):
  def walk():
    return print("Humans can walk") 
  def swim():
    return print("Humans can swim") 

class Whale(Swimmer):
  def swim():
    return print("Whales can swim") 

if __name__ == "__main__":
  Human.walk()
  Human.swim()

  Whale.swim()
  Whale.walk()

# Humans can walk
# Humans can swim
# Whales can swim
```

Now, every sub-class inherits only what it needs, avoiding invoking an out-of-context (wrong) sub-method. That might create an error hard to catch.
This has the final aim to keep our classes clean and minimise mistakes.

#### The Dependency Inversion Principle (DIP)
---
> Abstractions should not depend on details. Details should depend on abstraction. High-level modules should not depend on low-level modules. Both should depend on abstractions.

Next on our list is Liskov substitution, which is arguably the most complex of the five principles. Simply put, if class A is a subtype of class B, we should be able to replace B with A without disrupting the behavior of our program.

To better explain this concept, Imagine that you have a program that takes in input a specific set of info (a file, a format, etc) and you wrote a script to process it. What would happen if that info were subject to changes? You would have to rewrite your script and adjust the new format. Losing the retro compatibility with the older files.
However, you could solve this by creating a third abstraction that takes the info as input and passes it to the others.

Let's understand this using below example.

```
class NewsPerson:
    """This is a high-level module"""
    @staticmethod
    def publish(news: str) -> None:
        """
        :param news:
        :return:
        """
        print(NewsPaper().publish(news=news))
class NewsPaper:
    """This is a low-level module"""
    @staticmethod
    def publish(news: str) -> None:
        """
        :param news:
        :return:
        """
        print(f"{news} Hello newspaper")
person = NewsPerson()
print(person.publish("News Paper"))
```

In this example, we found that the high-level module depends on the low-level module. Hence this example are violated the Dependency Inversion Principle. Let’s solve the problem according to the definition of DIP.

```
class NewsPerson:
   """This is a high-level module"""
   @staticmethod
   def publish(news: str, publisher=None) -> None:
       print(publisher.publish(news=news))
class NewsPaper:
   """This is a low-level module"""
   @staticmethod
   def publish(news: str) -> None:
       print("{} news paper".format(news))
class Facebook:
   """This is a low-level module"""
   @staticmethod
   def publish(news: str) -> None:
       print(f"{news} - share this post on {news}")
person = NewsPerson()
person.publish("hello", NewsPaper())
person.publish("facebook", Facebook())
```

### **I hope the purpose of this article is clear and you now know when and how to use it.**

**Please use below links for better understanding**
* [Solid principle using java](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
* [Solid principle using python](https://medium.com/@vubon.roy/solid-principles-with-python-examples-10e1f3d91259)
* [Java examples using solid principle](https://www.baeldung.com/solid-principles)
* [Python examples using solid principle](https://gist.github.com/dmmeteo/f630fa04c7a79d3c132b9e9e5d037bfd)