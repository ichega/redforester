# redforester - RedForester API module for Python  
  
**Features**  
  
- Python 3.7
- Async (using aiohttp)  
- Lightweight  
- Fast  
  
## Installing  
  
1. Install with PIP `pip install git+https://github.com/ichega/redforester.git`  

or 

`pip install redforester`

## Roadmap
Реализовать следующий набор классов:
- [x] Session - создание сессии для работы с RedForester
- [x] Request - получение данных из RedForester
- [x] Action - совершение действий в RedForester
- [x] Sequense - совершение сразу множества действий в RedForester 
- [x] User - работа с данными о пользователе
- [x] Map - работа с данными о карте
- [x] Node - работа с данными об узле
- [ ] Event - информации о событии на карте
- [ ] Listener - отслеживание событий на карте (создание, удаление, модификация узлов и веток)
  
##   User guide 
### 1. Создать сессию 

```python  
import redforester  
  
session = redforester.Session(username="username", password="password")
# Можно вместо пароля передать его md5 хеш
# session = redforester.Session(username="username", password="5F4DCC3B5AA765D61D8327DEB882CF99", use_md5=True)
```
> **Замечание:** Можно также подключится к тестовому серверу
```python  
import redforester  
from redforester.config import DEVELOPMENT_CONFIG  
  
session = redforester.Session(username="", password="", config=DEVELOPMENT_CONFIG)
```
### 2. Получение информации из RedForester
Для этого необходимо использовать класс `Request`:
Пример, получение данных о пользователе:
#### 2.1. Синхронный вариант - метод `send`
```python
import redforester  
  
session = redforester.Session(username="username", password="password")  
  
request = redforester.Request(session=session, method="GET", url="/api/user")  
response = request.send()  
print(response)
```
#### 2.2. Асинхронный вариант - метод `async_send`
```python
import redforester  
import asyncio  
  
session = redforester.Session(username="username", password="password")  
request = redforester.Request(session=session, method="GET", url="/api/user")  
loop = asyncio.get_event_loop()  
response = loop.run_until_complete(request.async_send())  
print(response)
```

### 3. Совершение действий в RedForester 
#### 3.1. Если нужно сделать просто запрос к рф ручками :)
##### 3.1.1. Action
Для этого используется класс `Action`.  Аналогично `Request`, он имеет синхронный и асинхронный вариант.
Пример, пусть нужно изменить аватар пользователя:
**Синхронный вариант**:
```python
import redforester  
  
session = redforester.Session(username="username", password="password")  
data = {  
    "avatar": "https://img.icons8.com/color/48/000000/administrator-male.png",  
}  
action = redforester.Action(session, method="PATCH", url="/api/user", data=data)  
response = action.send()  
print(response)
```
##### 3.1.2.  Sequence
В документации RedForester рекомендуется использовать batch запросы для совершения нескольких последовательных действий. Для этих целей был разработан класс `Sequence`, который позволяет легко организовать несколько `Action` в batch-запрос. 

Конструктор `Sequence` принимает два аргумента:
- Сессию
- Кортеж из `Action`

Пример, пусть нужно изменить аватар пользователя и создать новую карту:
```python
import redforester  
  
session = redforester.Session(username="username", password="password")  
avatar_info = {  
    "avatar": "https://img.icons8.com/color/48/000000/administrator-male.png",  
}  
map_info = {  
    "name": redforester  
}  
action1 = redforester.Action(session, "PATCH", "/api/user", avatar_info)  
action2 = redforester.Action(session, "POST", "/api/maps", map_info)  
sequence = redforester.Sequence(session, (action1, action2))  
response = sequence.send()  
print(response)
```
#### 3.2  User

#### 3.3  Maps

#### 3.4  Node

#### 3.5  Event

#### 3.6  Listener
