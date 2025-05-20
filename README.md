# Система бронирования туров  
**Технологическая практика по направлению "Искусственный интеллект и большие данные"**  

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

2. Настройте PostgreSQL
Убедитесь, что PostgreSQL запущен.
Создайте базу данных tourism_db.
Выполните SQL-скрипты из файла case.sql из [Кейс-Задачи №3]([https://hello-sunil.in](https://github.com/ThePavLan/Synergy_Educational_practice/blob/main/Кейс-задача%20№%203)) для создания таблиц и заполнения данных.

