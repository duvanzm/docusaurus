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

4. Obtener una lista de los usuarios que se han registrado en el sistema pero que nunca han realizado una compra.

5. Calcular el monto total gastado de un usuario (el que ustedes elijan) en toda su historia, mostrando el nombre del usuario y el total.

6. Contar cuántos pedidos existen actualmente clasificados por cada estado (`status`).

7. Listar todos los productos de la categoría `Electrónica` ordenados por precio de venta, del más caro al más barato.

8. Dado un número de orden específico, mostrar los IDs de los productos y la cantidad comprada de cada uno en esa orden.

9. Listar los nombres de los usuarios de una ciudad específica (ej: `Monterrey`) que tengan al menos un pedido registrado.

10. Calcular el valor promedio de los pedidos realizados por cada usuario.

---

## Nivel 2: Consultas Intermedias (3 Tablas)

11. Generar un recibo detallado que muestre:
   - Código de orden  
   - Fecha  
   - Nombre del producto comprado  
   - Precio al que se vendió  

12. Calcular el ingreso total generado por cada categoría de productos.

13. Listar los nombres únicos de todos los productos que ha comprado un cliente específico (buscar por nombre del cliente).

14. Identificar los 5 productos más vendidos históricamente (basado en la cantidad total de unidades).

15. Obtener la fecha de la última vez que se vendió cada producto, mostrando el nombre del producto y la fecha.

16. Listar los nombres de los usuarios que han comprado al menos un producto que contenga la palabra `Gamer` en su nombre.

17. Calcular los ingresos totales de la tienda agrupados por día.

18. Identificar las categorías que tienen productos registrados pero que nunca han generado una venta.

19. Mostrar el ticket promedio de compra (gasto promedio por orden) de cada usuario.

20. Listar los nombres de los productos que formaban parte de órdenes que terminaron siendo canceladas.

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

22. Calcular cuánto dinero han generado las ventas de la categoría `Ropa` exclusivamente en una ciudad específica.

23. Identificar al **Cliente del Año**:  
   El usuario que ha gastado más dinero en total dentro de la plataforma.

24. Listar los productos que no han tenido ninguna venta registrada.

25. Calcular la **Ganancia Real (Profit)** de la empresa:  
   `(Precio de venta histórico en la orden - Costo de compra del producto)`.

26. Mostrar los usuarios que han comprado productos de la categoría `Videojuegos` pero no han comprado productos de `Hogar`.

27. Generar un ranking de las 3 ciudades que más ingresos han generado a la tienda.

28. Encontrar la orden que contiene la mayor variedad de productos distintos (mayor cantidad de ítems únicos).

29. Listar los productos que se vendieron en el pasado a un precio menor que su precio de venta actual en catálogo.

30. Mostrar el historial de compras de un producto específico:
   - Quién lo compró  
   - Cuándo  
   - A qué precio  

---

## Nivel 4: Lógica de Negocio y Analítica Avanzada

31. Listar a los usuarios cuyo gasto total acumulado es superior al promedio de gasto de todos los clientes de la tienda.

32. Identificar los productos **Estrella**:  
   Aquellos que representan individualmente más del 2% del total de ingresos de la empresa.

33. **Churn Rate**  
   Listar los usuarios que hicieron compras en el pasado, pero que no han realizado ningún pedido en los últimos 6 meses.

34. Clasificar a los clientes en tres niveles según su gasto total:
   - `VIP` → gasto > 5000  
   - `Frecuente` → entre 1000 y 5000  
   - `Regular` → < 1000  

35. Determinar cuál ha sido el mes (y año) con mayor facturación en la historia de la tienda.

36. **Alerta de Inventario**  
   Listar las órdenes pendientes que incluyen productos cuyo stock actual es menor a 5 unidades.

37. Calcular qué porcentaje de las ventas totales representa cada categoría  
   (ej: Electrónica 40%, Ropa 20%, etc.).

38. Comparar las ventas totales de cada ciudad contra el promedio de ventas de todas las ciudades.

39. Calcular la tasa de cancelación:  
   Porcentaje de órdenes con estado `cancelled` respecto al total de órdenes por mes.

40. **Análisis de Canasta**  
   Identificar qué pares de productos se venden juntos con mayor frecuencia en la misma orden.



### Corro mi servidor con Node.js para provar los ejecicios

```
cd express
node index.js
```