# Conceptos básicos de SQL
* * *

## SELECT:
Obtiene filas de la base de datos y permite realizar la selección de una o varias filas o columnas de una o varias tablas.

## Claúsulas
- **SELECT nombre_Columnas:** nombre de las columnas que se desean ver en el resultado de una consulta, se pueden listar columnas a calcular.
- **INTO nueva_tabla_desino:** Usada para los resultados del SELECT, puede ser opcional
- **FROM nombre_tabla:** tabla de donde voy a necesitar para realizar la consulta.
- **WHERE condiciones_Búsqueda:** Puede omitirse si no hay condiciones de búsqueda.
- **GROUP BY Columnas:** permite agrupar un conjunto de columnas de valores repetidos y para los valores no repetidos, permite usar funciones de agregación. Puede omitirse.
- **HAVING:** incluye los valores que el GROUP BY ha agrupado, Utiliza una condición de búsqueda para especificar los grupos. Puede omitirse.
- **ORDER BY nombre de columnas [ASC || DESC] :** establecer la columna o columnas sobre las cuales las filas resultantes deben ser ordenadas. Puede omitirse.

## Otros elementos en la consulta:
- **AS:** Agrega un alias a la columna del SELECT
- **DISTINCT:** Se usa para sacar registros no repetidos de una consulta, cuando la clave primaria no esta incluida en las columnas del SELECT.
- **Columnas calculadas:** Son columnas donde su valor es hallado a partir de los valores de otras columnas.

## Condiciones de búsqueda:
- **( =, <>, >, <, >=, <= ), BETWEEN:** Son utilizados para comparar cadenas, expresiones aritméticas y fechas.
- **AND,OR,NOT:** Son utilizados para filtrar la selección en la consulta.
- **LIKE:** hace una búsqueda donde selecciona por la condición luego del LIKE.
- **IS NULL:** Cabe la posibilidad que en una columna exista un campo en el que no contengan valor alguno, por lo cual la consulta vuelve un valor NULL.

## Operaciones básicas:
- **SUM:** Sumar elemento de una columna
- **AVG:** Obtener el promedio deseado
- **MIN:** obtener el valor mínimo de la columna seleccionada
- **MAX:** Obtiene la cantidad máxima de una columna
- **COUNT:** Cuenta el total de elementos según la condición deseada.
