# ACT-mysql (del punto 5 al 6)

TABLA NORMALIZADA:

![Imagen](https://cdn.discordapp.com/attachments/1390717312893587597/1390717328345399317/image.png?ex=686945f1&is=6867f471&hm=eb66d04e7e1c7a595b675cca391b0e56075e3001cf308363d4639d35f6335b3d)

## 5. Subconsultas

1. Consultar el producto más caro en cada categoría.

   ```sql
   SELECT precio AS producto_mas_caro
   FROM productos 
   ORDER BY precio DESC
   LIMIT 1;
   ```

   ![image-20250704111253156](/home/camper/.config/Typora/typora-user-images/image-20250704111253156.png)

2. Encontrar el cliente con mayor total en pedidos.

   ```sql
   SELECT c.nombre AS cliente, COUNT(p.id) AS total_pedidos
   FROM clientes c
   JOIN pedidos p ON c.id = p.cliente_id
   GROUP BY c.id, c.nombre
   ORDER BY total_pedidos DESC
   LIMIT 1;
   ```

   ![image-20250704115416993](/home/camper/.config/Typora/typora-user-images/image-20250704115416993.png)

   ![image-20250704115452976](/home/camper/.config/Typora/typora-user-images/image-20250704115452976.png)

3. Listar empleados que ganan más que el salario promedio.

   ```sql
   SELECT e.nombre AS empleado, e.salario
   FROM empleados e
   WHERE e.salario > (SELECT AVG(salario) FROM empleados);
   ```

   ![image-20250704114452467](/home/camper/.config/Typora/typora-user-images/image-20250704114452467.png)

4. Consultar productos que han sido pedidos más de 5 veces.

   ```sql
   SELECT pr.nombre, COUNT(p.id) AS total_pedidos
   FROM productos pr
   JOIN pedidos p ON p.producto_id = pr.id
   GROUP BY pr.id, pr.nombre
   HAVING total_pedidos > 5;
   ```

   ![image-20250704123035774](/home/camper/.config/Typora/typora-user-images/image-20250704123035774.png)

5. Listar pedidos cuyo total es mayor al promedio de todos los pedidos.

   ```sql
   SELECT id, precio        
   FROM pedidos
   WHERE precio > (SELECT AVG(precio) FROM pedidos)
   ORDER BY precio DESC;
   ```

   ![image-20250704124458469](/home/camper/.config/Typora/typora-user-images/image-20250704124458469.png)

6. Seleccionar los 3 proveedores con más productos.

   ```sql
   SELECT p.nombre, COUNT(pp.producto_id) AS productos_del_proveedor
   FROM productos_proveedores pp
   JOIN proveedores p ON pp.proveedor_id = p.id
   GROUP BY pp.proveedor_id
   ORDER BY productos_del_proveedor DESC
   LIMIT 3;
   ```

   ![image-20250704125320878](/home/camper/.config/Typora/typora-user-images/image-20250704125320878.png)

7. Consultar productos con precio superior al promedio en su tipo.

   ```sql
   SELECT
   FROM productos p
   JOIN tiposproducto tp ON p.tipo_id = tp.id
   WHERE precio > (SELECT AVG(...) FROM ... GROUP BY ...)
   ```

   

8. Mostrar clientes que han realizado más pedidos que la media.

   ```sql
   
   ```

   

9. Encontrar productos cuyo precio es mayor que el promedio de todos los productos.

   ```sql
   
   ```

   

10. Mostrar empleados cuyo salario es menor al promedio del departamento.

    ```sql
    
    ```

## 6. Procedimientos Almacenados

1. Crear un procedimiento para actualizar el precio de todos los productos de un proveedor.

   ```sql
   
   ```

   

2. Un procedimiento que devuelva la dirección de un cliente por ID.

   ```sql
   
   ```

   

3. Crear un procedimiento que registre un pedido nuevo y sus detalles.

   ```sql
   
   ```

   

4. Un procedimiento para calcular el total de ventas de un cliente.

   ```sql
   
   ```

   

5. Crear un procedimiento para obtener los empleados por puesto

   ```sql
   
   ```

   

6. Un procedimiento que actualice el salario de empleados por puesto.

   ```sql
   
   ```

   

7. Crear un procedimiento que liste los pedidos entre dos fechas.

   ```sql
   
   ```

   

8. Un procedimiento para aplicar un descuento a productos de una categoría.

   ```sql
   
   ```

   

9. Crear un procedimiento que liste todos los proveedores de un tipo de producto.

   ```sql
   
   ```

   

10. Un procedimiento que devuelva el pedido de mayor valor.

    ```sql
    
    ```

    
