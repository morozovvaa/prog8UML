# Диаграмма вариантов использования (use case)

Use Case показывает функции системы (просмотр событий, создание персоны, поиск), кто использует эти функции (посетитель, администратор)

```
@startuml
left to right direction
skinparam packageStyle rectangle

title Диаграмма вариантов использования\nКраеведческий веб-ресурс

' Акторы (Admin сверху)
actor "Администратор" as Admin
actor "Посетитель" as Visitor

' Основная система
rectangle "Краеведческий веб-ресурс" {
  
  ' Публичный функционал
  package "Публичный доступ" #AliceBlue {
    usecase "Просмотр главной\nстраницы" as UC1
    usecase "Просмотр галереи\nфотографий" as UC2
    usecase "Просмотр страницы\nсобытия" as UC3
    usecase "Просмотр страницы\nперсоны" as UC4
    usecase "Просмотр статьи" as UC5
    usecase "Поиск материалов" as UC6
  }
  
  ' Административный функционал
  package "Управление контентом" #MistyRose {
    usecase "Авторизация" as UC8
    
    usecase "Управление\nсобытиями" as UC9
    usecase "Создать событие" as UC9_1
    usecase "Редактировать событие" as UC9_2
    usecase "Удалить событие" as UC9_3
    
    usecase "Управление\nперсонами" as UC10
    usecase "Создать персону" as UC10_1
    usecase "Редактировать персону" as UC10_2
    usecase "Удалить персону" as UC10_3
    
    usecase "Управление\nсправочниками" as UC11
    usecase "Управление улицами" as UC11_1
    usecase "Управление профессиями" as UC11_2
    usecase "Управление книгами" as UC11_3
    usecase "Управление ключевыми словами" as UC11_4
  }
}

' Связи посетителя
Visitor --> UC1
Visitor --> UC2
Visitor --> UC3
Visitor --> UC4
Visitor --> UC5
Visitor --> UC6

' Администратор наследует права посетителя (стрелка вниз)
Admin -down-|> Visitor

' Связи администратора
Admin --> UC8
Admin --> UC9
Admin --> UC10
Admin --> UC11

' Include отношения для событий
UC9 ..> UC9_1 : <<include>>
UC9 ..> UC9_2 : <<include>>
UC9 ..> UC9_3 : <<include>>

' Include отношения для персон
UC10 ..> UC10_1 : <<include>>
UC10 ..> UC10_2 : <<include>>
UC10 ..> UC10_3 : <<include>>

' Include отношения для справочников
UC11 ..> UC11_1 : <<include>>
UC11 ..> UC11_2 : <<include>>
UC11 ..> UC11_3 : <<include>>
UC11 ..> UC11_4 : <<include>>

' Зависимости поиска и фильтрации
UC6 ..> UC3 : <<extend>>
UC6 ..> UC4 : <<extend>>

' Заметки
note right of Visitor
  GET запросы
  без токена
end note

note right of Admin
  POST/PUT/PATCH/DELETE
  требуется токен
end note

note bottom of UC6
  Поиск по событиям,
  персонам, тексту
end note
@enduml
```

<img width="1084" height="1576" alt="image" src="https://github.com/user-attachments/assets/63e7644a-63a5-4aca-afae-1704a6b46590" />


# Диаграмма деятельности (activity)

Диаграмма деятельности показывает как именно выполняется конкретный процесс или вариант использования - пошаговую последовательность действий с учетом условий, ветвлений и взаимодействия между участниками.

```
@startuml
title Диаграмма деятельности\nАдаптивный краеведческий веб-ресурс

|Посетитель|
start
:Открытие главной страницы;
:Просмотр галереи фотографий;

if (Нужна конкретная\nинформация?) then (да)
  
  if (Известны критерии?) then (да)
    :Использование фильтрации\n(по дате, улице, категории);
  else (нет)
    :Использование поиска\n(по тексту);
  endif
  
  |Система|
  :Обработка GET запроса;
  :Поиск в базе данных;
  :Формирование результатов;
  
  |Посетитель|
  :Просмотр результатов;
  
else (нет)
  :Просмотр списка\nсобытий/персон;
endif

:Выбор материала;
:Просмотр детальной страницы;
:Чтение статьи;

stop
@enduml
```
<img width="807" height="876" alt="image" src="https://github.com/user-attachments/assets/6ea1180d-14b9-44e2-b184-a8451b111d99" />

# Диаграмма классов (classes)

Диаграмма классов UML используется для моделирования предметной области и проектирования архитектуры системы. Она определяет сущности (персоны, события, улицы) и их связи, из которых автоматически генерируются Django модели и REST API endpoints.

```
@startuml
title Диаграмма классов\nКраеведческий веб-ресурс
skinparam linetype ortho
skinparam packageStyle rectangle
skinparam nodesep 150
skinparam ranksep 150
skinparam padding 10
package "Справочные модели" #E6F3FF {
  class Street {
    +id: Integer
    +name: String
    __
    +__str__(): String
  }
  
  class Profession {
    +id: Integer
    +name: String
    __
    +__str__(): String
  }
  
  class Book {
    +id: Integer
    +author: String
    +title: String
    +url: String
    +image: ImageField
    __
    +__str__(): String
  }
  
  class Keyword {
    +id: Integer
    +keyword: String
    __
    +__str__(): String
  }
}
package "Основные модели" #FFE6E6 {
  class Event {
    +id: Integer
    +title: String
    +date: Date
    +description_html: Text
    +image: ImageField
    __
    +__str__(): String
    +day: Integer
    +month: Integer
  }
  
  class Person {
    +id: Integer
    +last_name: String
    +first_name: String
    +middle_name: String
    +birth_date: Date
    +death_date: Date
    +description_html: Text
    +article_html: Text
    +image: ImageField
    __
    +__str__(): String
    +full_name: String
    +short_name: String
  }
}
Event   -->  Street : 0..1
Event   --   Person : M:N
Event   ..>  Keyword : M:N
Event   ..>  Book : M:N
Person  ..>  Profession : M:N
Person  ..>  Street : M:N
Person  ..>  Book : M:N
Person  ..>  Keyword : M:N
legend bottom
  ──> = ForeignKey
  ─ ─ = ManyToMany
  M:N = многие-ко-многим
  0..1 = опционально
endlegend
@enduml
```

<img width="1168" height="1715" alt="image" src="https://github.com/user-attachments/assets/cbe5e3c3-8844-41e5-bc12-36e7f3d06f49" />

## Предметная область
Краеведческий веб-ресурс Петроградского района СПб: персоны, события, улицы, профессии, книги, теги.

## Программный код (Django модели)

```
from django.db import models

# ========================================
# СПРАВОЧНЫЕ МОДЕЛИ (из UML)
# ========================================
class Street(models.Model):
    """Улицы Петроградского района"""
    name = models.CharField(max_length=200, verbose_name='Название')
    
    class Meta:
        ordering = ['name']
        verbose_name = 'Улица'
        verbose_name_plural = 'Улицы'
    
    def __str__(self):
        return self.name  # UML: +name: String

class Profession(models.Model):
    """Профессии персон"""
    name = models.CharField(max_length=200, verbose_name='Название')
    
    class Meta:
        ordering = ['name']
        verbose_name = 'Профессия'
        verbose_name_plural = 'Профессии'
    
    def __str__(self):
        return self.name

class Book(models.Model):
    """Литературные источники"""
    author = models.CharField(max_length=200, verbose_name='Автор')
    title = models.CharField(max_length=200, verbose_name='Название')
    url = models.URLField(max_length=1000, blank=True, verbose_name='Ссылка')
    image = models.ImageField(upload_to="books_images", blank=True, null=True, verbose_name='Обложка')
    
    class Meta:
        ordering = ['author', 'title']
        verbose_name = 'Книга'
        verbose_name_plural = 'Книги'
    
    def __str__(self):
        return f"{self.author} — {self.title}"

class Keyword(models.Model):
    """Ключевые слова (теги)"""
    keyword = models.CharField(max_length=100, unique=True, verbose_name='Тег')
    
    class Meta:
        ordering = ['keyword']
        verbose_name = 'Тег'
        verbose_name_plural = 'Теги'
    
    def __str__(self):
        return self.keyword

# ========================================
# ОСНОВНЫЕ МОДЕЛИ (из UML)
# ========================================
class Event(models.Model):
    """Исторические события"""
    title = models.CharField(max_length=255, verbose_name='Название')
    date = models.DateField(verbose_name='Дата события')
    
    # UML: Event --> Street : 0..1
    street = models.ForeignKey(
        Street, on_delete=models.SET_NULL, null=True, blank=True,
        related_name='events', verbose_name='Улица'
    )
    
    description_html = models.TextField(verbose_name='Статья')
    image = models.ImageField(upload_to="events_images", blank=True, null=True, verbose_name='Изображение')
    
    # UML: M:N связи
    persons = models.ManyToManyField('Person', through='Person_event', related_name='events', verbose_name='Персоны')
    keywords = models.ManyToManyField(Keyword, through='Event_keyword', related_name='events', verbose_name='Теги')
    books = models.ManyToManyField(Book, through='Event_book', related_name='event_books', verbose_name='Книги')
    
    class Meta:
        ordering = ['-date']
        verbose_name = 'Событие'
        verbose_name_plural = 'События'
    
    def __str__(self):
        return self.title
    
    @property
    def day(self):
        return self.date.day if self.date else None
    
    @property
    def month(self):
        return self.date.month if self.date else None

class Person(models.Model):
    """Выдающиеся личности"""
    last_name = models.CharField(max_length=100, verbose_name='Фамилия')
    first_name = models.CharField(max_length=100, verbose_name='Имя')
    middle_name = models.CharField(max_length=100, default='', blank=True, verbose_name='Отчество')
    birth_date = models.DateField(null=True, blank=True, verbose_name='Дата рождения')
    death_date = models.DateField(null=True, blank=True, verbose_name='Дата смерти')
    description_html = models.TextField(verbose_name='Краткое описание')
    article_html = models.TextField(max_length=10000, default='', blank=True, verbose_name='Статья')
    image = models.ImageField(upload_to="persons_images", null=True, blank=True, verbose_name='Фотография')
    
    # UML: M:N связи
    professions = models.ManyToManyField(Profession, through='Person_profession', related_name='persons', verbose_name='Профессии')
    streets = models.ManyToManyField(Street, through='Person_street', related_name='persons', verbose_name='Улицы')
    books = models.ManyToManyField(Book, through='Person_book', related_name='person_books', verbose_name='Книги')
    keywords = models.ManyToManyField(Keyword, through='Person_keyword', related_name='tagged_persons', verbose_name='Теги')
    
    class Meta:
        ordering = ['last_name', 'first_name']
        verbose_name = 'Персона'
        verbose_name_plural = 'Персоны'
    
    def __str__(self):
        parts = [self.last_name, self.first_name]
        if self.middle_name: parts.append(self.middle_name)
        return ' '.join(parts)
    
    @property
    def full_name(self):
        return str(self)
    
    @property
    def short_name(self):
        initials = self.first_name[0] + '.'
        if self.middle_name: initials += self.middle_name[0] + '.'
        return f"{self.last_name} {initials}"

# ========================================
# THROUGH-МОДЕЛИ (M:N связи из UML)
# ========================================
class Person_event(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    event = models.ForeignKey(Event, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['person', 'event']]

class Person_keyword(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    keyword = models.ForeignKey(Keyword, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['person', 'keyword']]

class Event_keyword(models.Model):
    event = models.ForeignKey(Event, on_delete=models.CASCADE)
    keyword = models.ForeignKey(Keyword, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['event', 'keyword']]

class Person_book(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    book = models.ForeignKey(Book, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['person', 'book']]

class Event_book(models.Model):
    event = models.ForeignKey(Event, on_delete=models.CASCADE)
    book = models.ForeignKey(Book, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['event', 'book']]

class Person_profession(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    profession = models.ForeignKey(Profession, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['person', 'profession']]

class Person_street(models.Model):
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    street = models.ForeignKey(Street, on_delete=models.CASCADE)
    class Meta:
        unique_together = [['person', 'street']]

```

## Процесс генерации UML → Код

```
ШАГ 1: UML класс Street {name: String} 
     ↓
     class Street(models.Model):
         name = models.CharField(...)

ШАГ 2: UML Event --> Street (0..1)
     ↓ 
     street = models.ForeignKey(Street, null=True)

ШАГ 3: UML Event -- Person (M:N)
     ↓
     persons = ManyToManyField(through='Person_event')
     + through модель Person_event

```

# 
