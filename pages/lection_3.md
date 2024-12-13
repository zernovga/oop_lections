---
theme: sirius-college
layout: cover
---

# Объектно-ориентированное программирование<br>Лекция 3

Объекты. Аттрибуты. Слоты. Цепочки методов. Абстрактные классы и методы.

---

# Наследование от `object`

````md magic-move
```python
class MyClass(object):
    def __init__(self):
        self.x = 10
        self.y = 20
```

```python
class MyClass:
    def __init__(self):
        self.x = 10
        self.y = 20
```
````

---

# Аттрибуты

````md magic-move
```python
class MyClass:
    def __init__(self):
        self.x = 10
        self.y = 20

obj = MyClass()
setattr(obj, 'z', 30)
print(f"{obj.z = }")
# obj.z = 30
```

```python
class MyClass:
    def __init__(self):
        self.x = 10
        self.y = 20

obj = MyClass()
setattr(obj, 'z', 30)
obj.d = 40
print(f"{obj.d = }")
# obj.d = 40
```
````

---

# Аттрибуты. Слоты

````md magic-move
```python
class MyClass:
    __slots__ = ('x', 'y')

    def __init__(self):
        self.x = 10
        self.y = 20

obj = MyClass()
setattr(obj, 'z', 30)
#   setattr(obj, 'z', 30)
# AttributeError: 'MyClass' object has no attribute 'z'
```

```python
class MyClass:
    __slots__ = ('x', 'y')

    def __init__(self):
        self.x = 10
        self.y = 20

obj = MyClass()
obj.d = 40
#   obj.d = 40
#   ^^^^^
# AttributeError: 'MyClass' object has no attribute 'd'
```

```python
class MyClass:
    __slots__ = ('x',)

    def __init__(self):
        self.x = 10
        self.y = 20

obj = MyClass()
#   self.y = 40
#   ^^^^^^
# AttributeError: 'MyClass' object has no attribute 'y'
```
````

---

# Аттрибуты. Слоты

````md magic-move
```python
from sys import getsizeof

class ExampleSlots:
    __slots__ = ('x', 'y')

class ExampleNoSlots:
    pass

slots = [ExampleSlots() for _ in range(100)]
no_slots = [ExampleNoSlots() for _ in range(100)]

print(getsizeof(slots) + sum(getsizeof(obj) for obj in slots)) # 5720
print(getsizeof(no_slots) + sum(getsizeof(obj) for obj in no_slots)) # 6520
```
````

---

# Методы

Методы класса можно разделить на 3 группы:

1. Методы экземпляра класса (они же обычные методы)
1. Статические методы
1. Методы класса

---

# Методы экземпляра класса

- Доступны только после создания экземпляра класса, то есть чтобы вызвать такой метод надо обратиться к экземпляру.
- Первым аргументом метода является ссылка на экземпляр `self`.

```py
class MyClass:
    i = 12345

    def f(self):
        return 'hello world'

x = MyClass()
x.f()
# 'hello world'
```

---

# Методы класса

Метод класса вместо того, чтобы принимать аргумент `self`, принимает аргумент `cls`. При вызове метода этот аргумент указывает на сам класс, а не на экземпляр класса.

Поскольку метод класса имеет доступ только к аргументу `cls`, он не может изменять состояние экземпляра объекта. Для этого потребуется доступ к аргументу `self`. Но все же методы класса могут изменять состояние класса, которое применяется ко всем экземплярам класса.

Методы класса отмечаются декоратором `@classmethod`.

---

# Методы класса

````md magic-move
```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls
```

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls

obj = MyClass()
obj.method()
# ('instance method called', <__main__.MyClass object at 0x7f9748403f40>)
MyClass.method(obj)
# ('instance method called', <__main__.MyClass object at 0x7f9748403f40>)
```

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls

obj = MyClass()
obj.classmethod()
# ('class method called', <class '__main__.MyClass'>)
```

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls

obj = MyClass()
MyClass.classmethod()
# ('class method called', <class '__main__.MyClass'>)
MyClass.method()
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: method() missing 1 required positional argument: 'self'
```
````

---

# Применение методов класса

````md magic-move
```python
class Date:
    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    def string_to_db(self):
        return f'{self.year}-{self.month}-{self.day}'
```

```python
class Date:
    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    def string_to_db(self):
        return f'{self.year}-{self.month}-{self.day}'


string_date = '10.10.2020'
day, month, year = map(int, string_date.split('.'))
date = Date(day, month, year)
date.string_to_db()
# '2020-10-10'
```

```python
class Date:
    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    def string_to_db(self):
        return f'{self.year}-{self.month}-{self.day}'

    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('.'))
        date1 = cls(day, month, year)
        return date1
```

```python
class Date:
    def __init__(self, day=0, month=0, year=0): ...

    def string_to_db(self): ...

    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('.'))
        date1 = cls(day, month, year)
        return date1

date1 = Date.from_string('30.12.2020')
date1.string_to_db()
# '2020-12-30'
```
````

---

# Статические методы

Этот тип метода не принимает ни параметра self как метод экземпляра класса, ни параметра cls как метод класса. При этом, конечно, статический метод может принимать произвольное количество других параметров.

Поэтому статический метод не может изменять ни состояние объекта, ни состояние класса. Статические методы ограничены в том, к каким данным они могут получить доступ.

Статические методы отмечаются декоратором `@staticmethod`.

---

# Статические методы

````md magic-move
```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @staticmethod
    def mystaticmethod():
        return 'static method called'
```

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @staticmethod
    def mystaticmethod():
        return 'static method called'

obj = MyClass()
obj.method()
# ('instance method called', <__main__.MyClass object at 0x7f9748403f40>)

obj.mystaticmethod()
# 'static method called'
```

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @staticmethod
    def mystaticmethod():
        return 'static method called'

MyClass.mystaticmethod()
# 'static method called'
MyClass.method()
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: method() missing 1 required positional argument: 'self'
```
````

---

# Применение статических методов

````md magic-move
```python
class Date(object):
    def __init__(self, day=0, month=0, year=0): ...

    def string_to_db(self): ...

    @classmethod
    def from_string(cls, date_as_string): ...


date = Date.from_string('30.12.2020')
date.string_to_db()
# '2020-12-30'
```

```python
class Date(object):
    def __init__(self, day=0, month=0, year=0): ...
    def string_to_db(self): ...
    @classmethod
    def from_string(cls, date_as_string): ...

    @staticmethod
    def is_date_valid(date_as_string):
        if date_as_string.count('.') == 2:
            day, month, year = map(int, date_as_string.split('.'))
            return day <= 31 and month <= 12 and year <= 3999

date = Date.from_string('30.12.2020')
date.string_to_db()
# '2020-12-30'
```

```python
dates = [
    '30.12.2020',
    '30-12-2020',
    '01.01.2021',
    '12.31.2020'
    ]

for string_date in dates:
    if Date.is_date_valid(string_date):
        date = Date.from_string(string_date)
        string_to_db = date.string_to_db()
        print(string_to_db)
    else:
        print(f'Неправильная дата или формат строки с датой')
```
````

---

# Цепочки методов

```python {*|2-6|8-}{maxHeight:'420px'}
class ChessPiece:
    def __init__(self, x=0, y=0, x_lim=7, y_lim=7):
        if x < 0 | x > x_lim | y < 0 | y > y_lim:
            raise ValueError('Invalid coordinates')
        self.x, self.y = x, y
        self.x_lim, self.y_lim = x_lim, y_lim

    def up(self):
        self.y = self.y_lim if self.y == 0 else self.y - 1

    def down(self):
        self.y = 0 if self.y == self.y_lim else self.y + 1

    def left(self):
        self.x = self.x_lim if self.x == 0 else self.x - 1

    def right(self):
        self.x = 0 if self.x == self.x_lim else self.x + 1
```

---

# Цепочки методов

```python
queen = ChessPiece()
queen.right()
queen.down()
queen.left()
queen.up()
queen.down()
queen.down()
queen.right()
queen.right()
print(queen.x, queen.y)
```

---

# Цепочки методов

```python {*|8-}{maxHeight:'420px'}
class ChessPiece:
    def __init__(self, x=0, y=0, x_lim=7, y_lim=7):
        if x < 0 | x > x_lim | y < 0 | y > y_lim:
            raise ValueError('Invalid coordinates')
        self.x, self.y = x, y
        self.x_lim, self.y_lim = x_lim, y_lim

    def up(self):
        self.y = self.y_lim if self.y == 0 else self.y - 1
        return self

    def down(self):
        self.y = 0 if self.y == self.y_lim else self.y + 1
        return self

    def left(self):
        self.x = self.x_lim if self.x == 0 else self.x - 1
        return self

    def right(self):
        self.x = 0 if self.x == self.x_lim else self.x + 1
        return self
```

---

# Цепочки методов

````md magic-move
```python
queen = ChessPiece()
queen.right().down().left().up().down().down().right().right()
print(queen.x, queen.y)
```

```python
queen = ChessPiece()
queen.right()\
    .down()\
    .left()\
    .up()\
    .down()\
    .down()\
    .right()\
    .right()
print(queen.x, queen.y)
```

```python
queen = ChessPiece()
(
    queen.right()
        .down()
        .left()
        .up()
        .down()
        .down()
        .right()
        .right()
)
print(queen.x, queen.y)
```
````

---

# Абстрактные классы и методы

Абстрактный класс имеет некоторые особенности, а именно:

- Абстрактный класс не содержит всех реализаций методов, необходимых для полной работы, это означает, что он содержит один или несколько абстрактных методов. Абстрактный метод - это только объявление метода, без его подробной реализации.
- Абстрактный класс предоставляет интерфейс для подклассов, чтобы избежать дублирования кода. Нет смысла создавать экземпляр абстрактного класса.
- Производный подкласс должен реализовать абстрактные методы для создания конкретного класса, который соответствует интерфейсу, определенному абстрактным классом. Следовательно, экземпляр не может быть создан, пока не будут переопределены все его абстрактные методы.

---
layout: statement
---

Абстрактный класс определяет общий интерфейс для набора подклассов. Он предоставляет общие атрибуты и методы для всех подклассов, чтобы уменьшить дублирование кода. Он также заставляет подклассы реализовывать абстрактные методы, чтобы избежать каких-либо несоответствий.

---

# Определение абстрактного класса

````md magic-move
```python
class Animal():
    @abstractmethod
    def move(self):
        pass

a = Animal()
```

```python
class Animal():
    @abstractmethod
    def move(self):
        pass

a = Animal()
```

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def move(self):
        pass

a = Animal()
# TypeError: Can't instantiate abstract class Animal with abstract methods move
```
````

<div v-click="[1, 2]">

> Класс Animal не является в полной мере абстрактным, так как может быть инициализирован.

</div>

---

# Реализация абстрактного метода

````md magic-move
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def move(self):
        print('Animal moves')

class Cat(Animal):
    def move(self):
        super().move()
        print('Cat moves')

c = Cat()
c.move()
# Animal moves
# Cat moves
```
````
