# LAB 1: TRANSACTION PSEUDOCODE

## 1. Initialize Transaction
```sh
BEGIN TRANSACTION;
```

## 2. Set Order Number
```sh
SET orderNumber = (SELECT MAX(orderNumber) + 1 FROM orders);
```

## 3. Insert Order
```sh
INSERT INTO orders (orderNumber, orderDate, requiredDate, shippedDate, status, customerNumber)
VALUES (orderNumber, CURRENT_DATE, CURRENT_DATE + 3, CURRENT_DATE + 2, "In Process", 145);
```

## 4. Insert Order Details - Product 1
```sh
SAVEPOINT before_product_1;

INSERT INTO orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)
VALUES (orderNumber, "S18_1749", 2724, 136, 1);
```

## 5. Update Product Stock - Product 1
```sh
SET quantityInStock = (SELECT quantityInStock FROM products WHERE productCode = "S18_1749");

UPDATE products SET quantityInStock = quantityInStock - 2724 WHERE productCode = "S18_1749";
```

## 6. Insert Order Details - Product 2
```sh
SAVEPOINT before_product_2;

INSERT INTO orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)
VALUES (orderNumber, "S18_2248", 540, 55.09, 2);
```

## 7. Update Product Stock - Product 2
```sh
SET quantityInStock = (SELECT quantityInStock FROM products WHERE productCode = "S18_2248");

UPDATE products SET quantityInStock = quantityInStock - 540 WHERE productCode = "S18_2248";
```

## 8. Rollback Product 2
```sh
ROLLBACK TO SAVEPOINT before_product_2;
```

## 9. Insert Order Details - Product 3
```sh
SAVEPOINT before_product_3;

INSERT INTO orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)
VALUES (orderNumber, "S12_1099", 68, 95.34, 3);
```

## 10. Update Product Stock - Product 3
```sh
SET quantityInStock = (SELECT quantityInStock FROM products WHERE productCode = "S12_1099");

UPDATE products SET quantityInStock = quantityInStock - 68 WHERE productCode = "S12_1099";
```

## 11. Insert Payment
```sh
INSERT INTO payments (customerNumber, checkNumber, paymentDate, amount)
VALUES (145, "JM555210", CURRENT_DATE, 12000);
```

## 12. Commit Transaction
```sh
COMMIT;
```

## 13. Verify Transaction
```sh
SELECT * FROM orderdetails WHERE orderNumber = 10426 ORDER BY orderLineNumber;
```
