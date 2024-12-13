---
theme: sirius-college
layout: cover
---

# Объектно-ориентированное программирование<br>Лекция 5

Наследование. Множественное наследование. Декорирование классов. Mixins. Dataclasses.

---

# Наследование

Наследование позволяет при создании нового класса указать для него **базовый класс**. От базового класса наследуется вся его структура — атрибуты и методы. Созданный класс-наследник называется **производным классом**.

Наследование позволяет выделить общее для нескольких классов поведение и вынести его в отдельную сущность. То есть наследование является средством **переиспользования кода (code reuse)** — использования существующего кода для решения новых задач.

```python
class DerivedClassName(BaseClassName):
    ...
```

---

# Наследование

Выполнение определения производного класса `DerivedClassName` происходит так же, как и для базового класса `BaseClassName`. Когда объект класса создан, базовый класс `BaseClassName` запоминается. Это используется для разрешения ссылок на атрибуты. Если запрошенный атрибут не найден в классе `DerivedClassName`, поиск переходит к поиску в базовом классе `BaseClassName`. Это правило применяется рекурсивно, если сам базовый класс является производным от какого-либо другого класса.

---

# Наследование

Используйте `isinstance()` для проверки типа экземпляра класса: `isinstance(obj, int)` будет истинным `True` только в том случае, если `obj.__class__` равен `int` или класс является производным от класса `int`.

Используйте `issubclass()` для проверки наследования классов: `issubclass(bool, int)` является истинным, так как `bool` является подклассом `int()`. Однако `issubclass(float, int)` является ложным `False`, так как `float` не является подклассом `int`.

---

# Наследование

Переопределяющий метод в производном классе может фактически расширить, а не просто заменить метод базового класса с тем же именем. Существует простой способ вызвать метод базового класса напрямую: просто вызовите `BaseClassName.methodname(self, arguments)`.

> Обратите внимание, что это работает только в том случае, если базовый класс доступен как имя базового класса `BaseClassName` в глобальной области видимости.

---

# Наследование

```python {*|9-12|11}{maxHeight: '420px'}
class Parent:
    def __init__(self):
        self.parent_attribute = 'I am a parent'

    def parent_method(self):
        print('Back in my day...')


class Child(Parent):
    def __init__(self):
        Parent.__init__(self)
        self.child_attribute = 'I am a child'
```

---

# Наследование

В простейшем случае функцию `super()` можно использовать для замены явного вызова `Parent.__init__(self)`.

```python {*|11}{maxHeight: '420px'}
class Parent:
    def __init__(self):
        self.parent_attribute = 'I am a parent'

    def parent_method(self):
        print('Back in my day...')


class Child(Parent):
    def __init__(self):
        super().__init__()
        self.child_attribute = 'I am a child'
```

---

# Множественное наследование

````md magic-move
```python
class B:
    def b(self):
        print('b')
class C:
    def c(self):
        print('c')
class D(B, C):
    def d(self):
        print('d')

d = D()
d.b() # >> b
d.c() # >> c
d.d() # >> d
```

```python
class B:
    def x(self):
        print('x: B')
class C:
    def x(self):
        print('x: C')
class D(B, C):
    pass
```

```python
class B:
    def x(self):
        print('x: B')
class C:
    def x(self):
        print('x: C')
class D(B, C):
    pass

d = D()
d.x() # >> ?
```

```python
class B:
    def x(self):
        print('x: B')
class C:
    def x(self):
        print('x: C')
class D(B, C):
    pass

d = D()
d.x() # >> x: B
```

```python
class B:
    def x(self):
        print('x: B')
class C:
    def x(self):
        print('x: C')
class D(B, C):
    pass

d = D()
d.x() # >> x: B
print(D.mro()) # multiple-resolution order
# [<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class 'object'>]
```
````

---

# Множественное наследование. Diamond problem

```python {*|1-5|7-12|14-19|21-25|27-31|27,32-39|41-43}{maxHeight: '420px'}
class Tokenizer:
    def __init__(self, text):
        print('Start Tokenizer.__init__()')
        self.tokens = text.split()
        print('End Tokenizer.__init__()')

class WordCounter(Tokenizer):
    def __init__(self, text):
        print('Start WordCounter.__init__()')
        super().__init__(text)
        self.word_count = len(self.tokens)
        print('End WordCounter.__init__()')

class Vocabulary(Tokenizer):
    def __init__(self, text):
        print('Start init Vocabulary.__init__()')
        super().__init__(text)
        self.vocab = set(self.tokens)
        print('End init Vocabulary.__init__()')

class TextDescriber(WordCounter, Vocabulary):
    def __init__(self, text):
        print('Start init TextDescriber.__init__()')
        super().__init__(text)
        print('End init TextDescriber.__init__()')

td = TextDescriber('row row row your boat')
print('--------')
print(td.tokens)
print(td.vocab)
print(td.word_count)
# Start init TextDescriber.__init__()
# Start WordCounter.__init__()
# Start init Vocabulary.__init__()
# Start Tokenizer.__init__()
# End Tokenizer.__init__()
# End init Vocabulary.__init__()
# End WordCounter.__init__()
# End init TextDescriber.__init__()
# --------
# ['row', 'row', 'row', 'your', 'boat']
# {'boat', 'your', 'row'}
# 5
```

<!--
Каждый метод `__init__` вызывался один и только один раз.

Класс `TextDescriber` унаследован от двух классов, унаследованных от `Tokenizer`. Почему `Tokenizer.__init__` не вызывался дважды?

Если бы мы заменили все наши вызовы `super()` старомодным способом, мы получили бы 2 вызова `Tokenizer.__init__`. Использование `super()` заранее «обдумывает» наш код и пропускает лишнее действие в виде двойного вызова `Tokenizer.__init__`.

Каждый метод `__init__` был запущен до того, как любой из других был завершен.

На порядок начала и окончания каждого `__init__` стоит обращать внимание, если вы пытаетесь установить атрибут, имя которого конфликтует с другим родительским классом. Атрибут будет перезаписан, и это может все запутать.
-->

---

# Декорирование классов

````md magic-move
```python
# декорирование функции
def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print('decorated')
        return func(*args, **kwargs)
    return wrapper

@decorator
def printer(message):
    print(message)
```

```python
# декорирование функции с параметром
def decorator_create(message):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print(message)
            return func(*args, **kwargs)
        return wrapper
    return decorator

@decorator_create('hello')
def printer(message):
    print(message)
```

```python
# декорирование классом
class DecoratorClass:
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        print('decorated')
        return self.func(*args, **kwargs)


@DecoratorClass
def printer(message):
    print(message)
```

```python
# декорирование классом с параметром
class DecoratorClass:
    def __init__(self, message):
        self.message = message
    def __wrap_function(self, func):
        def wrapper(*args, **kwargs):
            print(self.message)
            return func(*args, **kwargs)
        return wrapper
    def __call__(self, func):
        return self.__wrap_function(func)

@DecoratorClass('decorated')
def printer(message):
    print(message)
```
````

---

# Декорирование классов

```python
def add_str_dunder(class_):
    def wrapper(self):
        attrs = [f'{k}={v}' for k, v in self.__dict__.items()]
        return ", ".join(attrs)
    setattr(class_, "__str__", wrapper)
    return class_

@add_str_dunder
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

print(Person('Petya', 20))
# name=Petya, age=20
```

---

# Декорирование классов

```python {*}{maxHeight: '420px'}
class StrDunderModifier:
    def __init__(self, class_):
        self.class_ = class_

    def __call__(self, *args, **kwargs):
        def str_method(self):
            attrs = [f'{k}={v}' for k, v in self.__dict__.items()]
            return ", ".join(attrs)
        setattr(self.class_, "__str__", str_method)
        return self.class_

@StrDunderModifier
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

print(Person('Petya', 20))
# name=Petya, age=20
```

---

# Mixins

**Mixin классы (примеси)** - это классы у которых нет данных, но есть методы. Mixin используются для добавления одних и тех же методов в разные классы.

```python {*|1-4|6-14|16-19}{maxHeight: '340px'}
class StrMixin:
    def __str__(self):
        attrs = [f'{k}={v}' for k, v in self.__dict__.items()]
        return ", ".join(attrs)

class Person(StrMixin):
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Bike(StrMixin):
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

print(Person('Petya', 20))
# name=Petya, age=20
print(Bike('Kawasaki', 'Ninja'))
# brand=Kawasaki, model=Ninja
```

---

# Dataclasses

```python {*}{maxHeight: '420px'}
class Message:
    def __init__(self, id_, sender, recipient, text):
        self.__id = id_
        self.__sender = sender
        self.__recipient = recipient
        self.__text = text

    @property
    def id(self):
        return self.__id

    @property
    def sender(self):
        return self.__sender

    @property
    def recipient(self):
        return self.__recipient

    @property
    def text(self):
        return self.__text

    def __str__(self):
        class_name = self.__class__.__name__
        return f'{class_name} id={self.id} from {self.sender} \
            to {self.recipient}: {self.text}'

    def __repr__(self):
        class_name = self.__class__.__name__
        result = "{} id={} from {} to {}:"
        return result.format(class_name, self.id, self.sender,
            self.recipient)

    def __eq__(self, other):
        if not self.__class__ is other.__class__:
            return False
        attrs = ('id', 'sender', 'recipient', 'text')
        return all(
            getattr(self, attr) == getattr(other, attr)
            for attr in attrs
        )

    def __ne__(self, other):
        return not self.__eq__(other)

    def __hash__(self):
        return hash((self.id, self.sender, self.recipient, self.text))

    def __gt__(self, other):
        return self.id > other.id

    def __ge__(self, other):
        return self.id >= other.id

    def __lt__(self, other):
        return self.id < other.id

    def __le__(self, other):
        return self.id <= other.id
```

---

# Dataclasses

**Dataclasses** призваны автоматизировать генерацию кода классов, которые используются для хранения данных. Не смотря на то, что они используют другие механизмы работы, их можно сравнить с "изменяемыми именованными кортежами со значениями по умолчанию".

<v-click>

```python
from dataclasses import dataclass

@dataclass(frozen=True, order=True, unsafe_hash=True)
class Message:
    id_: int
    sender: str
    recipient: str
    text: str
```

</v-click>

---

# Dataclasses

```python
@dataclasses.dataclass(*, init=True, repr=True, eq=True, order=False, 
unsafe_hash=False, frozen=False, match_args=True, kw_only=False, 
slots=False, weakref_slot=False)
```

- `init` — метод `__init__` будет создан;
- `repr` — метод `__repr__` будет создан;
- `eq` — метод `__eq__` будет создан, сравнение будет производиться как сравнение кортежей полей;
- `order` — методы `__lt__`, `__le__`, `__gt__`, `__ge__` будут созданы, сравнение будет производиться как сравнение кортежей полей;
- `unsafe_hash` — метод `__hash__` будет создан;
- `frozen` — методы `__setattr__`, `__delattr__` будут вызывать ошибку.