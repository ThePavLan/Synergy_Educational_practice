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
