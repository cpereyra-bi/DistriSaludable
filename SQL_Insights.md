# SQL_Diseño analítico  
## Análisis de Insights

### 0. Entendiendo los datos  
**Pregunta:** ¿Qué datos contiene la tabla a analizar?

```sql
SELECT *
FROM tickets
LIMIT 100;


VENTAS Y TENDENCIA

1. Visión General de Ventas Totales
Pregunta 1: ¿Cuál es el ingreso total generado por el negocio?
SELECT SUM(precio_total) AS ingreso_total
FROM tickets;



2. Tendencias de Ventas en el Tiempo
Pregunta 2: ¿Cómo ha sido la tendencia de ingresos mensuales?
SELECT strftime('%Y-%m', fecha) AS mes, SUM(precio_total) AS ingreso_mensual
FROM tickets
GROUP BY mes
ORDER BY mes;

PRODUCTOS Y SECCIONES

3. Análisis por Departamento y Sección
Pregunta 3: ¿Cuál es el rendimiento de cada departamento en términos de ventas?
SELECT id_departamento, SUM(precio_total) AS ventas_departamento
FROM tickets
GROUP BY id_departamento
ORDER BY ventas_departamento DESC;

Pregunta 4: ¿Cómo se distribuyen las ventas entre las diferentes secciones?
SELECT id_seccion, SUM(precio_total) AS ventas_seccion
FROM tickets
GROUP BY id_seccion
ORDER BY ventas_seccion DESC;

4. Análisis de Productos
Pregunta 5: ¿Cuáles son los 10 productos más vendidos en cantidad?
SELECT nombre_producto, SUM(cantidad) AS total_vendido
FROM tickets
GROUP BY nombre_producto
ORDER BY total_vendido DESC
LIMIT 10;

Pregunta 6: ¿Qué 10 productos generan más ingresos?
SELECT nombre_producto, SUM(precio_total) AS ingreso_producto
FROM tickets
GROUP BY nombre_producto
ORDER BY ingreso_producto DESC
LIMIT 10;


CLIENTES Y PEDIDOS

5. Comportamiento de los Clientes
Pregunta 7: ¿Quiénes son los 20 clientes que más compran en términos de ingresos?
SELECT id_cliente, SUM(precio_total) AS ingreso_cliente
FROM tickets
GROUP BY id_cliente
ORDER BY ingreso_cliente DESC
LIMIT 20;

Pregunta 8: ¿Cuál es la compra media por cliente?
SELECT AVG(compra_total_cliente) AS compra_media_por_cliente 
FROM ( 
SELECT id_cliente, SUM(precio_total) AS compra_total_cliente 
FROM tickets 
GROUP BY id_cliente ) subconsulta;

(Aqui se ejecuta una subconsulta, ya que primero necesito calcular la media de compra por cliente y luego puedo calcular media de las medias de clientes)


6. Análisis de Pedidos
Pregunta 9: ¿Cuántos pedidos totales se han realizado?
SELECT COUNT(DISTINCT id_pedido) AS total_pedidos
FROM tickets;
En esta consulta estamos contando la cantidad total de pedidos únicos registrados en la tabla tickets, utilizando COUNT(DISTINCT id_pedido).
 Sin embargo, es importante tener en cuenta que la tabla tickets está estructurada en función de los productos pedidos, es decir, cada fila representa un producto dentro de un pedido. Por eso, para obtener la cantidad de pedidos únicos, necesitamos aplicar DISTINCT sobre id_pedido.
💡 Nota: Si existiera una tabla específica llamada pedidos que ya contuviera un pedido por fila, sería más adecuado hacer el conteo directamente allí.
Pregunta 10: ¿Cuál es el valor promedio por pedido?
SELECT AVG(total_pedido) AS valor_promedio_pedido
FROM (
SELECT id_pedido, SUM(precio_total) AS total_pedido
FROM tickets
GROUP BY id_pedido
) subconsulta;



