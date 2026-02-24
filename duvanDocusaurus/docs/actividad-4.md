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
Después, realiza una consulta que:

Agrupe los usuarios por ciudad.
Muestre la cantidad de usuarios por cada ciudad.
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
Luego, realiza una consulta que:

Filtre únicamente usuarios con ingresos mayores a 3.000.000.
Ordene los resultados por ciudad.
## Conceptos evaluados
CREATE VIEW
Funciones de fecha
CONCAT
COALESCE
WHERE
ORDER BY
GROUP BY
Las vistas permiten encapsular lógica de negocio directamente en la base de datos, mejorando legibilidad, reutilización y seguridad de las consultas.