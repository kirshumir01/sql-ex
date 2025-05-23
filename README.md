# Решения задач по SQL с sql-ex.ru

## Описание

В репозитории представлены мои решения практических задач по SQL-запросам в базы данных.

Запросы адаптированы для Microsoft SQL Server 2019 (15.0)

### Схема № 1. Компьютерная фирма

#### Описание схемы
Схема БД состоит из четырех таблиц:
- Product(maker, model, type)
- PC(code, model, speed, ram, hd, cd, price)
- Laptop(code, model, speed, ram, hd, price, screen)
- Printer(code, model, color, type, price)

Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер).

Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов.

В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price (в долларах).

Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах).

В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.

#### ER-диаграмма

![](https://github.com/kirshumir01/sql-ex/blob/main/computer-firm.png)

#### Задания и решения

1. Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd

```sql
SELECT model, speed, hd
FROM PC
WHERE price < 500
```

2. Найдите производителей принтеров. Вывести: maker

```sql
SELECT DISTINCT maker
FROM Product
WHERE type = 'Printer'
```

3. Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.

```sql
SELECT model, ram, screen
FROM Laptop
WHERE price > 1000
```

4. Найдите все записи таблицы Printer для цветных принтеров.

```sql
SELECT *
FROM Printer
WHERE color = 'y'
```

5. Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.

```sql
SELECT model, speed, hd
FROM PC
WHERE (cd = '12x' OR cd = '24x') AND price < 600
```

6. Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.

```sql
SELECT DISTINCT Product.maker, Laptop.speed
FROM Product, Laptop
WHERE Laptop.hd >= 10 AND Product.model = Laptop.model
```