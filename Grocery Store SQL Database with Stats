-- SQL code below creates a grocery store database with defined parameters
CREATE TABLE store (id INTEGER PRIMARY KEY, item TEXT, section TEXT, price INTEGER, favorability INTEGER);

-- Inputting values into the grocery store database based on defined parameters such as: id, item, price, and favorability
INSERT INTO store VALUES (1, "coffee", "beverages", 3.99, 85);
INSERT INTO store VALUES (2, "soda", "beverages", 2.99, 75); 
INSERT INTO store VALUES (3, "dental floss", "health", 0.99, 90); 
INSERT INTO store VALUES (4, "milk", "dairy", 2.99, 80);
INSERT INTO store VALUES (5, "yogurt", "dairy", 2.99, 70);
INSERT INTO store VALUES (6, "waffles", "frozen foods", 2.99, 30);
INSERT INTO store VALUES (7, "vegatables", "frozen foods", 4.99, 75);
INSERT INTO store VALUES (8, "toothbrush", "health", 1.99, 98); 
INSERT INTO store VALUES (9, "laundry detergent", "cleaning", 6.99, 99);
INSERT INTO store VALUES (10, "dishwashing detergent", "cleaning", 4.99, 98);
INSERT INTO store VALUES (11, "paper towels", "paper goods", 2.99, 80);
INSERT INTO store VALUES (12, "bread", "bakery", 5.99, 90);
INSERT INTO store VALUES (13, "hand soap", "health", 4.99, 95);
INSERT INTO store VALUES (14, "shampoo", "health", 4.99, 95);
INSERT INTO store VALUES (15, "butter", "dairy", 3.99, 85);

-- Entire database is displayed and ordered by price in descending order. 
SELECT * 
  FROM store
ORDER BY price desc; 

-- Average price of items in the health section
SELECT AVG(price)
  FROM store
WHERE section = 'health'; 

-- 10 most popular items within the grocery store database
SELECT item, price, favorability
  FROM store
ORDER BY popularity desc
LIMIT 10; 
