---
title: Actividad Robin -5
---
# Robin-BaseDatos

# Actividad 5

# Consultas en MongoDB

Colección: users

## Nivel 1 – Consultas Básicas
1.Listar todos los documentos de la colección users.

2.Mostrar únicamente los campos first_name, last_name y email.

3.Obtener todos los usuarios cuyo role sea "admin".

4.Buscar los usuarios cuyo country sea "Colombia".

5.Listar los usuarios que estén activos (is_active = true).

6.Buscar los usuarios que no estén verificados (is_verified = false).

7.Obtener los usuarios cuyo gender sea "Masculino".

8.Listar los usuarios que vivan en la ciudad "Medellín".

9.Buscar los usuarios que tengan al menos un hijo (children_count > 0).

10.Listar los usuarios cuya profesión (profession) no sea null.

## Nivel 2 – Filtros con Operadores
11.Buscar usuarios con monthly_income mayor a 3000000.

12.Buscar usuarios con ingresos entre 2000000 y 5000000.

13.Buscar usuarios cuya fecha de nacimiento sea posterior al 2000-01-01.

14.Buscar usuarios cuyo document_type esté en ["CC", "CE"].

15.Buscar usuarios cuyo city no sea "Bogotá".

16.Buscar usuarios cuyo nombre empiece por la letra "A".

17.Buscar usuarios cuyo email termine en "gmail.com".

18.Buscar usuarios que tengan más de 2 hijos y estén activos.

19.Buscar usuarios cuyo marital_status sea "Casado" y tengan hijos.

20.Buscar usuarios que estén inactivos o no verificados.

## Nivel 3 – Ordenamiento y Paginación
21.Listar usuarios ordenados por monthly_income de mayor a menor.

22.Obtener los 5 usuarios más recientes según created_at.

23.Implementar paginación: mostrar la página 2 con 10 registros por página.

24.Mostrar nombre completo concatenado (first_name + last_name) y ciudad usando agregación.

25.Listar usuarios ordenados por fecha de nacimiento del más joven al mayor.

## Nivel 4 – Aggregation Framework
26.Calcular el ingreso promedio (monthly_income) de todos los usuarios.

27.Calcular el ingreso promedio por ciudad.

28.Contar cuántos usuarios hay por cada role.

29.Contar cuántos usuarios están activos vs inactivos.

30.Obtener la cantidad total de hijos agrupados por state.