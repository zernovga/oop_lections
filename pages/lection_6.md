---
theme: sirius-college
layout: cover
---

# Объектно-ориентированное программирование<br>Лекция 6

Паттерны проектирования. Фундаментальные паттерны.

---

# Паттерн (шаблон) проектирования (Design pattern)

- Описание взаимодействия объектов и классов, адаптированных для решения общей задачи проектирования в конкретном контексте.
- Повторяемая архитектурная конструкция в сфере проектирования программного обеспечения, предлагающая решение проблемы проектирования в рамках некоторого часто возникающего контекста.

---

# Составные части определения паттерна

1. Имя
2. Решаемая задача
3. Предлагаемое решение
4. Ожидаемый результат, последствия

---

# Имя

- Ссылаясь на него, можно сразу описать проблему проектирования; ее решения и их последствия.
- Присваивание паттернам имен позволяет проектировать на более высоком уровне абстракции.
- С помощью словаря паттернов можно вести обсуждение с коллегами, упоминать паттерны в документации, в тонкостях представлять дизайн системы.

---

# Задача

- Описание того, когда следует применять паттерн.
- Необходимо сформулировать задачу и ее контекст. Может описываться конкретная проблема проектирования, например способ представления алгоритмов в виде объектов.
- Иногда отмечается, какие структуры классов или объектов свидетельствуют о негибком дизайне. Также может включаться перечень условий, при выполнении которых имеет смысл применять данный паттерн.

---

# Решение

- Описание элементов дизайна, отношений между ними, функций каждого элемента.
- Конкретный дизайн или реализация не имеются в виду, поскольку паттерн - это шаблон, применимый в самых разных ситуациях.
- Наоборот, дается абстрактное описание задачи проектирования и того, как она может быть решена с помощью некоего весьма обобщенного сочетания элементов (классов, объектов, свойств, методов).

---

# Результат

- Следствия применения паттерна и разного рода компромиссы.
- Хотя при описании проектных решений о последствиях часто не упоминают, знать о них необходимо, чтобы можно было выбрать между различными вариантами и оценить преимущества и недостатки данного паттерна.
- Здесь речь идет и о выборе языка и реализации. Поскольку в объектно-ориентированном проектировании повторное использование зачастую является важным фактором, то к результатам следует относить и влияние на степень гибкости, расширяемости и переносимости системы.
- Перечисление всех последствий поможет вам понять и оценить их роль.

---

# Паттерны бывают и другими

- Архитектурные шаблоны - это паттерны высокоуровневого проектирования системы
- Алгоритмы - это вычислительные паттерны

---

# Виды паттернов проектирования

<table>
<tr>
<th v-click>Фундаментальные</th>
<th v-click="6">Порождающие</th>
<th v-click="12">Структурные</th>
<th v-click="20">Поведенческие</th>
</tr>
<tr>
<td style="vertical-align: top;">

<v-clicks depth="2">

- Интерфейс
- Делегирование
- Контейнер свойств
- Канал событий

</v-clicks>
</td>
<td style="vertical-align: top;">
<v-clicks depth="2" at="7">

- Абстрактная фабрика
- Строитель
- Фабричный метод
- Прототип
- Синглтон

</v-clicks>
</td>
<td style="vertical-align: top;">
<v-clicks depth="2" at="13">

- Адаптер
- Фасад
- Мост
- Компоновщик
- Декоратор
- Приспособленец
- Заместитель

</v-clicks>
</td>
<td style="vertical-align: top;">
<v-clicks depth="2" at="21">

- Цепочка обязанностей
- Команда
- Интерпретатор
- Итератор
- Посредник
- Хранитель
- Наблюдатель
- Состояние
- Стратегия
- Шаблонный метод
- Посетитель

</v-clicks>
</td></tr>
</table>

---

# Фундаментальные (основные, базовые)

- Это паттерны проектирования, которые, скорее, являются основами, на которых строится всё остальное взаимодействие.
- Фундаментальные паттерны в том или ином виде реализованы в объектно-ориентированных языках программирования.
- Примеры: интерфейс, интерфейс-маркер, неизменяемый интерфейс, шаблон делегирования, шаблон функционального дизайна, контейнер свойств, канал событий.

---

# Фундаментальные паттерны: Интерфейсы

- Интерфейс - общий метод или общий класс.
- Неизменяемый интерфейс - метод, создающий неизменяемый объект.
- Интерфейс-маркер - метод с проверкой типов.

---

# Интерфейс

```python {*|1-8|11-19|22-32|28-32|35-36|38-42}{maxHeight: '420px'}
class InformalParserInterface:
    def load_data_source(self, path: str, file_name: str) -> str:
        """Load in the file for extracting text."""
        pass

    def extract_text(self, full_file_name: str) -> dict:
        """Extract text from the currently loaded file."""
        pass


class PdfParser(InformalParserInterface):
    """Extract text from a PDF"""
    def load_data_source(self, path: str, file_name: str) -> str:
        """Overrides InformalParserInterface.load_data_source()"""
        pass

    def extract_text(self, full_file_path: str) -> dict:
        """Overrides InformalParserInterface.extract_text()"""
        pass


class EmlParser(InformalParserInterface):
    """Extract text from an email"""
    def load_data_source(self, path: str, file_name: str) -> str:
        """Overrides InformalParserInterface.load_data_source()"""
        pass

    def extract_text_from_email(self, full_file_path: str) -> dict:
        """A method defined only in EmlParser.
        Does not override InformalParserInterface.extract_text()
        """
        pass


issubclass(PdfParser, InformalParserInterface) # True
issubclass(EmlParser, InformalParserInterface) # True

PdfParser.__mro__
# (__main__.PdfParser, __main__.InformalParserInterface, object)

EmlParser.__mro__
# (__main__.EmlParser, __main__.InformalParserInterface, object)
```

---

# Интерфейс

```python {*|1-11|4-5|7-11|14-19|22-30|33-43|46-47|49-50}{maxHeight: '420px'}
class ParserMeta(type):
    """A Parser metaclass that will be used for parser class creation.
    """
    def __instancecheck__(cls, instance):
        return cls.__subclasscheck__(type(instance))

    def __subclasscheck__(cls, subclass):
        return (hasattr(subclass, 'load_data_source') and
                callable(subclass.load_data_source) and
                hasattr(subclass, 'extract_text') and
                callable(subclass.extract_text))


class UpdatedInformalParserInterface(metaclass=ParserMeta):
    """This interface is used for concrete classes to inherit from.
    There is no need to define the ParserMeta methods as any class
    as they are implicitly made available via .__subclasscheck__().
    """
    pass


class PdfParserNew:
    """Extract text from a PDF."""
    def load_data_source(self, path: str, file_name: str) -> str:
        """Overrides UpdatedInformalParserInterface.load_data_source()"""
        pass

    def extract_text(self, full_file_path: str) -> dict:
        """Overrides UpdatedInformalParserInterface.extract_text()"""
        pass


class EmlParserNew:
    """Extract text from an email."""
    def load_data_source(self, path: str, file_name: str) -> str:
        """Overrides UpdatedInformalParserInterface.load_data_source()"""
        pass

    def extract_text_from_email(self, full_file_path: str) -> dict:
        """A method defined only in EmlParser.
        Does not override UpdatedInformalParserInterface.extract_text()
        """
        pass


issubclass(PdfParserNew, UpdatedInformalParserInterface) # True
issubclass(EmlParserNew, UpdatedInformalParserInterface) # False

PdfParserNew.__mro__
# (<class '__main__.PdfParserNew'>, <class 'object'>)

```

---

# Интерфейс

```python {*|1-3|4-10|12-20|22-30|32-42|44-49}{maxHeight: '420px'}
import abc

class FormalParserInterface(metaclass=abc.ABCMeta):
    @classmethod
    def __subclasshook__(cls, subclass):
        return (hasattr(subclass, 'load_data_source') and
                callable(subclass.load_data_source) and
                hasattr(subclass, 'extract_text') and
                callable(subclass.extract_text) or
                NotImplemented)

    @abc.abstractmethod
    def load_data_source(self, path: str, file_name: str):
        """Load in the data set"""
        raise NotImplementedError

    @abc.abstractmethod
    def extract_text(self, full_file_path: str):
        """Extract text from the data set"""
        raise NotImplementedError

class PdfParserNew(FormalParserInterface):
    """Extract text from a PDF."""
    def load_data_source(self, path: str, file_name: str) -> str:
        """Overrides FormalParserInterface.load_data_source()"""
        pass

    def extract_text(self, full_file_path: str) -> dict:
        """Overrides FormalParserInterface.extract_text()"""
        pass

class EmlParserNew(FormalParserInterface):
    """Extract text from an email."""
    def load_data_source(self, path: str, file_name: str) -> str:
        """Overrides FormalParserInterface.load_data_source()"""
        pass

    def extract_text_from_email(self, full_file_path: str) -> dict:
        """A method defined only in EmlParser.
        Does not override FormalParserInterface.extract_text()
        """
        pass

pdf_parser = PdfParserNew()
eml_parser = EmlParserNew()
# Traceback (most recent call last):
#   ...
# TypeError: Can't instantiate abstract class EmlParserNew with
# abstract methods extract_text
```

---

# Фундаментальные методы: Функциональный дизайн

Функциональный дизайн гарантирует, что каждый модуль компьютерной программы имеет только одну обязанность и исполняет её с минимумом побочных эффектов на другие части программы.

---

# Фундаментальные паттерны: Делегирование

- Делегирование - шаблон проектирования, в котором объект внешне выражает некоторое поведение, но в реальности передаёт ответственность за выполнение этого поведения связанному объекту.
- Шаблон делегирования является фундаментальной абстракцией, на основе которой реализованы другие шаблоны - композиция, агрегация, миксины (mixins) и аспекты (aspects).

---

# Фундаментальные паттерны: Контейнер свойств

- Контейнер свойств предоставляет механизм для динамического расширения объектов дополнительными атрибутами во время выполнения.
- Кроме этого, приложению могут потребоваться ещё модули, которые явным образом используют преимущества нового свойства, если оно было добавлено.
- В Python есть встроенный метод-пример этого паттерна, общий для всех объектов - `setattr`

---

# Фундаментальные паттерны. Канал событий

- Используется для создания канала связи и коммуникации через него посредством событий. Этот канал обеспечивает возможность разным издателям публиковать события и подписчикам, подписываясь на них, получать уведомления.
- В python типичным примером `Event` и `Condition` в конкурентном программировании, где одни абстракции могут подписываться на уведомления (`wait`), а другие - публиковать их (`notify`).
