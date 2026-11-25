# Entregable: Consultas SQL y Validación de Datos

## 1. Modelo de Base de Datos (DDL)
Scripts utilizados en Oracle Database para la creación de las tablas `customerss`, `orders` y `order_items`:

```sql
CREATE TABLE customerss (
    customer_id NUMBER NOT NULL,
    full_name   VARCHAR2(150) NOT NULL,
    email       VARCHAR2(150) NOT NULL,
    CONSTRAINT PK_customers PRIMARY KEY (customer_id),
    CONSTRAINT UQ_customers_email UNIQUE (email)
);

CREATE TABLE orders (
    order_id     NUMBER NOT NULL,
    customer_id  NUMBER NOT NULL,
    order_date   DATE DEFAULT SYSDATE NOT NULL,
    total_amount NUMBER(10, 2) NOT NULL,
    status       VARCHAR2(50) NOT NULL,
    CONSTRAINT PK_orders PRIMARY KEY (order_id),
    CONSTRAINT FK_orders_customer FOREIGN KEY (customer_id) REFERENCES customerss(customer_id)
);

CREATE TABLE order_items (
    item_id      NUMBER NOT NULL,
    order_id     NUMBER NOT NULL,
    product_name VARCHAR2(150) NOT NULL,
    quantity     NUMBER NOT NULL,
    unit_price   NUMBER(10, 2) NOT NULL,
    CONSTRAINT PK_order_items PRIMARY KEY (item_id),
    CONSTRAINT FK_order_items_order FOREIGN KEY (order_id) REFERENCES orders(order_id)
);
```

---

## 2. Datos de Prueba (DML)
Inserción de registros iniciales y catálogo de productos para validación:

```sql
-- Clientes
INSERT INTO customerss (customer_id, full_name, email) VALUES (1, 'Luna Andrade', 'luna.andrade@email.com');
INSERT INTO customerss (customer_id, full_name, email) VALUES (2, 'Monica Coloma', 'Monica.Coloma@email.com');
INSERT INTO customerss (customer_id, full_name, email) VALUES (3, 'Jhon Gutierrez', 'jhon.gutierrez@email.com');

-- Órdenes
INSERT INTO orders (order_id, customer_id, order_date, total_amount, status) VALUES (1, 1, SYSDATE - 5, 250.00, 'COMPLETED');
INSERT INTO orders (order_id, customer_id, order_date, total_amount, status) VALUES (2, 1, SYSDATE - 2, 80.50, 'COMPLETED');
INSERT INTO orders (order_id, customer_id, order_date, total_amount, status) VALUES (3, 2, SYSDATE - 1, 420.00, 'COMPLETED');
INSERT INTO orders (order_id, customer_id, order_date, total_amount, status) VALUES (4, 3, SYSDATE, 150.00, 'PENDING');

-- Ítems de órdenes

INSERT INTO order_items (item_id, order_id, product_name, quantity, unit_price) VALUES (6, 1, 'Sauce labs Onesie', 1, 7.99);
INSERT INTO order_items (item_id, order_id, product_name, quantity, unit_price) VALUES (7, 2, 'Test.allthethings t-shirt (red)', 1, 15.99);
INSERT INTO order_items (item_id, order_id, product_name, quantity, unit_price) VALUES (8, 2, 'Sauce labs bolt t-shirt', 1, 15.99);
INSERT INTO order_items (item_id, order_id, product_name, quantity, unit_price) VALUES (9, 3, 'Sauce labs fleece jacket', 1, 49.99);
INSERT INTO order_items (item_id, order_id, product_name, quantity, unit_price) VALUES (10, 4, 'Sauce labs backpack', 1, 29.99);
INSERT INTO order_items (item_id, order_id, product_name, quantity, unit_price) VALUES (11, 4, 'Sauce labs bike light', 1, 9.99);

COMMIT;
```

---

## 3. Consultas Solicitadas

### Query 1: Órdenes completadas mayores a $100
Lista el nombre del cliente, el ID de la orden y el monto total de las órdenes con estado `'COMPLETED'` que superan los $100, ordenadas de mayor a menor.

```sql
SELECT 
    c.full_name,
    o.order_id,
    o.total_amount
FROM customerss c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE o.status = 'COMPLETED'
  AND o.total_amount > 100
ORDER BY o.total_amount DESC;
```

### Query 2: Detección de inconsistencias (Órdenes sin ítems)
Identifica todas las órdenes registradas en `orders` que no tienen ningún ítem asociado en `order_items`.

```sql
SELECT 
    o.order_id,
    o.customer_id,
    o.order_date,
    o.total_amount,
    o.status
FROM orders o
LEFT JOIN order_items oi ON o.order_id = oi.order_id
WHERE oi.item_id IS NULL;
```
