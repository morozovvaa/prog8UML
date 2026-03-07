# Диаграмма вариантов использования (use case)

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
