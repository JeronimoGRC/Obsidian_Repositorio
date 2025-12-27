
---

Revisar [[Base de datos]].

---

## Inicio de sesión en el entorno

Desde la terminal usaremos este comando:

```
mysql -u <usuario> -p
```

Ejemplo:

```
mysql -u root -p
```

Visualizar las bases de datos;

```
SHOW DATABASES;
# SALIDA DE EJEMPLO QUE NOS DARÁ
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| phpmyadmin         |
| sakila             |
| test               |
+--------------------+
6 rows in set (0.001 sec)
```


Seleccionar una base de datos con la que trabajaremos:

```
USE <Base de datos que vamos a escoger>;
# SALIDA 

Database changed
```

Ver las tablas:

```
SHOW TABLES;

+----------------------------+
| Tables_in_sakila           |
+----------------------------+
| actor                      |
| actor_info                 |
| address                    |
| category                   |
| city                       |
| country                    |
| customer                   |
| customer_list              |
| film                       |
| film_actor                 |
| film_category              |
| film_list                  |
| film_text                  |
| inventory                  |
| language                   |
| nicer_but_slower_film_list |
+----------------------------+
23 rows in set (0.001 sec)
```

---

## Consultas SQL


Consulta básica:

```
SELECT * FROM <tabla>;
```

Selección de columnas:

```
SELECT <fila1>, <fila2> FROM <tabla>;
```

### Modificadores

Registros únicos: **DISTINCT** -> Te devuelve las filas sin repetirse los datos.

```
SELECT DISTINCT <fila> FROM <tabla>;
```

Alias de columna: **AS** 

Ejemplo:

```
SELECT title AS "film name" FROM film; 
```

Al hacer la consulta aparecerá en la cabera de la columna film *name* en vez de *title* pero no se quedará como un cambio permanente.


### Restricción y ordenación

Clausula WHERE:

Ejemplo:

```
SELECT *
FROM actor
WHERE apellido = 'ALLEN';
```

Nos devolverá los datos de la tabla donde el apellido coincida con el filtro.


Operandos de comparación:

`= / > / < / >= / <= / <> o != `

```
SELECT *
FROM film
WHERE lenght >= 120;
```

Ordenación:

- `ASC` -> Ascendente -> Es la configuración por defecto.
- `DESC` -> Descendente.

```
SELECT *
FROM film
ORDER BY title DESC;
```

### Operadores Lógicos

Condiciones múltiples:

- **AND** -> Deben cumplir ambas condiciones.

```
SELECT *
FROM film
WHERE lenght > 90 AND rental_rate <= 3;
```

- **OR** -> Cualquiera de las condiciones es candidata para aparecer en las salidas.

```
SELECT *
FROM film
WHERE lenght > 90 OR rental_rate <= 3;
```


Filtro mas complejo:

```
SELECT *
FROM customer
WHERE (first_name = 'JOHN' OR last_name = 'SMITH')
AND active = 1;
```

En este caso le dice que muestre todas las filas de la tabla `customer` donde el nombre sea JOHN o el apellido sea SMITH, puede servir cualquiera de las dos, y que a su vez esté activo.

- Negación, uso de **NOT**.

```
SELECT *
FROM actor
WHERE NOT last_name = 'ALLEN';
```

Todas las filas donde el apellido del actor no sea ALLEN.


- Operador **LIKE** + wildcards (%)

```
SELECT *
FROM film
WHERE title LIKE '%LOVE%';
```

Donde el `title` contenga la palabra `LOVE`. 

- Operador **IN** vs `=`.

```
SELECT *
FROM actor
WHERE last_name IN
('ALLEN', 'PALTROW');
```

El comparador **IN** no es compatible con la palabra reservada **LIKE**, para esos  casos habría que usar la igualdad `=`.

- Operador **BETWEEN**.

```
SELECT *
FROM film
WHERE lenght BETWEEN 0.5 AND 2.5;
```

Ejemplo de consulta completa:

```
SELECT title AS 'título', rental_rate as price
FROM film
WHERE (lenght BETWEEN 90 AND 120)
AND (rental_rate <= 3 OR replacement_cost < 15)
ORDER BY price DESC;
```


### Consultas de unión

**UNION** -> Consolidar los datos de dos tablas en una sola salida. 

En este ejemplo tenemos que observar que los campos de las columnas deben ser compatibles y que deben coincidir en el número de columnas que le pedimos en ambas consultas.  

```
SELECT last_name, first_name
FROM customer
UNION
SELECT last_name, first_name
FROM actor;
```

### Subconsultas

Ejemplo:

```
SELECT lastName, firstName
FROM employees
WHERE officeCode IN (SELECT officeCode
					FROM offices
					WHERE country = 'USA');
```

La subconsulta puede funcionar como una consulta independiente, nos devolvería un conjunto de códigos de oficina que se usará para hacer la consulta principal.



### Funciones escalares

- **LCASE** -> Low case , minúscula.

```
SELECT LCASE(title)
FROM film;
```

- **CONCAT** -> Concatenación.

```
SELECT CONCAT(apellido,', ',nombre)
FROM usuarios;
```


Para ver más funciones consultar https://mariadb.com/kb/en/library/built-in-functions/


### Funciones de agregado

Ejemplos: `COUNT, MAX, MIN, SUM, AVG`.

- COUNT

```
SELECT COUNT(*) AS `Clientes Activos`
FROM clientes
WHERE activo = 1;
```

Cuenta la cantidad de líneas de la consulta.

- MAX

```
SELECT MAX(precio)
FROM peliculas; 
```

Te debe dar el precio de cual es la película más cara.

- MIN

```
SELECT MIN(precio)
FROM peliculas; 
```

Te debe dar el precio de cual es la película más barata.

### Consultas de agrupamiento

Ejemplos Agrupación: **GROUP BY**

```
SELECT COUNT(film_id), rental_rate
FROM film
GROUP BY rental_rate;
```

- **HAVING** -> Permite hacer una restricción de filas sobre campos que serán el resultado, va "de la mano" con le **GROUP BY**.

```
SELECT COUNT(film_id), replacement_cost
FROM film
WHERE replacement_cost <= 20
GROUP BY replacement_cost
HAVING COUNT(film_id) <= 50
ORDER BY replacement_cost;
```



### Consultas multi-tabla mediante WHERE

```
SELECT first_name, last_name, address
FROM customer c, address a
WHERE c.address_id = a.address_id;
```

### SQL JOINS

TIPOS DE JOINS MÁS HABITUALES:

- INNER JOIN -> Obtengo solo los registros que tienen presencia en las dos tablas.
- LEFT JOIN -> Todos los registros de la tabla `A`.
- RIGHT JOIN -> Todos los registros de la tabla `B`.
- FULL OUTER JOIN -> Devuélveme todos los registros de las dos tablas.


Ejemplo:

**INNER JOIN**

```
SELECT first_name, last_name, a.address
FROM customer c INNER JOIN address a
ON c.address_id = a.address_id;
```


**LEFT JOIN**

```
SELECT first_name, last_name, a.address
FROM customer c LEFT JOIN address a
ON c.address_id = a.address_id;
```

Veríamos todos los clientes que tengan relación con la tabla `address` y los que no la tengan también.

**RIGHT JOIN**

```
SELECT first_name, last_name, a.address
FROM customer c RIGHT JOIN address a
ON c.address_id = a.address_id;
```

Veríamos todos las direcciones que tengan relación con la tabla `customer` y las que no la tengan también.

