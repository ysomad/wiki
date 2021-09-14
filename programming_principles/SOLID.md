# SOLID - 5 принципов ООП

## Single responsibility principle (принцип единственной ответственности)
Класс **должен выполнять одну задачу**, потому что если у него будет 2 и более задач, он станет зависимым, и если захочется модифицировать (изменить, добавить новую и.т.д.) логику, то придется переписывать еe в двух местах

Как не надо делать:
```python
class Customer:
    def __init__(self, email, first_name, last_name):
        self.email = email
        self.first_name = first_name
        self.last_name = last_name
        self.cart = dict()

    def get_full_name(self):
        return "%s %s" % (self.first_name, self.last_name)

    def set_email(self, new_email):
        self.email = new_email

    def save_to_database(self):
        pass

    def add_item_to_cart(self, item_id, item_data):
        self.cart[item_id] = item_data
```
Класс Customer выполняет 3 задачи: работа с данными кастомера, сохранение данных кастомера в БД, работа с корзиной товаров, что противоречит этому принципу

Как надо:
```python
class Customer:
    def __init__(self, email, first_name, last_name):
        self.email = email
        self.first_name = first_name
        self.last_name = last_name

    def get_full_name(self):
        return "%s %s" % (self.first_name, self.last_name)

class CustomerRepository:
    def get_customer(self, customer_id):
        pass

class Cart:
    def add_item_to_cart(self, item_id, item_data):
        self.items[item_id] = item_data

    def remove_item_from_cart(self, item_id):
        del self.items[item_id]
```
Также можно воспользоваться паттерном проектирования [Фасад](https://refactoring.guru/ru/design-patterns/facade)

## Open-closed principle (принцип открытости-закрытости)
Программные сущности (классы, модули, функции) **должны быть открыты для расширения, но не модификации**.

Как делать не надо:
```python
class Discount:
    def __init__(self, customer, price):
        self.customer = customer
        self.price = price

    def get_customer_discount(self):
        if self.customer == 'fav':
            return self.price * 0.2
        if self.customer == 'vip':
            return self.price * 0.4
```
При добавлении новых скидок, придется каждый раз добавлять новую логику в метод `give_customer_discount()`, что противоречит OCP.

Как надо:
```python
class Discount:
    def __init__(self, customer, price):
        self.customer = customer
        self.price = price

    def get_discount(self):
        """По умолчанию скидка составляет 20%"""
        return self.price * 0.2

class DiscountVIP(Discount):
    def get_discount(self):
        """Скидка 40%"""
        return super().get_discount() * 2

class DiscountSuperVIP(DiscountVIP):
    def get_discount(self):
        """Скидка 60%"""
        return super().get_discount() * 2
```

## Liskov substitution principle (принцип подстановки Лисков) - пример полиморфизма
Для любого класса клиент должен иметь возможность **использовать любой подкласс базового класса, не замечая разницы между ними**, и следовательно, без каких-либо изменений поведения программы при выполнении.

Как не надо делать:
```python
class Bird:
    def fly(self):
        pass

class Duck(Bird):
    pass

class Ostrich(Bird):
    pass
```
Утка(Duck) умеет летать, потому что она птица, но страус(Ostrich) также является птицей но летать не умеет.

Как стоит делать:
```python
class Bird:
    pass

class FlyingBird(Bird):
    def fly(self):
        pass

class Duck(FlyingBird):
    pass

class Ostrich(Bird):
    pass
```

## Interface segregation principle (принцип разделения интерфейсов)
1. Нужно создавать интерфейсы, ориентированные на клиента
2. Клиенты не должны зависеть от интерфейсов, которые они не используют (разбивать интерфейсы на более мелкие)

Как не стоит делать:
```python
class Worker:
    def work(self):
        raise NotImplementedError("child must implement .work()")

    def sleep(self):
        raise NotImplementedError("child must implement .sleep()")

class HumanWorker(Worker):
    def work(self):
        print('works')

    def sleep(self):
        print('sleep')

class RobotWorker(Worker):
    def work(self):
        print('work')

    def sleep(self):
        """Robots do not need sleep"""
        pass
```

Как надо делать:
```python
class WorkAbleInterface:
    def work(self):
        raise NotImplementedError("child must implement .work()")

class SleepAbleInterface:
    def sleep(self):
        raise NotImplementedError("child must implement .sleep()")

class HumanWorker(WorkAbleInterface, SleepAbleInterface):
    def work(self):
        print('human working')

    def sleep(self):
        print('human sleeping')

class RobotWorker(WorkAbleInterface):
    def work(self):
        print('robot working')
```

## Dependency inversion principle (принцип инверсии зависимостей)
1. Модули верхних уровней не должны зависеть от модулей нижних уровней
2. Оба типа модулей должны зависеть от абстракций
3. Абстракции не должны зависеть от деталей
4. Детали должны зависеть от абстракций

Пример имплементации DIP на примере приготовления еды:
```python
class FoodInterface:
    def cook(self):
        raise NotImplementedError("child must implement .cook()")

    def eat(self):
        raise NotImplementedError("child must implement .cook()")

def Bread(FoodInterface):
    def cook(self):
        print('bread is cooked')

    def eat(self):
        print('bread was eaten')

def Pastry(FoodInterface):
    def cook(self):
        print('pastry is cooked')

    def eat(self):
        print('pastry was eaten')

class Production:
    def __init__(self, food: FoodInterface):
        self.food = food

    def produce(self):
        self.food.cook()

    def consume(self):
        self.food.eat()
```