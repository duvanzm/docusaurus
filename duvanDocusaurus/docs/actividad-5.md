---
title: Actividad Robin -5
---
# Robin-BaseDatos

# Actividad 5

# Consultas en MongoDB

Colección: users

## Nivel 1 – Consultas Básicas
1.Listar todos los documentos de la colección users.

```
db.users.find({},{document_type:1 , document_number:1, _id:0})
```

2.Mostrar únicamente los campos first_name, last_name y email.

```
db.users.find({},{first_name:1 , last_name:1, _id:0, email:1})
```

3.Obtener todos los usuarios cuyo role sea "admin".

```
db.users.find({role: "admin"})
```

4.Buscar los usuarios cuyo country sea "Colombia".

```
db.users.find({country: "Colombia"})
```

5.Listar los usuarios que estén activos (is_active = true).

```
db.users.find({is_active: 1})
```

6.Buscar los usuarios que no estén verificados (is_verified = false).

```
db.users.find({is_verified: 0})
```

7.Obtener los usuarios cuyo gender sea "Masculino".

```
db.users.find({gender: "Masculino"})
```


8.Listar los usuarios que vivan en la ciudad "Medellín".

```
db.users.find({city: "Medellín"})
```

9.Buscar los usuarios que tengan al menos un hijo (children_count > 0).

```
db.users.find({children_count:{$gt:0}})
```

10.Listar los usuarios cuya profesión (profession) no sea null.

```
db.users.find({profession: null})
```

## Nivel 2 – Filtros con Operadores
11.Buscar usuarios con monthly_income mayor a 3000000.

```
db.users.find({monthly_income: {$gt:3000000}})
```

12.Buscar usuarios con ingresos entre 2000000 y 5000000.

```
db.users.find({monthly_income: {$gte:2000000, $lte:5000000}})
```

13.Buscar usuarios cuya fecha de nacimiento sea posterior al 2000-01-01.

```
db.users.find({birth_date:{$gt: new Date(2000-01-01)}})
```

14.Buscar usuarios cuyo document_type esté en ["CC", "CE"].

```
db.users.find({$or:[{document_type: "CC"},{document_type:"CE"}]})

```

15.Buscar usuarios cuyo city no sea "Bogotá".

```
db.users.find({city:{$ne:"Bogotá"}})
```

16.Buscar usuarios cuyo nombre empiece por la letra "A".

```
db.users.find({first_name:{ $regex: "^A", $options: "i" }})
```

17.Buscar usuarios cuyo email termine en "gmail.com".

```
db.users.find({email: { $regex: "gmail\\.com$", $options: "i" }})
```

18.Buscar usuarios que tengan más de 2 hijos y estén activos.

```
db.users.find({$and: [{ children_count: { $gt: 2 } },{ is_active: 1}]})
```

19.Buscar usuarios cuyo marital_status sea "Casado" y tengan hijos.

```
db.users.find({marital_status: "Casado",children_count: { $exists: true, $gt: 0 }})
```

20.Buscar usuarios que estén inactivos o no verificados.

```
db.users.find({$or: [{ is_active: 1},{ is_verified: 1 }]})
```

## Nivel 3 – Ordenamiento y Paginación
21.Listar usuarios ordenados por monthly_income de mayor a menor.

```
db.users.find().sort({ monthly_income: -1 })
```

22.Obtener los 5 usuarios más recientes según created_at.

```
db.users.find().sort({ created_at: -1 })
```


23.Implementar paginación: mostrar la página 2 con 10 registros por página.

```
db.users.find().skip(10).limit(10)
```

24.Mostrar nombre completo concatenado (first_name + last_name) y ciudad usando agregación.

```
db.users.aggregate([{$project: {nombreCompleto: { $concat: ["$first_name", " ", "$last_name"] },ciudad: "$city"}}])
```

25.Listar usuarios ordenados por fecha de nacimiento del más joven al mayor.

```
db.users.find().sort({ birth_date: 1 })
```

## Nivel 4 – Aggregation Framework
26.Calcular el ingreso promedio (monthly_income) de todos los usuarios.

```
db.users.aggregate([{$group: {_id: null,ingresoPromedio: { $avg: "$monthly_income" }}}])
```

27.Calcular el ingreso promedio por ciudad.

```
db.users.aggregate([{$group: {_id: "$city",ingresoPromedio: { $avg: "$monthly_income" }}}])
```

28.Contar cuántos usuarios hay por cada role.

```
db.users.aggregate([{$group: {_id: "$role",cantidad: { $sum: 1 }}}])
```

29.Contar cuántos usuarios están activos vs inactivos.

```
db.users.aggregate([{$group: {_id: "$is_active",cantidad: { $sum: 1 }}}])
```

30.Obtener la cantidad total de hijos agrupados por state.

```
db.users.aggregate([{$group: { _id: "$state",totalHijos: { $sum: "$children_count" }}}])
```