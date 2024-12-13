---
theme: sirius-college
layout: cover
---

# Объектно-ориентированное программирование<br>Лекция 4

Магические методы. Атрибуты. Метаклассы

---

# Магические методы

- Магические методы в Python — это специальные методы, которые начинаются и заканчиваются двойным подчеркиванием.
- Магические методы не предназначены для непосредственного вызова вами, но вызов происходит внутри класса при определенном действии.

---

# Магические методы

| Конструкторы и деструкторы | Описание                                            |
| -------------------------- | --------------------------------------------------- |
| `__new__(cls, other)`      | Вызывается при создании объекта                     |
| `__init__(self, other)`    | Вызывается при создании объекта, но после `__new__` |
| `__del__(self)`            | Деструктор                                          |

---

# Магические методы. `__new__`

- В таких языках, как Java и C#, оператор new используется для создания нового экземпляра класса.
- В Python магический метод `__new__()` неявно вызывается перед `__init__()` методом.
- Метод `__new__()` возвращает новый объект, который затем инициализируется с помощью `__init__()`.

---

# Магические методы. `__new__`

```python
class MyClass:
    def __new__(cls, other):
        print('__new__ called')
        return super().__new__(cls)

    def __init__(self, other):
        print('__init__ called')


MyClass(1)
# __new__ called
# __init__ called
```

---

# Магические методы

- `__repr__(self)` — информационная строка об объекте. Выводится при вызове функции `repr(...)` или в момент отладки. Для последнего этот метод и предназначен.
- `__str__(self)` — вызывается при вызове функции `str(...)`, возвращает строковый объект.
- `__bytes__(self)` — аналогично `__str__(self)`, только возвращается набор байт.
- `__format__(self, format_spec)` — вызывается при вызове функции `format(...)` и используется для форматировании строки с использованием строковых литералов.

---

# Магические методы сравнения

- `__lt__(self, other)` — определяет поведение оператора сравнения «меньше», `<`.
- `__le__(self, other)` — определяет поведение оператора сравнения «меньше или равно», `<=`.
- `__eq__(self, other)` — определяет поведение оператора «равенства», `==`.
- `__ne__(self, other)` — определяет поведение оператора «неравенства», `!=`.
- `__gt__(self, other)` — определяет поведение оператора сравнения «больше», `>`.
- `__ge__(self, other)` — определяет поведение оператора сравнения «больше или равно», `>=`.

---

# Магические методы сравнения

- `__hash__(self)` — вызывается функцией `hash(...)` и используется для определения контрольной суммы объекта, чтобы доказать его уникальность. Например, чтобы добавить объект в `set`, `frozenset`, или использовать в качестве ключа в словаре `dict`.
- `__bool__(self)` — вызывается функцией `bool(...)` и возвращает `True` или `False` в соответствии с реализацией. Если данный метод не реализован в объекте, и объект является какой-либо последовательностью (списком, кортежем и т.д.), вместо него вызывается метод `__len__`.

---

# Магические методы

- `__getattr__(self, name)` — вызывается методом `getattr(...)` или при обращении к атрибуту объекта через `x.y`, где `x` — объект, а y — атрибут.
- `__setattr__(self, name, value)` — вызывается методом `setattr(...)`или при обращении к атрибуту объекта с последующим определением значения переданного атрибута. Например: `x.y = 1`, где `x` — объект, `y` — атрибут, а `1` — значение атрибута.
- `__delattr__(self, name)` — вызывается методом `delattr(...)`или при ручном удалении атрибута у объекта с помощью `del x.y`, где `x` — объект, а `y` — атрибут.
- `__dir__(self)` — вызывается методом `dir(...)` и выводит список доступных атрибутов объекта.

---

# Магические методы. Создание последовательностей

- `__len__(self)` — вызывается методом `len(...)` и возвращает количество элементов в последовательности.
- `__getitem__(self, key)` — вызывается при обращении к элементу в последовательности по его ключу (индексу). Метод должен выбрасывать исключение `TypeError`, если используется некорректный тип ключа, `KeyError`, если данному ключу не соответствует ни один элемент в последовательности.
- `__setitem__(self, key, value)` — вызывается при присваивании какого-либо значения элементу в последовательности. Также может выбрасывать исключения `TypeError` и `KeyError`.
- `__iter__(self)` — вызывается методом `iter(...)` и возвращает итератор последовательности, например, для использования объекта в цикле.
- `__contains__(self, item)` — вызывается при проверке принадлежности элемента к последовательности с помощью `in` или `not in`.

---

# Числовые магические методы. Унарные операторы

- `__neg__(self)` — определяет поведение для отрицания (`-a`)
- `__pos__(self)` — определяет поведение для унарного плюса (`+a`)
- `__abs__(self)` — определяет поведение для встроенной функции `abs(...)`
- `__invert__(self)` — определяет поведение для инвертирования оператором `~`

---

# Числовые магические методы. Обычные арифметические операторы

- `__add__(self, other)` — сложение, оператор `+`
- `__sub__(self, other)` — вычитание, оператор `-`
- `__mul__(self, other)` — умножение, оператор `*`
- `__truediv__(self, other)` — деление, оператор `/`
- `__floordiv__(self, other)` — целочисленное деление, оператор `//`
- `__mod__(self, other)` — остаток от деления, оператор `%`
- `__pow__(self, other[, modulo])` — возведение в степень, оператор - `**`
- `__and__(self, other)` — двоичное И, оператор `&`
- `__xor__(self, other)` — исключающее ИЛИ, оператор `^`
- `__or__(self, other)` — двоичное ИЛИ, оператор `|`

---

# Числовые магические методы. Составное присваивание

- `__iadd__(self, other)` — сложение с присваиванием, оператор `+=`
- `__isub__(self, other)` — вычитание с присваиванием, оператор `-=`
- `__imul__(self, other)` — умножение с присваиванием, оператор `*=`
- `__itruediv__(self, other)` — деление с присваиванием, оператор `/=`
- `__ifloordiv__(self, other)` — целочисленное деление с присваиванием, оператор `//=`
- `__imod__(self, other)` — остаток от деления с присваиванием, оператор `%=`
- `__ipow__(self, other[, modulo])` — возведение в степень с присваиванием, оператор `**=`
- `__iand__(self, other)` — двоичное И с присваиванием, оператор `&=`
- `__ixor__(self, other)` — исключающее ИЛИ с присваиванием, оператор `^=`
- `__ior__(self, other)` — двоичное ИЛИ с присваиванием, оператор `|=`

---

# Атрибуты

```python
class Human:
    planet = "Earth"

    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

print(getattr(Human, "planet"))
# 'Earth'
```

---

# Атрибуты

```python
class Human:
    planet = "Earth"

    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

setattr(Human, "has_children", True)
print(getattr(Human, "has_children"))
# True
```

---

# Атрибуты

```python
class Human:
    planet = "Earth"

    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

def human_apple_conversion(self, apples: int) -> str:
    return f"{self.name} has {apples} apples"

setattr(Human, "apple_conversion", human_apple_conversion)
petya = Human("Petya", 20)
print(petya.apple_conversion(12))
# Petya has 12 apples
```

---

# Атрибуты

```python {all|2,6,9|12-18|12,13,15,17|14,16,18}{maxHeight: '420px'}
class MySpecialFunction:
    init_counter = 0

    def __init__(self) -> None:
        self.call_counter = 0
        self.__class__.init_counter += 1

    def __call__(self, *args: tuple[int | float]) -> int:
        self.call_counter += 1
        return sum(args)

fun_1 = MySpecialFunction()
fun_2 = MySpecialFunction()
print(fun_1(1, 2, 3)) # 6
print(fun_1.init_counter) # 2
print(fun_1.call_counter) # 1
print(fun_2.init_counter) # 2
print(fun_2.call_counter) # 0
```

---

# Метаклассы

````md magic-move
```python
a = 1
print(a.__class__)
# <class 'int'>
```

```python
a = 1
print(a.__class__.__class__)
# <class 'type'>
```
````

::v-click

**Метакласс** — это «класс классов». Как класс определяет поведение объекта, метакласс определяет поведение класса. По умолчанию в Python все классы создаются из встроенного метакласса `type`.

::

---

# Метаклассы

```python
class Human:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

Student = type("Student", (Human,), {'college': "Sirius"})
vanya = Student("Vanya", 20)
print(type(Student))
# <class '__main__.Student'>
print(vanya)
# <__main__.Student object at 0x7f9b9b5b0c10>
```

---

# Метаклассы

```python
class Human:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

def student_str(self) -> str:
    return f"{self.name} is {self.age} years old"
student_attrs = {
    'college': "Sirius",
    '__str__': student_str
}
Student = type("Student", (Human,), student_attrs)
vanya = Student("Vanya", 20)
print(vanya)
# Vanya is 20 years old
```

---

# Метаклассы

```python {all|12-15|17-21|23-26|all}{maxHeight: '420px'}
class Human:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

    def live(self) -> str:
        return f"{self.name}: what is going on?"

def student_str(self) -> str:
    return f"{self.name} is {self.age} years old"

def student_init(self, name: str, age: int, group: int) -> None:
    self.name = name
    self.age = age
    self.group = group

student_attrs = {
    'college': "Sirius",
    '__str__': student_str,
    '__init__': student_init
}

Student = type("Student", (Human,), student_attrs)
vanya = Student("Vanya", 20, "K0709-23")
print(type(vanya), vanya, vanya.live(), sep="|")
# <class '__main__.Student'>|Vanya is 20 years old|Vanya: what is going on?
```

---

# Создание метакласса

```py
class MyMeta(type):
    def __init__(cls, name, bases, attrs):
        print(f"Создание класса {name}")
        super().__init__(name, bases, attrs)

class MyClass(metaclass=MyMeta):
    pass  # Создание класса MyClass
```

---

# Применение метаклассов

Метаклассы используются редко из-за их сложности, но они могут быть чрезвычайно мощными.

Django использует метаклассы для создания моделей базы данных. Это позволяет разработчикам определять структуры своих баз данных с помощью простого Python-кода, и метаклассы автоматически преобразуют эти определения в SQL-запросы для создания таблиц.
