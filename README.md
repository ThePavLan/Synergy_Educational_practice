### **Проектно-технологическая практика по направлению "Искусственный интеллект и большие данные"**
### **Университет "Синергия"** 
### 2 семестр

# Система бронирования туров  
 

## Описание  
Проект реализует REST API для системы бронирования туров на основе Flask (Python) и PostgreSQL.  
- **Функционал:**  
  - Получение списка доступных туров.  
  - Бронирование туров (через POST-запросы).  
  - Использование PostgreSQL для хранения данных.  

## Технологии  
- **Python 3.9+** — язык программирования.  
- **Flask** — фреймворк для создания веб-приложения.  
- **PostgreSQL** — система управления базой данных.  
- **SQLAlchemy** — ORM для работы с БД.  
- **HTML/CSS/JS** — простой интерфейс для тестирования API.  
- **Jupyter Notebook** — для тестирования кода (по желанию).  

## Структура базы данных  
База данных `tourism_db` включает следующие таблицы:  
Countries, Cities, Tours, Bookings, Clients, TourTypes.

### Схема базы данных:
- Countries (CountryID, CountryName)
- Cities (CityID, CityName, CountryID) → связь с Countries
- TourTypes (TypeID, TypeName)
- Tours (TourID, TourName, CityID, TypeID) → связь с Cities и TourTypes
- Clients (ClientID, FirstName, LastName, Email)
- Bookings (BookingID, ClientID, TourID, BookingDate, Status) → связь с Clients и Tours

![case3 pgerd](https://github.com/user-attachments/assets/f87614fb-4d18-4dc7-8e2d-7f2502e75cc0)


## Как запустить проект  
### 1. Установите зависимости  
```bash
pip install flask flask-sqlalchemy flask-cors psycopg2-binary
```

2. Настройте PostgreSQL
Убедитесь, что PostgreSQL запущен.
Создайте базу данных tourism_db.
Выполните SQL-скрипты из файла case3.sql из [Кейс-Задачи №3](https://github.com/ThePavLan/Synergy_Educational_practice/blob/main/Кейс-задача%20№%203) для создания таблиц и заполнения данных.

3. Запустите Flask-сервер
Вариант 1: Через Jupyter Notebook
 - Вставьте код из app.py в ячейку Jupyter.
 - Выполните ячейку.
 - API будет доступен по адресу: http://localhost:5000/tours

Вариант 2: Через терминал
 - Сохраните код в файл app.py.
```
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_cors import CORS
import threading
import time  # Добавленный импорт time

# Создайте Flask-приложение
app = Flask(__name__)
CORS(app)

# Настройка PostgreSQL
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:postgres@localhost/tourism_db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# Инициализация SQLAlchemy
db = SQLAlchemy(app)

# Модель для таблицы Tours
class Tour(db.Model):
    __tablename__ = 'tours' #имя таблицы
    TourID = db.Column(db.Integer, primary_key=True, name='tourid')  # Сопоставление с tourid
    TourName = db.Column(db.String(255), name='tourname')            # Сопоставление с tourname
    CityID = db.Column(db.Integer, db.ForeignKey('cities.cityid'), name='cityid')  # cities.cityid
    TypeID = db.Column(db.Integer, db.ForeignKey('tourtypes.typeid'), name='typeid')  # tourtypes.typeid
    Price = db.Column(db.Numeric, name='price')
    DurationDays = db.Column(db.Integer, name='durationdays')

class City(db.Model):
    __tablename__ = 'cities'
    CityID = db.Column(db.Integer, primary_key=True, name='cityid')
    CityName = db.Column(db.String(100), name='cityname')
    CountryID = db.Column(db.Integer, db.ForeignKey('countries.countryid'), name='countryid')

class TourType(db.Model):
    __tablename__ = 'tourtypes'
    TypeID = db.Column(db.Integer, primary_key=True, name='typeid')
    TypeName = db.Column(db.String(100), name='typename')

# Создание таблиц в контексте приложения
with app.app_context():
    db.create_all()

#проверка перед запуском сервера
with app.app_context():
    try:
        db.session.query(Tour).first()  # Проверяем подключение
        print("Таблица tours доступна")
    except Exception as e:
        print("Ошибка подключения к БД:", str(e))

#получаю Таблица tours доступна

# Маршрут для получения данных
@app.route('/tours', methods=['GET'])
def get_tours():
    with app.app_context():  # Активация контекста
        try:
            tours = Tour.query.all()
            return jsonify([{'id': t.TourID, 'name': t.TourName, 'price': float(t.Price)} for t in tours])
        except Exception as e:
            return jsonify({"error": str(e)}), 500

# Функция запуска сервера в отдельном потоке
def run_flask():
    app.run(debug=True, port=5000, use_reloader=False) # Отключен перезагрузчик
 
# Запуск сервера в отдельном потоке
thread = threading.Thread(target=run_flask)
thread.daemon = True  # Поток завершится при закрытии Jupyter
thread.start()

# Ждём, пока сервер запустится 2 секунды
time.sleep(2)

#получаю следующее
# * Serving Flask app '__main__'
# * Debug mode: on
#WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
# * Running on http://127.0.0.1:5000
#Press CTRL+C to quit

with app.app_context():
    tours = Tour.query.all()
    print(tours)  # Должно вывести пустой список, если таблица пуста, в моем случае выходит [<Tour 1>, <Tour 2>, <Tour 3>, <Tour 4>, <Tour 5>]

import requests

# Делаем запрос к API
response = requests.get('http://localhost:5000/tours')
print(response.json())
```
  
 - Запустите сервер:
```
python app.py
```
 - Откройте браузер и перейдите по ссылке:
```
http://localhost:5000/tours
```
![по адресу localhost 5000](https://github.com/user-attachments/assets/64ce0c57-5825-471f-8dae-ef231fec92f6)


Проверьте работу API
Получение списка туров:
```
curl http://localhost:5000/tours
```

Пример ответа API
```
[
  {"id": 1, "name": "Обзорная экскурсия по Москве", "price": 15000.0},
  {"id": 2, "name": "Деловой тур в Берлин", "price": 16000.0},
  {"id": 3, "name": "Тематический тур в Париж", "price": 17000.0}
]
```

Объяснение выбора технологий
В рамках кейс-задачи №4 требовалось использовать Delphi 10.2, IIS и MS SQL Server. Однако для упрощения разработки и соответствия требованиям анализа данных и машинного обучения (ваше направление) был выбран стек Python + Flask + PostgreSQL. Это позволяет:

Свободное использование (бесплатные технологии).
Простоту освоения (низкий порог входа для новичков).
Гибкость (интеграция с ML-библиотеками, например, scikit-learn или TensorFlow).


Итоговая оценка:
Текущий статус: Реализован минимальный функционал.
Главные проблемы: Отсутствие безопасности, неполная функциональность, низкая сопровождаемость.
Приоритетные улучшения:
Добавить аутентификацию (JWT).
Реализовать интерфейс на React.
Настроить Docker и CI/CD.
