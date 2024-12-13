---
theme: sirius-college
layout: cover
---

# Объектно-ориентированное программирование<br>Лекция 2

Принципы проектирования. Модификаторы доступа. Геттеры, сеттеры, property.

---
layout: two-cols
---

# Zen of Python

- Beautiful is better than ugly.
- Explicit is better than implicit.
- Simple is better than complex.
- Complex is better than complicated.

::right::

- Красивое лучше уродливого.
- Явное лучше неявного.
- Простое лучше сложного.
- Сложное лучше запутанного.

---
layout: fact
---

# DRY

## [Don't Repeat Yourself]{v-click}

## [Не повторяйся]{v-click=1}

---
layout: fact
---

# KISS

## [Keep it stupid simple]{v-click}

## [Пусть оно будет простым до безобразия]{v-click=1}

## [(Keep it simple, stupid)]{v-click=2}

---
layout: fact
---

# SOLID

<div style="text-align: left;color: white;font-size: 2rem;" v-clicks>

### **S**ingle responsibility principle (принцип единственной ответственности)

### **O**pen-closed principle (принцип открытости/закрытости)

### **L**iskov substitution principle (принцип подстановки Лисков)

### **I**nterface segregation principle (принцип разделения интерфейса)

### **D**ependency inversion principle (принцип инверсии зависимостей)

</div>

---

# S - Single responsibility principle

Каждый блок вашего кода должен выполнять одну задачу

````md magic-move
```python
class TicketAndCardChecker:
    def check_ticket(ticket: str):
        if ticket.isdigit() and len(ticket) == 10:
            ...
        return False

    def check_card (card: str):
        card = card. replace(' ', '')
        if card.isdigit() and len(card) <= 17:
            ...
        return False

```

```python
class TicketChecker:
    def check(ticket: str):
        if ticket. isdigit() and len(ticket) == 10:
            ...
        return False

class CardChecker:
    def check(card: str):
        card = card. replace(' ', '')
        if card.isdigit() and len(card) <= 17:
            ...
        return False

```
````

---

# O - Open-closed principle

Ваши модули или библиотеки должны быть открыты для расширения, но закрыты для модификации.

````md magic-move
```python
class Building:
    known_buildings = ['Arena', 'Lyceum', 'Park']

    def valid_building(name) :
        return name in Building.known_buildings
```

```python
from dotenv import load_dotenv
from os import environ

class Building:
    def __init__(self) :
        load_dotenv ()
        buildings = environ.get ( 'BUILDINGS', default='')
        self. known_buildings = buildings.split()
        print (self. known_buildings)

    def valid_building (self, name) :
        return name in self.known buildings

```

```

```
````

---

# L - Liskov substitution principle

- Функции (и классы), которые используют указатели или ссылки на базовые классы, должны иметь возможность использовать подтипы базового типа, ничего не зная об их существовании.
- Подкласс не должен создавать новых мутаторов свойств базового класса.

---

# L - Liskov substitution principle

<div v-mark.red.cross>

```py
class Email:
    def __init__(self, username: str, host: str):
        self.email = '{0}@{1}'. format(username, host)

    def isvalid(self):
        known_hosts = ['yandex.ru', 'gmail.com']
        for host in known_hosts:
            if self.email.endswith(host):
                return True
        return False

class SiriusEmail(Email):
    def __init__(self, username: str) :
        super().__init__(username, 'talantiuspeh.ru')
```

</div>

---

# I - Interface segregation principle

Программные сущности не должны зависеть от методов, которые они не используют.

Отделяйте и разделяйте методы, не заставляйте пользователей (вашего кода) использовать ненужные или навязанные методы.

<div v-mark.red.cross>

```py
class Url:
    def opener(url: str, python_version: str, django_version: str):
        if python_version.startswith('3.'):
            if python version < '3.8' and django_version < '4.0.1':
                open_url(url)
```

</div>

---

# D - Dependency inversion principle

- Модули верхних уровней не должны импортировать сущности из модулей нижних уровней. Оба типа модулей должны зависеть от абстракций.
- Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

---

# Модификаторы доступа в python

Уровни доступа задаются с помощью искажения имен (name mangling).

````md magic-move
```py
class Human:
    def __init__(self, name) :
        self.name = name
        self.age = 0
        self.passport = Passport()
```

```py
class Human:
    def __init__(self, name) :
        self.name = name # public
        self._age = 0 # private
        self.__passport = Passport() # hidden
```

```py
class Human:
    def __init__(self, name) :
        self.name = name # public
        self._age = 0 # private
        self.__passport = Passport() # hidden

petya = Human('Petya')
print(petya.name)
print(petya._age)
print(petya.__passport) # error
```

```py
class Human:
    def __init__(self, name) :
        self.name = name # public
        self._age = 0 # private
        self.__passport = Passport() # hidden

petya = Human('Petya')
print(petya.name)
print(petya._age)
print(petya._Human__passport) # error
```
````

---

# Геттеры и сеттеры

````md magic-move
```py
class Human:
    def __init__(self, name):
        self.name = name
        self.__age = 0 # hidden
```

```py
class Human:
    def __init__(self, name):
        self.name = name
        self.__age = 0 # hidden

    def get_age(self) :
        return self.__age

    def set_age(self, new_age: int):
        self.__age = new_age

    def del_age (self) :
        del self.__age
```

```py
class Human:
    def __init__(self, name):
        self.name = name
        self.__age = 0 # hidden

    def get_age(self) :
        return self.__age

    def set_age(self, new_age: int):
        self.__age = new_age

    def del_age (self) :
        del self.__age

    age = property(get_age, set_age, del_age)
```

```py
class Human:
    def __init__(self, name):
        self.name = name
        self.__age = 0 # hidden

    @property
    def age(self) :
        return self.__age

    @property.setter
    def age(self, new_age: int):
        self.__age = new_age

    @property.deleter
    def age (self) :
        del self.__age
```
````
