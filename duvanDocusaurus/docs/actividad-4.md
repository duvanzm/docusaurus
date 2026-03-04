---
title: Actividad Robin -4
---

# Robin-BaseDatos

# Actividad 4

# Vistas de Consulta en MySQL

- Base de datos: users
## Objetivo: practicar la creación y uso de VISTAS (VIEW) en MySQL usando funciones, filtros y agregaciones.

- Ejercicio 1 – Vista de usuarios mayores de edad

Crea una vista llamada view_adult_users que cumpla con lo siguiente:

Muestre los campos:
id
first_name
last_name
document_type
document_number
city
country
Calcule la edad a partir del campo birth_date.
Incluya únicamente usuarios cuya edad sea mayor o igual a 18 años.

```
CREATE VIEW view_adult_users AS
SELECT 
    id,
    first_name,
    last_name,
    document_type,
    document_number,
    city,
    country,
    TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) AS age
FROM users
WHERE TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) >= 18;
```

- Ejercicio 2 – Vista de contactos consolidados
Crea una vista llamada view_user_contacts que:

Genere un campo full_name concatenando first_name y last_name.
Muestre el correo electrónico del usuario.
Genere un campo contact_number que:
Use mobile si existe.
En caso contrario use phone.
Si ninguno existe, muestre el texto "Sin teléfono".
Incluya:
address
city
state
country

```
CREATE VIEW view_user_contacts AS
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    email,
    COALESCE(mobile, phone, 'Sin teléfono') AS contact_number,
    address,
    city,
    state,
    country
FROM users;
```

- Ejercicio 3 – Vista financiera de usuarios
Crea una vista llamada view_users_with_income que:

Muestre los campos:
id
first_name
last_name
profession
monthly_income
Incluya únicamente usuarios que tengan ingresos registrados y mayores a cero.
Luego, realiza una consulta sobre la vista que ordene los usuarios por ingreso mensual de mayor a menor.

```
CREATE VIEW view_users_with_income AS
SELECT
    id,
    first_name,
    last_name,
    profession,
    monthly_income
FROM users
WHERE monthly_income IS NOT NULL
AND monthly_income > 0;
```
Consulta ordenada:

```
SELECT *
FROM view_users_with_income
ORDER BY monthly_income DESC;
```

- Ejercicio 4 – Vista demográfica
Crea una vista llamada view_demographic_summary que:

Genere un campo full_name.
Calcule la edad del usuario.
Muestre los campos:
gender
marital_status
education_level
city
country

```
CREATE VIEW view_demographic_summary AS
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) AS age,
    gender,
    marital_status,
    education_level,
    city,
    country
FROM users;
```

Después, realiza una consulta que:

Agrupe los usuarios por ciudad.

Muestre la cantidad de usuarios por cada ciudad.

```
SELECT 
    city,
    COUNT(*) AS total_users
FROM view_demographic_summary
GROUP BY city;
```
Ejercicio 5 – Vista de perfil ejecutivo
Crea una vista llamada view_user_profile que:

Genere el campo full_name.
Incluya información de identificación:
document_type
document_number
Calcule la edad del usuario.
Incluya:
profession
education_level
company
Incluya información financiera:
monthly_income
Incluya ubicación:
city
country

```
CREATE VIEW view_user_profile AS
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    document_type,
    document_number,
    TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) AS age,
    profession,
    education_level,
    company,
    monthly_income,
    city,
    country
FROM users;
```
Luego, realiza una consulta que:

Filtre únicamente usuarios con ingresos mayores a 3.000.000.

Ordene los resultados por ciudad.

```
SELECT *
FROM view_user_profile
WHERE monthly_income > 3000000
ORDER BY city;
```

## Conceptos evaluados
CREATE VIEW
Funciones de fecha
CONCAT
COALESCE
WHERE
ORDER BY
GROUP BY
Las vistas permiten encapsular lógica de negocio directamente en la base de datos, mejorando legibilidad, reutilización y seguridad de las consultas.