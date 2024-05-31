# iron-hack-lab10

## 1: Code Snippets


``` javascript

// Inefficient loop handling and excessive DOM manipulation
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

``` javascript

function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = "";
  
  let fragment = document.createDocumentFragment();
  
  items.forEach(item => {
    let listItem = document.createElement("li");
    listItem.innerHTML = item;
    fragment.appendChild(listItem);
  });
  
  list.appendChild(fragment);
}


```

``` javascript

function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = items.map(item => `<li>${item}</li>`).join('');
}


```


Explicación:

Versión 1: Utiliza forEach en lugar de un bucle for tradicional. Esto hace que el código sea más limpio y fácil de leer, manteniendo la eficiencia al agregar elementos al DocumentFragment antes de anexarlo al DOM.

Versión 2: Convierte la lista de elementos en una cadena de HTML usando map y join. Esto reduce aún más la manipulación del DOM al hacer una sola actualización a innerHTML.

Ambas versiones optimizan el bucle y la manipulación del DOM, pero la segunda es la más eficiente en términos de rendimiento y simplicidad, siempre y cuando no necesites añadir atributos o eventos a los elementos li



## 2: Code Snippets

``` java

// Redundant database queries
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

``` java

public List<Product> loadProducts() {
        return database.getAllProducts();
}


public List<Product> getAllProducts() {
        List<Product> products = new ArrayList<>();
        String query = "SELECT * FROM products";
        
        try (Connection connection = getConnection();
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery(query)) {
            
            while (resultSet.next()) {
                Product product = new Product();
                product.setId(resultSet.getInt("id"));
                product.setName(resultSet.getString("name"));
                product.setPrice(resultSet.getDouble("price"));
                // establecer otros campos del producto
                
                products.add(product);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        
        return products;
    }

```

Explicación:

 * Unificar las Consultas: En lugar de llamar a database.getProductById(id) 100 veces, llamamos a un nuevo método database.getAllProducts() que obtiene todos los productos de una sola vez.

 * Mejora de Rendimiento: Al reducir el número de consultas a la base de datos de 100 a 1, se mejora significativamente el rendimiento, especialmente si la base de datos está en una red remota o tiene un tiempo de respuesta alto.


## 3: Code Snippets

``` c#
// Unnecessary computations in data processing
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

``` c#

public List<int> ProcessData(List<int> data) {
    return data.Select(d => d % 2 == 0 ? d * 2 : d * 3).ToList();
}


```

Explicación:

Uso de LINQ Select: El método Select de LINQ aplica una función a cada elemento de la lista y devuelve una nueva lista con los resultados. Esto elimina la necesidad de inicializar una lista y añadir elementos manualmente.
Expresión Condicional: La expresión d % 2 == 0 ? d * 2 : d * 3 utiliza el operador ternario para determinar si el número es par o impar y aplicar la transformación correspondiente, simplificando el código.

