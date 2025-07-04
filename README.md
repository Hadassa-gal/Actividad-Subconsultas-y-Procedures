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

   ![image-20250704111253156](https://media.discordapp.net/attachments/1337463162940817490/1390754346207019130/image.png?ex=6869686b&is=686816eb&hm=d679ac6c0109acd8433bdb65325a6bea12e49828c499a5dad031f5d52009198c&=&format=webp&quality=lossless&width=877&height=138)

2. Encontrar el cliente con mayor total en pedidos.

   ```sql
   SELECT c.nombre AS cliente, COUNT(p.id) AS total_pedidos
   FROM clientes c
   JOIN pedidos p ON c.id = p.cliente_id
   GROUP BY c.id, c.nombre
   ORDER BY total_pedidos DESC
   LIMIT 1;
   ```

   ![image-20250704115416993](https://media.discordapp.net/attachments/1337463162940817490/1390754527531241584/image.png?ex=68696896&is=68681716&hm=c1b0c445aba4f5c15e5f6ab61fc8c7fffe4bad943060113135884f16c96f8355&=&format=webp&quality=lossless&width=877&height=470)

3. Listar empleados que ganan más que el salario promedio.

   ```sql
   SELECT e.nombre AS empleado, e.salario
   FROM empleados e
   WHERE e.salario > (SELECT AVG(salario) FROM empleados);
   ```

   ![image-20250704114452467](https://media.discordapp.net/attachments/1337463162940817490/1390754579565641808/image.png?ex=686968a2&is=68681722&hm=2612eb5be39046464e11645c58e2d4962e4732329f31fab247073299c6a168d5&=&format=webp&quality=lossless&width=877&height=186)

4. Consultar productos que han sido pedidos más de 5 veces.

   ```sql
   SELECT pr.nombre, COUNT(p.id) AS total_pedidos
   FROM productos pr
   JOIN pedidos p ON p.producto_id = pr.id
   GROUP BY pr.id, pr.nombre
   HAVING total_pedidos > 5;
   ```

   ![image-20250704123035774](https://media.discordapp.net/attachments/1337463162940817490/1390754622213329008/image.png?ex=686968ac&is=6868172c&hm=485a17a4522839d408ea7bd8decac6578269d8cdfb641f811d195c901bbb17a6&=&format=webp&quality=lossless&width=877&height=285)

5. Listar pedidos cuyo total es mayor al promedio de todos los pedidos.

   ```sql
   SELECT id, precio        
   FROM pedidos
   WHERE precio > (SELECT AVG(precio) FROM pedidos)
   ORDER BY precio DESC;
   ```

   ![image-20250704124458469](https://media.discordapp.net/attachments/1337463162940817490/1390754693499715756/image.png?ex=686968bd&is=6868173d&hm=38ead235602a912f5bada90eb0b6ab52a4859918d09383c692253e92f74f633f&=&format=webp&quality=lossless&width=877&height=342)

6. Seleccionar los 3 proveedores con más productos.

   ```sql
   SELECT p.nombre, COUNT(pp.producto_id) AS productos_del_proveedor
   FROM productos_proveedores pp
   JOIN proveedores p ON pp.proveedor_id = p.id
   GROUP BY pp.proveedor_id
   ORDER BY productos_del_proveedor DESC
   LIMIT 3;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390754760529018880/image.png?ex=686968cd&is=6868174d&hm=cbc245c5ca792dbfb7d5fec0708705de2c844e3cb1873f26ea81ba057ba33890&=&format=webp&quality=lossless&width=877&height=424)

7. Consultar productos con precio superior al promedio en su tipo.

   ```sql
   SELECT p.nombre AS producto, p.precio, tp.tipo_nombre
   FROM productos p
   JOIN tiposproducto tp ON p.tipo_id = tp.id
   WHERE p.precio > ( SELECT AVG(p2.precio) FROM productos p2 WHERE p2.tipo_id = p.tipo_id);
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390783215178088491/image.png?ex=6869834e&is=686831ce&hm=ef08f2e69b2659bfba453df11da89196a91f4b85da2228838fa8b95c51ad2a34&=&format=webp&quality=lossless&width=877&height=310)

   

8. Mostrar clientes que han realizado más pedidos que la media.

   ```sql
   SELECT c.nombre AS cliente, COUNT(p.id)
   FROM clientes c
   JOIN pedidos p ON c.id = p.cliente_id
   GROUP BY c.id, c.nombre
   HAVING COUNT(p.id) > (SELECT AVG(pedidos_por_cliente)
                        FROM ( SELECT COUNT(id) AS pedidos_por_cliente
                              FROM pedidos 
                              GROUP BY cliente_id) AS sub);
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390793912314232902/image.png?ex=68698d44&is=68683bc4&hm=89e6f303b9613a812d743c4c55a14eb54f2199ae6748b7b32e07bf608de82710&=&format=webp&quality=lossless&width=877&height=261)

9. Encontrar productos cuyo precio es mayor que el promedio de todos los productos.

   ```sql
   SELECT p.nombre AS producto, p.precio
   FROM productos p
   WHERE p.precio > (SELECT AVG(precio) FROM productos);
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390796121558876240/image.png?ex=68698f53&is=68683dd3&hm=f84b6537ce82a5b123c5d02ecd9fd05137e6019afab5b34256505cfe70122138&=&format=webp&quality=lossless&width=877&height=349)

10. Mostrar empleados cuyo salario es menor al promedio del departamento.

    (departamento = puesto del empleado?)

    ```sql
    SELECT e.nombre AS empleado, e.salario
    FROM empleados e
    WHERE e.salario < (SELECT AVG(som) FROM (SELECT COUNT(e2.id) AS som FROM empleados e2 GROUP BY puesto_id) AS sub);
    
    SELECT AVG(e.salario)
    FROM empleados e;
    
    SELECT e.nombre AS empleado, e.salario, e.puesto_id
    FROM empleados e;
    ```

    ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390799994256359425/image.png?ex=686992ee&is=6868416e&hm=8edaec98a9ba5cb99705a9b653b12cff6468b709fbe0289da1134a8c88402ade&=&format=webp&quality=lossless&width=877&height=491)

## 6. Procedimientos Almacenados

1. Crear un procedimiento para actualizar el precio de todos los productos de un proveedor.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE actualizar_precios_proveedor (
       IN proveedor_id_input INT,
       IN porcentaje_aumentar DECIMAL(5,2)
   )
   BEGIN
       UPDATE productos p
       JOIN productos_proveedores pp ON p.id = pp.producto_id
       SET p.precio = p.precio * (1 + porcentaje_aumentar / 100)
       WHERE pp.proveedor_id = proveedor_id_input;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390820474224640151/image.png?ex=6869a601&is=68685481&hm=d01ee81d15164bdee6b187e1f30421a6531e92bf30edab21a430c2298b258254&=&format=webp&quality=lossless&width=459&height=726)

2. Un procedimiento que devuelva la dirección de un cliente por ID.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE actualizar_precios_proveedor (
       IN proveedor_id_input INT
   )
   BEGIN
       SELECT ;
   END $$
   
   DELIMITER ;
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

    
