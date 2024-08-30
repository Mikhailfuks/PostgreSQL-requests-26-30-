Requests 26 
CREATE DATABASE my_database;

### 2. Создание таблицы пользователей


CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


### 3. Вставка данных в таблицу пользователей


INSERT INTO users (username, email, password) 
VALUES ('john_doe', 'john@example.com', 'securepassword');


### 4. Обновление данных пользователя


UPDATE users 
SET email = 'john.doe@example.com' 
WHERE username = 'john_doe';

### 5. Удаление пользователя


DELETE FROM users 
WHERE username = 'john_doe';


### 6. Выбор всех пользователей


SELECT * FROM users;

ChatGPT4 | Midjourney, [29.08.2024 14:29]
### 7. Выбор пользоватей с определенными условиями

ChatGPT4 | Midjourney, [29.08.2024 14:29]
SELECT * FROM users 
WHERE created_at >= '2023-01-01';

ChatGPT4 | Midjourney, [29.08.2024 14:29]
### 8. Создание таблицы продуктов


CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


### 9. Вставка продукта в таблицу продуктов


INSERT INTO products (name, price, description) 
VALUES ('Laptop', 999.99, 'High-performance laptop');

### 10. Обновление цены продукта


UPDATE products 
SET price = 899.99 
WHERE name = 'Laptop';


### 11. Удаление продукта


DELETE FROM products 
WHERE name = 'Laptop';


### 12. Получение всех продуктов


SELECT * FROM products;


### 13. Создание таблицы заказов


CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),
    product_id INT REFERENCES products(id),
    quantity INT NOT NULL,
    total DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


### 14. Вставка заказа


INSERT INTO orders (user_id, product_id, quantity, total) 
VALUES (1, 1, 2, 1999.98);


### 15. Обновление количества в заказе


UPDATE orders 
SET quantity = 3 
WHERE id = 1;


### 16. Удаление заказа


DELETE FROM orders 
WHERE id = 1;


### 17. Получение всех заказов

SELECT * FROM orders;


### 18. Получение заказов по пользователю


SELECT * FROM orders 
WHERE user_id = 1;


### 19. Создание таблицы для хранения категорий продуктов


CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

### 20. Вставка категории в таблицу


INSERT INTO categories (name) 
VALUES ('Electronics');


### 21. Создание связи между продуктами и категориями

ALTER TABLE products ADD COLUMN category_id INT REFERENCES categories(id);


### 22. Обновление продукта с его категорией


UPDATE products 
SET category_id = 1 
WHERE name = 'Laptop';

ChatGPT4 | Midjourney, [29.08.2024 14:29]
### 23. Получение всех продуктов с их категориями


SELECT p.name, c.name AS category_name 
FROM products p
JOIN categories c ON p.category_id = c.id;


### 24. Создание индекса для ускорения поиска


CREATE INDEX idx_username ON users(username);


### 25. Получение пользователей с сортировкой по дате создания


SELECT * FROM users 
ORDER BY created_at DESC;


### 26. Получение подсчета продуктов по категориям


SELECT c.name, COUNT(p.id) AS product_count 
FROM categories c
LEFT JOIN products p ON c.id = p.category_id
GROUP BY c.name;


### 27. Получение общей выручки от заказов


SELECT SUM(total) AS total_revenue FROM orders;


### 28. Получение средних цен продуктов


SELECT AVG(price) AS average_price FROM products

Requests 25 

### 29. Создание триггера для автоматического обновления времени последнего заказа


CREATE OR REPLACE FUNCTION update_last_order_time() 
RETURNS TRIGGER AS $$
BEGIN
    UPDATE users SET updated_at = CURRENT_TIMESTAMP WHERE id = NEW.user_id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER order_update_trigger 
AFTER INSERT ON orders 
FOR EACH ROW EXECUTE PROCEDURE update_last_order_time();


### 30. Получение пользователей без заказов


SELECT * FROM users 
WHERE id NOT IN (SELECT DISTINCT user_id FROM orders);


### 31. Получение всех продуктов с ценой выше заданного значения


SELECT * FROM products 
WHERE price > 500.00;

### 32. Объединение данных из двух таблиц


SELECT u.username, COUNT(o.id) AS order_count 
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.username;

### 33. Получение последних 5 заказов


SELECT * FROM orders 
ORDER BY created_at DESC LIMIT 5;


### 34. Создание представления для упрощенного доступа к данным


CREATE VIEW user_order_summary AS 
SELECT u.username, SUM(o.total) AS total_spent 
FROM users u 
JOIN orders o ON u.id = o.user_id 
GROUP BY u.username;


### 35. Получение данных из представления

SELECT * FROM user_order_summary;


### 36. Удаление данных из представления


DROP VIEW IF EXISTS user_order_summary;


### 37. Перемещение данных между таблицами

INSERT INTO archived_orders (user_id, product_id, quantity, total, created_at) 
SELECT user_id, product_id, quantity, total, created_at 
FROM orders 
WHERE created_at < '2022-01-01';


### 38. Создание уникального ограничения

ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);


### 39. Полное удаление данных из таблицы


TRUNCATE TABLE products;


### 40. Получение самого дорогого продукта


SELECT * FROM products 
ORDER BY price DESC LIMIT 1;


### 41. Получение всех пользователей с уникальными именами


SELECT DISTINCT username FROM users;


### 42. Обновление всех цен продуктов на 10%


UPDATE products 
SET price = price * 1.10;


### 43. Получение пользователей, у которых больше 5 заказов


SELECT u.username 
FROM users u
JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 5;


### 44. Удаление всех заказы от определенного пользователя

DELETE FROM orders 
WHERE user_id = 1;


### 45. Получение минимальной и максимальной цены продуктов


SELECT MIN(price) AS min_price, MAX(price) AS max_price FROM products;


### 46. Переименование столбца


ALTER TABLE products RENAME COLUMN description TO details;


### 47. Получение количества пользователей, созданных в этом месяце


SELECT COUNT(*) FROM users 
WHERE created_at >= date_trunc('month', CURRENT_DATE);


### 48. Получение всех заказов для указанного продукта


SELECT * FROM orders 
WHERE product_id = 1;

### 49. Получение всех пользователей, зарегистрированных за последний месяц

SELECT * FROM users 
WHERE created_at >= NOW() - INTERVAL '1 month';


### 50. Получение суммарного количества проданных товаров по пользователям


SELECT u.username, SUM(o.quantity) AS total_quantity 
FROM users u
JOIN orders o ON u.id = o.user_id
GROUP BY u.username;

Requests 26 
### 51. Создание обертки для хранения информации о продукте и категории

### 45. Создание триггера для обновления времени последнего изменения пользователя


CREATE OR REPLACE FUNCTION update_user_timestamp() 
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER user_update_trigger 
BEFORE UPDATE ON users 
FOR EACH ROW EXECUTE PROCEDURE update_user_timestamp();


### 46. Запрос на множественное обновление статуса заказов


UPDATE orders 
SET status = 'shipped' 
WHERE created_at < NOW() - INTERVAL '7 days' AND status = 'pending';


### 47. Получение всех заказов, у которых превышена дата доставки


SELECT * FROM orders 
WHERE delivery_date < CURRENT_DATE;


### 48. Получение всех пользователей, зарегистрированных за последний год


SELECT * FROM users 
WHERE created_at >= NOW() - INTERVAL '1 year';


### 49. Создание представления для заказов и пользователей

ChatGPT4 | Midjourney, 
CREATE VIEW user_orders AS 
SELECT u.username, o.total 
FROM users u 
JOIN orders o ON u.id = o.user_id;


### 50. Получение данных из представления


SELECT * FROM user_orders;


### 51. Создание таблицы на случай угроз в безопасности


CREATE TABLE security_incidents (
    id SERIAL PRIMARY KEY,
    incident_type VARCHAR(100),
    severity INT,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


### 52. Вставка инцидента безопасности


INSERT INTO security_incidents (incident_type, severity, description) 
VALUES ('Unauthorized Access', 5, 'Attempt to access restricted area.');


### 53. Удаление инцидента безопасности


DELETE FROM security_incidents 
WHERE id = 1;


### 54. Получение всех инцидентов с определенной степенью серьезности

 
SELECT * FROM security_incidents 
WHERE severity >= 4;


### 55. Получение записей инцидентов, произошедших в последние 30 дней

ChatGPT4 | Midjourney, 
SELECT * FROM security_incidents 
WHERE created_at >= NOW() - INTERVAL '30 days';


### 56. Обновление описания инцидента


UPDATE security_incidents 
SET description = 'Updated description for unauthorized access attempt.' 
WHERE id = 1;


### 57. Создание таблицы для хранения авторизации пользователей


CREATE TABLE user_authentications (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),
    login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


### 58. Вставка события авторизации


INSERT INTO user_authentications (user_id) 
VALUES (1);


### 59. Получение всех событий авторизации по пользователю


SELECT * FROM user_authentications 
WHERE user_id = 1;


### 60. Удаление всех событий авторизации старше 30 дней


DELETE FROM user_authentications 
WHERE login_time < NOW() - INTERVAL '30 days';


### 61. Получение общего количества активных пользователей


SELECT COUNT(*) 
FROM users 
WHERE is_active = true;


### 62. Добавление ограничения на количество символов в поле продукт


ALTER TABLE products 
ALTER COLUMN name SET NOT NULL;


### 63. Создание таблицы для хранения функций пользователей


CREATE TABLE user_roles (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),
    role VARCHAR(50)
);


### 64. Вставка роли пользователя


INSERT INTO user_roles (user_id, role) 
VALUES (1, 'admin');













