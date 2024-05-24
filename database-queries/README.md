# 1. SQL Query Optimization
## 1.1 Orders Query
> Problema:
```
SELECT Orders.OrderID, SUM(OrderDetails.Quantity * OrderDetails.UnitPrice) AS TotalPrice
FROM Orders
JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
WHERE OrderDetails.Quantity > 10
GROUP BY Orders.OrderID;
```
> Solucion: Agregar los indices para `OrderID` por ser una columna de identificador, `Quantity` por ser una condicion de la consulta.
```
CREATE INDEX idx_orders_orderid ON Orders(OrderID);
CREATE INDEX idx_orders_quantity ON Orders(Quantity);
```

## 1.2 Customer Query
> Problema:
```
SELECT CustomerName FROM Customers WHERE City = 'London' ORDER BY CustomerName;
```
> Solucion: Agregar los indices para `City` por ser una condicion de la consulta y `CustomerName` por ser un ordenamiento de la consulta.
```
CREATE INDEX idx_customers_city ON Customers(City);
CREATE INDEX idx_customers_customername ON Customers(CustomerName);
```

# 2. NoSQL Query Implementation
## 2.1 User Posts Query
> Problema: 
```
db.posts
  .find({ status: "active" }, { title: 1, likes: 1 })
  .sort({ likes: -1 });
```
> Solucion: Agregar los indices para `likes` y para `status`
```
db.posts.createIndex({ status: 1 });
db.posts.createIndex({ likes: -1 });
```

## 2.2 User Data Aggregation
> Problema: 
```
db.users.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$location", totalUsers: { $sum: 1 } } },
]);
```

> Solucion: Agregar los indices para `status`
```
db.users.createIndex({ status: 1 });
```
