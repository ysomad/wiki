# Принципы ООП
## Наследование (дочерний класс является родительским классом)
Наследование - механизм, предоставляющий возможность написать класс на основе другого класса, заимствуя поля и методы родительского класса.

Преимущества:
* Переиспользование кода
* Упрощение классов
* Уменьшение кол-ва кода

</br>

### ***Ассоциация*** (дочерний класс имеет родительский класс)
Ассоциация - это включение одного класса в другой в качестве одного из полей.
Выделяют два частных случая ассоциации: композицию и агрегацию.

</br>

#### ***Композиция***
Композиция - это создание экземпляра класса в конструкторе родительского класса.
```python
class Engine:
    def __init__(self, power: int):
        self.power = power

class Car:
    def __init__(self, model: str):
        self.model = model
        self.engine = Engine(360)
```

</br>

#### ***Агрегация***
Агрегация - это создание экземпляра класса где-то в другом месте кода и передача его в конструктор класса в качестве параметра.
```python
class Engine:
    def __init__(self, power: int):
        self.power = power

class Car:
    def __init__(self, model: str, engine: Engine):
        self.model = model
        self.engine = engine

engine = Engine(360)
car = Car('Priora', engine)
```

</br>

## Абстракция
Абстракция - это выделение главных (наиболее значимых) характеристик объекта и наоборот - отбрасывает второстепенных, незначительных.

### Пример абстракции
Нужно создать картотеку работников компании (класс `Employee`) с полями: ФИО, дата рождения, номер социального страхования, ИНН. Но вряд ли в карточке такого типа нужны его рост, цвет глаз и волос. Компании эта информация о сотруднике ни к чему.

Поэтому для класса `Employee` нужно задать поля `name`, `age`, `social_insurance_number` и `tax_number`, а от лишней информации вроде цвета глаз лучше отказаться, абстрагироваться.

</br>

## Инкапсуляция
Выделяют две формулировки инкапсуляции, работающих вместе:
* Объединение данных и методов в одном объекте
* Защита данных и методов класса от внешних воздействий, изменение состояний объекта происходит с помощью методов данного объекта

</br>

## Полиморфизм
Полиморфизм - это использование единственной сущности для представления различных типов в различных сценариях использования.

</br>

### ***Полиморфизм оператора сложения***
Оператор `+` часто используется в Python, но не имеет ***единственного*** использования:
1. Для `int` используется чтобы сложить операнды
```python
a, b = 1, 2
print(a + b) # 3
```
2. Для `str` используется для конкатенации
```python
first_name, last_name = 'Alex', 'Malykh'
print(first_name + ' ' + last_name) # Alex Malykh
```
Из примеров видно, что единственный оператор `+` выполняет разные операции для различных типов данных.

</br>

### ***Полиморфизм функций***
В Python есть функции, которые могут принимать параметры разных типов, например, `len()`
```python
print(len('string')) # 6
print(len(['list_item1', 'list_item2', 'list_item3'])) # 3
print(len({'key1': 'val1', 'key2': 'val2'})) # 2
```
Из примера видно, что в функцию `len()` передаются параметры разных типов и возвращает специфичную информацию для каждого типа.

</br>

### ***Полиморфизм в методах класса***
```python
class Cat:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def info(self):
        print(f"I am a cat. My name is {self.name}. I am {self.age} years old.")

    def make_sound(self):
        print("Meow")

class Dog:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def info(self):
        print(f"I am a dog. My name is {self.name}. I am {self.age} years old.")

    def make_sound(self):
        print("Bark")

cat1 = Cat("Kitty", 2.5)
dog1 = Dog("Fluffy", 4)

for animal in (cat1, dog1):
    animal.make_sound()
    animal.info()
    animal.make_sound()
```
Вывод:
```
Meow
I am a cat. My name is Kitty. I am 2.5 years old.
Meow
Bark
I am a dog. My name is Fluffy. I am 4 years old.
Bark
```
Здесь было создано 2 класса `Cat` и `Dog`. У них похожая структура и они имеют методы с одними и теми же именами `info()` и `make_sound()`.

> В примере не было создано базового класса (родителя) и классы независимы друг от друга. Если есть возможность упаковать два разных класса в единую структуру данных (в примере используется `кортеж`, или `tuple`) и итерировать по ней, используя общую переменную (в примере `animal`) - это и будет являться полиморфизмом.

</br>

### ***Полиморфизм и наследование*** (переопределение методов)
Благодаря наследованию, дочерние классы могут наследовать методы и поля родительского класса. Есть возможность перезаписать некоторые методы и поля, чтобы они соответствовали дочернему классу, и это поведение называется `переопределением метода` (`method overriding`)

Полиморфизм позволяет получать доступ к переопределенным методам и атрибутам, которые имеют то же самое имя, что и в родительском классе.
```python
from math import pi

class Shape:
    def __init__(self, name: str):
        self.name = name

    def area(self):
        raise NotImplementedError("Child class should override `.area()`")

    def fact(self):
        return "I'm two-dimensional shape."

    def __str__(self):
        return self.name

class Square(Shape):
    def __init__(self, length: int):
        super().__init__("Square")
        self.length = length

    def area(self):
        return self.length**2

    def fact(self):
        return "Squares have each angle equal to 90 degrees.")

class Circle(Shape):
    def __init__(self, radius: int):
        super().__init__("Circle")
        self.radius = radius

    def area(self):
        return pi * self.radius**2

square = Square(4)
circle = Circle(7)
print(circle)
print(circle.fact())
print(square.fact())
print(circle.fact())
```
Вывод:
```
Circle
I am a two-dimensional shape.
Squares have each angle equal to 90 degrees.
153.93804002589985
```
Метод `__str__()` не был переопределен в дочерних классах, он используется из родительского класса.

***Если метод в дочернем классе не переопределен, то используется метод из родительского***.

Также см. [[SOLID]].




