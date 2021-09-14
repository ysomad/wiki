# Алгоритмы и структуры данных

# Стеки и очереди

## Стек (LIFO)
Стек - это коллекция, элементы которой получают по принципу "последний вошел, первый вышел" (Last-In-First-Out или `LIFO`).

В отличие от списков, получить доступ к произвольному элементу стека невозможно. (нет метода `Contains` и `итератора`).

### Пример из реального мира
Стопка тарелок. Вне зависимости от количества тарелок, первой можно взять только верхнюю тарелку.

Последний добавленный элемент стека называется `верхушкой стека`, или `top`.

### Методы стека
- push - добавление элемента в стек
- pop - удаление
- peek - просмотр последнего элемента

### Реализация на Python
1. Использование базовой структуры `list`
```python
stack = []
stack.append('list') # добавление элементов
stack.pop() # удаление элементов
```
2. Использование `collection.deque`

deque переводится как "колода", или "двусторонняя очередь". Можно использовать те же методы, что использовались в примере выше: `append()` и `pop()`.
```python
from collections import deque
stack = deque()
stack.append('deque')
stack.pop()
```

## Очередь (FIFO)
Очереди похожи на стеки, они также не дают доступа к произвольному элементу, но элементы кладутся(`enqueue`) и забираются(`dequeue`) с разных концов. Такой метод называется "первый вошел, первый вышел" (First-In-First-Out или FIFO).

Очереди часто используются для реализации буфера, в который можно положить элемент для последующей обработки, сохраняя порядок поступления. Например, если БД поддерживает только одно соединение, можно использовать очередь потоков, которые будут ждать своей очереди на доступ к БД.

### Реализация на Python
```python
class Queue:
    def __init__(self):
      self.items = []

    def is_empty(self):
      return self.items == []

    def enqueue(self, item):
      self.items.insert(0, item)

    def dequeue(self):
      return self.items.pop()

    def size(self):
      return len(self.items)
```