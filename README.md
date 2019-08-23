```python
rom abc import abstractmethod, ABC


class Pizza(ABC):

   @abstractmethod
   def prepare(self):
       pass

   @staticmethod
   def bake():
       print("baking pizza for 12min in 400 degrees..")

   @staticmethod
   def cut():
       print("cutting pizza in pieces")

   @staticmethod
   def box():
       print("putting pizza in box")

class CheesePizza(Pizza):
   def prepare(self):
       print("preparing a cheese pizza..")


class GreekPizza(Pizza):
   def prepare(self):
       print("preparing a greek pizza..")

def order_pizza(pizzaType: str):
   # orderPizza needs to be aware of the pizza creation task.
   # this is violating the single responsibility principle
   # since our code is open for modification

   if pizzaType == 'Greek':
       pizza = GreekPizza()
   elif pizzaType == 'Cheese':
       pizza = CheesePizza()
   else:
       raise ValueError("No matching pizza found...")

   pizza.prepare()
   pizza.bake()
   pizza.cut()
   pizza.box()


print("##### [1] ########")
order_pizza('Greek')
print("##################\n")

# We can instead create a Simple Factory that will
# exclusively deal with the pizza creation process.
class SimplePizzaFactory:

   @staticmethod
   def create_pizza(pizzaType: str) -> Pizza:
       pizza: Pizza

       if pizzaType == 'Greek':
           pizza = GreekPizza()
       elif pizzaType == 'Cheese':
           pizza = CheesePizza()
       else:
           raise ValueError("No matching pizza found...")

       return pizza


class PizzaStore:
   factory: SimplePizzaFactory

   def __init__(self, factory: SimplePizzaFactory):
       self.factory = factory

   def order_pizza(self, pizzaType: str):
       # orderPizza only needs to be aware of preparing the pizza.
       # we have encompassed everything
       pizza: Pizza
       pizza = self.factory.create_pizza(pizzaType)

       pizza.prepare()
       pizza.bake()
       pizza.cut()
       pizza.box()


#################################################
store = PizzaStore(SimplePizzaFactory())

print("##### [2] ########")
store.order_pizza('Greek')
print("##################\n")
```
