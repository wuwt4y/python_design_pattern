# Python Design Pattern
- ### Creational Pattern(生成模式)
	- Simple Factory
	- Factory Method
	- Abstrac Factory
	- Builder
	- Prototype
	- Singleton
- ### Structual Patterns(結構模式)
	- Adapter
	- Bridge
	- Composite
	- Decorator
	- Facade
	- Flyweight
	- Proxy
- ### Behavior Patterns(行為模式)
	- Chain of Responsibility
	- Command
	- Interpreter
	- Iterator
	- Mediator
	- Memento
	- Observer
	- State
	- Strategy
	- Template Method
	- Visitor

[========]
## Simple Factory
他不是一個真正的設計模式
但提供一個簡單的方法將我們系統與具體類分離

優點：
分工明確，各司其職。
用戶端不再創建物件，而是把創建物件的職責交給了具體的工廠去創建。
用戶端只負責調用。

缺點：
工廠的靜態方法無法被繼承。
代碼維護不易 ( 物件要是很多的話，工廠是一個很龐大的類別 ) 。
這種模式對OCP原則(Open-Close Principle)支援的不夠，如果有新的產品加入到系統中就要修改工廠類。

使用時機：
用戶端需要創建物件，物件有同樣製作流程，但實作內容不同，並且數量不多的情況下，可以考慮使用簡單工廠模式。

### Simple Factory UML
UML

### Simple Factory Example

```python
from abc import abstractmethod, ABC


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

    Result:

    ##### [1] ########
    preparing a greek pizza..
    baking pizza for 12min in 400 degrees..
    cutting pizza in pieces
    putting pizza in box
    ##################

    ##### [2] ########
    preparing a greek pizza..
    baking pizza for 12min in 400 degrees..
    cutting pizza in pieces
    putting pizza in box
    ##################


[========]

## Factory Method
