---
title: Actividad Robin -3
---

# Robin-BaseDatos

# Actividad 3

# Taller de Consultas SQL por Niveles

## Levanto mi Servidor con express:

- Creo una carpeta express y mi archivo js:

```
mkdir express
touch index.js
```

- inicializo mi proyecto:

```
npm init -y
```

- instalo mis dependencias:
```
npm install express mysql2
```

- En mi archivo index.js levanto mi servidor y hago la conexion a mi BD:

- index.js
```
// express

const express = require('express')
const app = express()

// myQSL

const mysql = require('mysql2');

// las variables de entorno las debes remplazar con tus datos

const db = mysql.createPool({
    host: process.env.DB_HOST,             // servidor MySQL
    user: process.env.DB_USER,             // usuario
    password: process.env.DB_PASSWORD,     // contraseña
    database: process.env.DB_NAME,         // base de datos
    waitForConnections: true,              // si no hay conexiones, esperar
    connectionLimit: 40,                   // máximo de conexiones simultáneas
    queueLimit: 0                          // sin límite de cola
});
//----------------------------------------

// Conectar a la base de datos
db.getConnection((err, connection) => {
  if (err) {
    console.error('Error al conectar:', err);
    return;
  }
  console.log('Conectado a MySQL');
  connection.release();
});


app.get('/',(req, res)=>{

    res.send('Hello my friends: usa /level#/ejercicio# para ver los resultados de las consultas SQL ejemplo: /level1/ejercicio-1')
})

// Actividad 3: Consulta SQL con Express y MySQL
// Enpoint Ejecrcicios:
const urlEjer= 'ejercicio-'
const urllev = 'level'

// Aqui iran las enpoint de las consultas de la actividad 
// ------------------------------------------




const PORT = process.env.PORT || 3000;

app.listen(PORT, ()=>{
    console.log(`corriendo en el puerto: http://localhost:${PORT}/`)
})

```

## Nivel 1: Consultas Básicas y Relaciones Directas (2 Tablas)

1. Listar el nombre de un usuario (el que tu quieras), su correo electrónico y el código (`order_number`) de todos los pedidos que han realizado.

```
app.get(`/${urllev}1/${urlEjer}1`, (req, res) => {
  const query = "SELECT u.name,u.last_name,u.email,o.order_number FROM users u INNER JOIN orders o ON u.id = o.user_id WHERE u.id  = '1'";

  db.query(query, (err, results) => {
    if (err) {
      console.error('Error al ejecutar la consulta:', err);
      return res.status(500).send('Error en el servidor');
    }
    console.log(res.json(results)); 
  });
});   
```

2. Obtener todos los pedidos (código y fecha) realizados por un usuario con un correo electrónico específico (ej: `isamel@pedrito.es`).

```
app.get(`/${urllev}1/${urlEjer}2`, (req, res) => {
  const query = "SELECT o.order_number,o.created_at FROM orders o INNER JOIN users u ON o.user_id = u.id WHERE u.email = 'isamel@pedrito.es'";

  db.query(query, (err, results) => {
    if (err) {
      console.error('Error al ejecutar la consulta:', err);
      return res.status(500).send('Error en el servidor');
    }
    console.log(res.json(results)); 
  });
}); 
```

3. Mostrar el nombre de cada producto junto con el nombre de la categoría a la que pertenece.

```
app.get(`/${urllev}1/${urlEjer}3`, (req, res) => {
  const query = "SELECT p.name AS product_name, c.name AS category_name FROM products p INNER JOIN categories c ON p.category_id = c.id";

  db.query(query, (err, results) => {
    if (err) {
      console.error('Error al ejecutar la consulta:', err);
      return res.status(500).send('Error en el servidor');
    }
    console.log(res.json(results)); 
  });
}); 

```

4. Obtener una lista de los usuarios que se han registrado en el sistema pero que nunca han realizado una compra.

```
app.get(`/${urllev}1/${urlEjer}4`, (req, res) => {
  const query = "SELECT u.name,u.email FROM users u LEFT JOIN orders o ON u.id = o.user_id WHERE o.user_id IS NULL  ";

  db.query(query, (err, results) => {
    if (err) {
      console.error('Error al ejecutar la consulta:', err);
      return res.status(500).send('Error en el servidor');
    }
    console.log(res.json(results)); 
  });
}); 
```

5. Calcular el monto total gastado de un usuario (el que ustedes elijan) en toda su historia, mostrando el nombre del usuario y el total.

```
app.get(`/${urllev}1/${urlEjer}5`, (req, res) => {
  const query = "SELECT u.name, SUM(o.total_amount) AS total_gastado FROM users u LEFT JOIN orders o ON u.id = o.user_id GROUP BY u.id HAVING u.id = '1' ";

  db.query(query, (err, results) => {
    if (err) {
      console.error('Error al ejecutar la consulta:', err);
      return res.status(500).send('Error en el servidor');
    }
    console.log(res.json(results)); 
  });
}); 
```

6. Contar cuántos pedidos existen actualmente clasificados por cada estado (`status`).

```
app.get(`/${urllev}1/${urlEjer}6`, (req, res) => {
    const query = "SELECT status, COUNT(*) AS total FROM orders GROUP BY status";
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

7. Listar todos los productos de la categoría `Electrónica` ordenados por precio de venta, del más caro al más barato.

```
app.get(`/${urllev}1/${urlEjer}7`, (req, res) => {
    const query = "SELECT p.name, p.price FROM products p INNER JOIN categories c ON p.category_id = c.id WHERE c.name = 'Electrónica' ORDER BY p.price DESC";
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

8. Dado un número de orden específico, mostrar los IDs de los productos y la cantidad comprada de cada uno en esa orden.

```
app.get(`/${urllev}1/${urlEjer}8`, (req, res) => {
    const orderId = req.query.order_id || '1'; // puedes pasar el ID por query string
    const query = "SELECT oi.product_id, oi.quantity FROM order_items oi WHERE oi.order_id = ?";
    db.query(query, [orderId], (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

9. Listar los nombres de los usuarios de una ciudad específica (ej: `Monterrey`) que tengan al menos un pedido registrado.

```
app.get(`/${urllev}1/${urlEjer}9`, (req, res) => {
    const city = req.query.city || 'Monterrey';
    const query = "SELECT DISTINCT u.name FROM users u INNER JOIN orders o ON u.id = o.user_id WHERE u.city = ?";
    db.query(query, [city], (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

10. Calcular el valor promedio de los pedidos realizados por cada usuario.

```
app.get(`/${urllev}1/${urlEjer}10`, (req, res) => {
    const query = "SELECT u.name, AVG(o.total_amount) AS promedio_gasto FROM users u INNER JOIN orders o ON u.id = o.user_id GROUP BY u.id";
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});

```

---

## Nivel 2: Consultas Intermedias (3 Tablas)

11. Generar un recibo detallado que muestre:
   - Código de orden  
   - Fecha  
   - Nombre del producto comprado  
   - Precio al que se vendió  

```
app.get(`/${urllev}2/${urlEjer}11`, (req, res) => {
    const query = `
        SELECT o.order_number, o.created_at, p.name AS product_name, oi.price AS selling_price
        FROM orders o
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

12. Calcular el ingreso total generado por cada categoría de productos.

```
app.get(`/${urllev}2/${urlEjer}12`, (req, res) => {
    const query = `
        SELECT c.name AS category_name, SUM(oi.price * oi.quantity) AS total_income
        FROM categories c
        INNER JOIN products p ON c.id = p.category_id
        INNER JOIN order_items oi ON p.id = oi.product_id
        GROUP BY c.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

13. Listar los nombres únicos de todos los productos que ha comprado un cliente específico (buscar por nombre del cliente).

```
app.get(`/${urllev}2/${urlEjer}13`, (req, res) => {
    const name = req.query.name || 'Juan';
    const query = `
        SELECT DISTINCT p.name AS product_name
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
        WHERE u.name = ?
    `;
    db.query(query, [name], (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

14. Identificar los 5 productos más vendidos históricamente (basado en la cantidad total de unidades).

```
app.get(`/${urllev}2/${urlEjer}14`, (req, res) => {
    const query = `
        SELECT p.name, SUM(oi.quantity) AS total_sold
        FROM products p
        INNER JOIN order_items oi ON p.id = oi.product_id
        GROUP BY p.id
        ORDER BY total_sold DESC
        LIMIT 5
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

15. Obtener la fecha de la última vez que se vendió cada producto, mostrando el nombre del producto y la fecha.

```
app.get(`/${urllev}2/${urlEjer}15`, (req, res) => {
    const query = `
        SELECT p.name AS product_name, MAX(o.created_at) AS last_sold_date
        FROM products p
        INNER JOIN order_items oi ON p.id = oi.product_id
        INNER JOIN orders o ON oi.order_id = o.id
        GROUP BY p.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

16. Listar los nombres de los usuarios que han comprado al menos un producto que contenga la palabra `Gamer` en su nombre.

```
app.get(`/${urllev}2/${urlEjer}16`, (req, res) => {
    const query = `
        SELECT DISTINCT u.name
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
        WHERE p.name LIKE '%Gamer%'
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

17. Calcular los ingresos totales de la tienda agrupados por día.

```
app.get(`/${urllev}2/${urlEjer}17`, (req, res) => {
    const query = `
        SELECT DATE(o.created_at) AS sale_date, SUM(o.total_amount) AS daily_income
        FROM orders o
        GROUP BY DATE(o.created_at)
        ORDER BY sale_date DESC
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

18. Identificar las categorías que tienen productos registrados pero que nunca han generado una venta.

```
app.get(`/${urllev}2/${urlEjer}18`, (req, res) => {
    const query = `
        SELECT c.name AS category_name
        FROM categories c
        INNER JOIN products p ON c.id = p.category_id
        LEFT JOIN order_items oi ON p.id = oi.product_id
        WHERE oi.product_id IS NULL
        GROUP BY c.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

19. Mostrar el ticket promedio de compra (gasto promedio por orden) de cada usuario.

```
app.get(`/${urllev}2/${urlEjer}19`, (req, res) => {
    const query = `
        SELECT u.name, AVG(o.total_amount) AS avg_ticket
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

20. Listar los nombres de los productos que formaban parte de órdenes que terminaron siendo canceladas.

```
app.get(`/${urllev}2/${urlEjer}20`, (req, res) => {
    const query = `
        SELECT DISTINCT p.name
        FROM products p
        INNER JOIN order_items oi ON p.id = oi.product_id
        INNER JOIN orders o ON oi.order_id = o.id
        WHERE o.status = 'cancelled'
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

---

## Nivel 3: Consultas Complejas y Reportes (4+ Tablas)

21. **Reporte Global**  
   Mostrar una tabla con:
   - Nombre del Usuario  
   - Ciudad  
   - Número de Orden  
   - Nombre del Producto  
   - Categoría  
   - Cantidad  
   - Subtotal del ítem  

```
app.get(`/${urllev}3/${urlEjer}21`, (req, res) => {
    const query = `
        SELECT 
            u.name AS user_name,
            u.city,
            o.order_number,
            p.name AS product_name,
            c.name AS category_name,
            oi.quantity,
            (oi.price * oi.quantity) AS subtotal
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
        INNER JOIN categories c ON p.category_id = c.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

22. Calcular cuánto dinero han generado las ventas de la categoría `Ropa` exclusivamente en una ciudad específica.

```
app.get(`/${urllev}3/${urlEjer}22`, (req, res) => {
    const city = req.query.city || 'Monterrey';
    const query = `
        SELECT SUM(oi.price * oi.quantity) AS total_income
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
        INNER JOIN categories c ON p.category_id = c.id
        WHERE c.name = 'Ropa' AND u.city = ?
    `;
    db.query(query, [city], (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

23. Identificar al **Cliente del Año**:  
   El usuario que ha gastado más dinero en total dentro de la plataforma.

```
app.get(`/${urllev}3/${urlEjer}23`, (req, res) => {
    const query = `
        SELECT u.name, SUM(o.total_amount) AS total_spent
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.id
        ORDER BY total_spent DESC
        LIMIT 1
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

24. Listar los productos que no han tenido ninguna venta registrada.

```
app.get(`/${urllev}3/${urlEjer}24`, (req, res) => {
    const query = `
        SELECT p.name
        FROM products p
        LEFT JOIN order_items oi ON p.id = oi.product_id
        WHERE oi.product_id IS NULL
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

25. Calcular la **Ganancia Real (Profit)** de la empresa:  
   `(Precio de venta histórico en la orden - Costo de compra del producto)`.

```
app.get(`/${urllev}3/${urlEjer}25`, (req, res) => {
    const query = `
        SELECT 
            p.name AS product_name,
            oi.price AS selling_price,
            p.cost AS purchase_cost,
            (oi.price - p.cost) AS profit_per_unit,
            (oi.price - p.cost) * oi.quantity AS total_profit
        FROM order_items oi
        INNER JOIN products p ON oi.product_id = p.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

26. Mostrar los usuarios que han comprado productos de la categoría `Videojuegos` pero no han comprado productos de `Hogar`.

```
app.get(`/${urllev}3/${urlEjer}26`, (req, res) => {
    const query = `
        SELECT DISTINCT u.name
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
        INNER JOIN categories c ON p.category_id = c.id
        WHERE c.name = 'Videojuegos'
        AND u.id NOT IN (
            SELECT DISTINCT u2.id
            FROM users u2
            INNER JOIN orders o2 ON u2.id = o2.user_id
            INNER JOIN order_items oi2 ON o2.id = oi2.order_id
            INNER JOIN products p2 ON oi2.product_id = p2.id
            INNER JOIN categories c2 ON p2.category_id = c2.id
            WHERE c2.name = 'Hogar'
        )
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

27. Generar un ranking de las 3 ciudades que más ingresos han generado a la tienda.

```
app.get(`/${urllev}3/${urlEjer}27`, (req, res) => {
    const query = `
        SELECT u.city, SUM(o.total_amount) AS total_income
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.city
        ORDER BY total_income DESC
        LIMIT 3
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

28. Encontrar la orden que contiene la mayor variedad de productos distintos (mayor cantidad de ítems únicos).

```
app.get(`/${urllev}3/${urlEjer}28`, (req, res) => {
    const query = `
        SELECT o.order_number, COUNT(DISTINCT oi.product_id) AS unique_products
        FROM orders o
        INNER JOIN order_items oi ON o.id = oi.order_id
        GROUP BY o.id
        ORDER BY unique_products DESC
        LIMIT 1
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

29. Listar los productos que se vendieron en el pasado a un precio menor que su precio de venta actual en catálogo.

```
app.get(`/${urllev}3/${urlEjer}29`, (req, res) => {
    const query = `
        SELECT DISTINCT p.name, p.price AS current_price, oi.price AS sold_price
        FROM products p
        INNER JOIN order_items oi ON p.id = oi.product_id
        WHERE oi.price < p.price
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

30. Mostrar el historial de compras de un producto específico:
   - Quién lo compró  
   - Cuándo  
   - A qué precio  

```
app.get(`/${urllev}3/${urlEjer}30`, (req, res) => {
    const productId = req.query.product_id || '1';
    const query = `
        SELECT 
            u.name AS buyer_name,
            o.created_at AS purchase_date,
            oi.price AS purchase_price
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        INNER JOIN order_items oi ON o.id = oi.order_id
        WHERE oi.product_id = ?
    `;
    db.query(query, [productId], (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

---

## Nivel 4: Lógica de Negocio y Analítica Avanzada

31. Listar a los usuarios cuyo gasto total acumulado es superior al promedio de gasto de todos los clientes de la tienda.

```
app.get(`/${urllev}4/${urlEjer}31`, (req, res) => {
    const query = `
        SELECT u.name, SUM(o.total_amount) AS total_spent
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.id
        HAVING total_spent > (
            SELECT AVG(total_spent)
            FROM (
                SELECT SUM(o2.total_amount) AS total_spent
                FROM users u2
                INNER JOIN orders o2 ON u2.id = o2.user_id
                GROUP BY u2.id
            ) AS avg_table
        )
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

32. Identificar los productos **Estrella**:  
   Aquellos que representan individualmente más del 2% del total de ingresos de la empresa.

```
app.get(`/${urllev}4/${urlEjer}32`, (req, res) => {
    const query = `
        SELECT 
            p.name,
            SUM(oi.price * oi.quantity) AS product_income,
            (SUM(oi.price * oi.quantity) / (SELECT SUM(oi2.price * oi2.quantity) FROM order_items oi2)) * 100 AS percentage_of_total
        FROM products p
        INNER JOIN order_items oi ON p.id = oi.product_id
        GROUP BY p.id
        HAVING percentage_of_total > 2
        ORDER BY percentage_of_total DESC
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

33. **Churn Rate**  
   Listar los usuarios que hicieron compras en el pasado, pero que no han realizado ningún pedido en los últimos 6 meses.

```
app.get(`/${urllev}4/${urlEjer}33`, (req, res) => {
    const query = `
        SELECT u.name, MAX(o.created_at) AS last_purchase
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.id
        HAVING last_purchase < DATE_SUB(NOW(), INTERVAL 6 MONTH)
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

34. Clasificar a los clientes en tres niveles según su gasto total:
   - `VIP` → gasto > 5000  
   - `Frecuente` → entre 1000 y 5000  
   - `Regular` → < 1000  

```
app.get(`/${urllev}4/${urlEjer}34`, (req, res) => {
    const query = `
        SELECT 
            u.name,
            SUM(o.total_amount) AS total_spent,
            CASE
                WHEN SUM(o.total_amount) > 5000 THEN 'VIP'
                WHEN SUM(o.total_amount) BETWEEN 1000 AND 5000 THEN 'Frecuente'
                ELSE 'Regular'
            END AS customer_level
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.id
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

35. Determinar cuál ha sido el mes (y año) con mayor facturación en la historia de la tienda.

```
app.get(`/${urllev}4/${urlEjer}35`, (req, res) => {
    const query = `
        SELECT 
            YEAR(o.created_at) AS year,
            MONTH(o.created_at) AS month,
            SUM(o.total_amount) AS total_income
        FROM orders o
        GROUP BY YEAR(o.created_at), MONTH(o.created_at)
        ORDER BY total_income DESC
        LIMIT 1
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

36. **Alerta de Inventario**  
   Listar las órdenes pendientes que incluyen productos cuyo stock actual es menor a 5 unidades.

```
app.get(`/${urllev}4/${urlEjer}36`, (req, res) => {
    const query = `
        SELECT 
            o.order_number,
            p.name AS product_name,
            p.stock AS current_stock
        FROM orders o
        INNER JOIN order_items oi ON o.id = oi.order_id
        INNER JOIN products p ON oi.product_id = p.id
        WHERE o.status = 'pending' AND p.stock < 5
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

37. Calcular qué porcentaje de las ventas totales representa cada categoría  
   (ej: Electrónica 40%, Ropa 20%, etc.).

```
app.get(`/${urllev}4/${urlEjer}37`, (req, res) => {
    const query = `
        SELECT 
            c.name AS category_name,
            SUM(oi.price * oi.quantity) AS category_income,
            (SUM(oi.price * oi.quantity) / (SELECT SUM(oi2.price * oi2.quantity) FROM order_items oi2)) * 100 AS percentage_of_total
        FROM categories c
        INNER JOIN products p ON c.id = p.category_id
        INNER JOIN order_items oi ON p.id = oi.product_id
        GROUP BY c.id
        ORDER BY percentage_of_total DESC
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

38. Comparar las ventas totales de cada ciudad contra el promedio de ventas de todas las ciudades.

```
app.get(`/${urllev}4/${urlEjer}38`, (req, res) => {
    const query = `
        SELECT 
            u.city,
            SUM(o.total_amount) AS city_income,
            (SELECT AVG(city_total) FROM (
                SELECT SUM(o2.total_amount) AS city_total
                FROM users u2
                INNER JOIN orders o2 ON u2.id = o2.user_id
                GROUP BY u2.city
            ) AS avg_table) AS avg_income_all_cities,
            (SUM(o.total_amount) - (
                SELECT AVG(city_total) FROM (
                    SELECT SUM(o2.total_amount) AS city_total
                    FROM users u2
                    INNER JOIN orders o2 ON u2.id = o2.user_id
                    GROUP BY u2.city
                ) AS avg_table
            )) AS difference_from_avg
        FROM users u
        INNER JOIN orders o ON u.id = o.user_id
        GROUP BY u.city
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

39. Calcular la tasa de cancelación:  
   Porcentaje de órdenes con estado `cancelled` respecto al total de órdenes por mes.

```
app.get(`/${urllev}4/${urlEjer}39`, (req, res) => {
    const query = `
        SELECT 
            YEAR(o.created_at) AS year,
            MONTH(o.created_at) AS month,
            COUNT(*) AS total_orders,
            COUNT(CASE WHEN o.status = 'cancelled' THEN 1 END) AS cancelled_orders,
            (COUNT(CASE WHEN o.status = 'cancelled' THEN 1 END) * 100.0 / COUNT(*)) AS cancellation_rate
        FROM orders o
        GROUP BY YEAR(o.created_at), MONTH(o.created_at)
        ORDER BY year DESC, month DESC
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```

40. **Análisis de Canasta**  
   Identificar qué pares de productos se venden juntos con mayor frecuencia en la misma orden.

```
app.get(`/${urllev}4/${urlEjer}40`, (req, res) => {
    const query = `
        SELECT 
            p1.name AS product1,
            p2.name AS product2,
            COUNT(*) AS times_sold_together
        FROM order_items oi1
        INNER JOIN order_items oi2 ON oi1.order_id = oi2.order_id AND oi1.product_id < oi2.product_id
        INNER JOIN products p1 ON oi1.product_id = p1.id
        INNER JOIN products p2 ON oi2.product_id = p2.id
        GROUP BY p1.id, p2.id
        ORDER BY times_sold_together DESC
        LIMIT 5
    `;
    db.query(query, (err, results) => {
        if (err) {
            console.error('Error al ejecutar la consulta:', err);
            return res.status(500).send('Error en el servidor');
        }
        res.json(results);
    });
});
```



### Corro mi servidor con Node.js para provar los ejecicios

```
cd express
node index.js
```