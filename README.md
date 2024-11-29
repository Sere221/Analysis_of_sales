# Analysis_of_sales

### Задача
Проанализировать данные о продажах мясных продукции в 2024 году, чтобы выявить ключевые тренды и рекомендации для увеличения дохода.

### Данные
1. `sales` (`id`, `product_id`, `customer_id`, `sale_date`, `amount`, `quantity`)
2. `products` (`id`, `name`, `category`, `price`)
3. `customers` (`id`, `name`, `registration_date`, `country`)

### Инструменты
SQL Workbench, SQL

### План анализа
1. **Общее количество продаж**
2. **Топ 5-товаров**
3. **Анализ клиентов**
4. **Выручка по категориям**
5. **Средний чек**

### Анализ данных
1. **Общее количество продаж** Надо записать SQL-запрос, который возвращает общее количество продаж и общую сумму выручки за последний месяц.
```sql
SELECT SUM(amount) AS sum_amount
FROM sales
WHERE MONTH(sale_date) = 11 AND YEAR(sale_date) = 2024
```
**Результаты:**

![image](https://github.com/user-attachments/assets/d1d09188-57af-43b6-a870-9d91df9d27dd)

**Общая сумма выручки продаж за последний месяц ** - 166191586.00 ₽

2. **Топ 5-товаров** Надо записать SQL-запрос, который находит 5 товаров с наибольшими объемами продаж (по количеству проданных единиц) за последний квартал.
```sql
SELECT p.name, SUM(s.quantity) AS total_quantity, SUM(s.amount) AS total_amount 
FROM products AS p
RIGHT JOIN sales AS s ON p.id = s.product_id
WHERE MONTH(s.sale_date) BETWEEN 9 AND 12 AND YEAR(s.sale_date) = 2024
GROUP BY p.name
ORDER BY MAX(s.quantity) DESC, total_amount DESC LIMIT 5;
```
**Результаты:**

В этом квартале **ТОП-5** продуктов:

![image](https://github.com/user-attachments/assets/6f085c0f-a030-4196-864b-692657643cc0)

Попробовать улучшить качество и предложение этих продуктов, чтобы удержать свою аудиторию и привлечь новых покупателей.



3. **Анализ клиентов** Надо записать SQL-запрос, который возвращает количество клиентов, зарегистрированных в каждом месяце, и общее количество покупок, сделанных этими клиентами.
```sql
SELECT DATE_FORMAT(s.sale_date, '%Y-%m') AS month, COUNT(DISTINCT c.id) AS clients, COUNT(s.amount) AS count_amount
FROM customers AS c
RIGHT JOIN sales AS s ON c.id = s.customer_id
GROUP BY month
ORDER BY month
```
**Результаты:**

![image](https://github.com/user-attachments/assets/2122cd85-a22e-41c6-84d5-4e4fcc0328be)

Наибольшее число клиентов регистрируется в мае и октябре — 31. Однако разница с другими месяцами незначительна, за исключением января, где число клиентов составляет 17.

Что касается покупок, то в феврале их количество почти в два раза больше, чем в остальные месяцы. 

4. **Выручка по категориям** Надо записать SQL-запрос, который возвращает количество клиентов, зарегистрированных в каждом месяце, и общее количество покупок, сделанных этими клиентами.
```sql
SELECT p.category, SUM(s.amount) AS revanue
FROM products as p
RIGHT JOIN sales AS s ON p.id = s.product_id
GROUP BY p.category
```
**Результаты:**
![image](https://github.com/user-attachments/assets/467a9ca0-b1a5-4c0b-8b1c-69bb1c24f373)

5. **Средний чек** Надо записать SQL-запрос, который вычисляет средний чек (средняя сумма покупки) за последний месяц.
```sql
SELECT CAST(AVG(amount) AS DECIMAL(10, 2)) AS avg_check FROM sales
WHERE MONTH(sale_date) = 11 AND YEAR(sale_date) = 2024
```
**Результаты:**
![image](https://github.com/user-attachments/assets/593532a1-bea4-49af-a86e-bc1a271c18bc)



