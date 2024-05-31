# Code Optimization

## Javascript Snipet

Problema: Inefficient loop handling and excessive DOM manipulation
```
function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = "";
  for (let i = 0; i < items.length; i++) {
    let listItem = document.createElement("li");
    listItem.innerHTML = items[i];
    list.appendChild(listItem);
  }
}
```
Solucion: Construir una cadena aparrtir de las iteraciones y asignarla a elemento itemList
```
function updateList(items) {
  let list = document.getElementById("itemList");
  let html = "";
  for (let i = 0; i < items.length; i++) {
    html += "<li>" + items[i] + "</li>";
  }
  list.innerHTML = html;
}
```
## Java Snnipet

Problema: Redundant database queries
```
public class ProductLoader {
    public List<Product> loadProducts() {
        List<Product> products = new ArrayList<>();
        for (int id = 1; id <= 100; id++) {
            products.add(database.getProductById(id));
        }
        return products;
    }
}
```
Solucion: Utilizar una consulta SQL con un rango de ID
```
public class ProductLoader {
    public List<Product> loadProducts() {
        return database.getProductsInRange(1, 100);
    }
}
```
## C# Snnipet

Problema: Unnecessary computations in data processing
```
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>();
    foreach (var d in data) {
        if (d % 2 == 0) {
            result.Add(d * 2);
        } else {
            result.Add(d * 3);
        }
    }
    return result;
}
```
Solucion: Utilizar el método ConvertAll para generar la lista resultante directamente
```
public List<int> ProcessData(List<int> data) {
    return data.ConvertAll(d => d % 2 == 0 ? d * 2 : d * 3);
}
```
