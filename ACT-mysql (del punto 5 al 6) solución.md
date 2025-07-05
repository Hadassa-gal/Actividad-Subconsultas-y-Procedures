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
   
   CREATE PROCEDURE obtener_direccion_cliente (
       IN cliente_id_input INT
   )
   BEGIN
       SELECT 
           c.nombre AS cliente,
           u.calle_info,
           u.carrera,
           u.calle,
           u.description AS descripcion_direccion
       FROM clientes c
       JOIN ubicacion u ON c.ubicacion_id = u.id
       WHERE c.id = cliente_id_input;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390915836805644359/image.png?ex=6869fed1&is=6868ad51&hm=1b577e026905dad86c12f339b58b5cff90d97f5debb57bf075f11ca6a173d32e&=&format=webp&quality=lossless)

3. Crear un procedimiento que registre un pedido nuevo y sus detalles.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE registrar_pedido_completo (
       IN p_cliente_id INT,
       IN p_empleado_id INT,
       IN p_producto_id INT,
       IN p_estado_id INT,
       IN p_precio DECIMAL(10,2),
       IN p_fecha DATE,
       IN p_cantidad INT
   )
   BEGIN
       DECLARE nuevo_pedido_id INT;
   
       -- Insertar en la tabla pedidos
       INSERT INTO pedidos (cliente_id, empleado_id, producto_id, estado_id, precio, fecha)
       VALUES (p_cliente_id, p_empleado_id, p_producto_id, p_estado_id, p_precio, p_fecha);
   
       -- Obtener el ID del nuevo pedido
       SET nuevo_pedido_id = LAST_INSERT_ID();
   
       -- Insertar en detalles_pedido
       INSERT INTO detalles_pedido (pedido_id, producto_id, cantidad, precio)
       VALUES (nuevo_pedido_id, p_producto_id, p_cantidad, p_precio);
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390921932375068772/image.png?ex=686a047e&is=6868b2fe&hm=3a4b366fb069a3c1ac0bcf1440bd96351c802984515a0cabaac7160f1dfb458d&=&format=webp&quality=lossless)

4. Un procedimiento para calcular el total de ventas de un cliente.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE total_ventas_cliente (
       IN p_cliente_id INT
   )
   BEGIN
       SELECT 
           c.nombre AS cliente,
           SUM(p.precio) AS total_ventas
       FROM clientes c
       JOIN pedidos p ON c.id = p.cliente_id
       WHERE c.id = p_cliente_id
       GROUP BY c.id, c.nombre;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390924117774962849/image.png?ex=686a0687&is=6868b507&hm=7dcf86da6f8b80f0b3d83551e34e80cf8adec1b1edeaa2d1fc9834c2034154b8&=&format=webp&quality=lossless)

5. Crear un procedimiento para obtener los empleados por puesto

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE empleados_por_puesto (
       IN p_puesto_id INT
   )
   BEGIN
       SELECT 
           e.id AS empleado_id,
           e.nombre AS empleado,
           tp.puesto AS puesto,
           e.salario,
           e.fecha_contratacion
       FROM empleados e
       JOIN tipo_puestos tp ON e.puesto_id = tp.id
       WHERE tp.id = p_puesto_id;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390924708584620225/image.png?ex=686a0714&is=6868b594&hm=8fe424e272cd966c826841946356ef4d4890152bc0d6b338b38cab870685f7dd&=&format=webp&quality=lossless)

6. Un procedimiento que actualice el salario de empleados por puesto.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE actualizar_salario_por_puesto (
       IN p_puesto_id INT,
       IN p_porcentaje DECIMAL(5,2)  -- Ej: 10 para +10%, -5 para -5%
   )
   BEGIN
       UPDATE empleados
       SET salario = salario * (1 + p_porcentaje / 100)
       WHERE puesto_id = p_puesto_id;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390926475296903178/image.png?ex=686a08b9&is=6868b739&hm=12456f832c26432ab264d4b9f8afe012306f3fc04299f3f0c8159ae69dd680e2&=&format=webp&quality=lossless)

7. Crear un procedimiento que liste los pedidos entre dos fechas.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE listar_pedidos_por_fecha (
       IN fecha_inicio DATE,
       IN fecha_fin DATE
   )
   BEGIN
       SELECT 
           p.id AS pedido_id,
           c.nombre AS cliente,
           p.precio,
           p.fecha,
           ep.detalle AS estado
       FROM pedidos p
       JOIN clientes c ON p.cliente_id = c.id
       JOIN estado_pedidos ep ON p.estado_id = ep.id
       WHERE p.fecha BETWEEN fecha_inicio AND fecha_fin
       ORDER BY p.fecha;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390927066320338965/image.png?ex=686a0946&is=6868b7c6&hm=f9cf73f5fba059fea4c3ff222b8668f11d477ecbb79a7fcb3275d51857aafa93&=&format=webp&quality=lossless)

8. Un procedimiento para aplicar un descuento a productos de una categoría.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE aplicar_descuento_categoria (
       IN p_tipo_id INT,
       IN p_descuento DECIMAL(5,2)  -- Ej: 10 para 10%, 25.5 para 25.5%
   )
   BEGIN
       UPDATE productos
       SET precio = precio * (1 - p_descuento / 100)
       WHERE tipo_id = p_tipo_id;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390929091900866580/image.png?ex=686a0b29&is=6868b9a9&hm=3c9b09c8c4b24b4a8b1e2cc6676c968fbf0795fd514899f1f6c63e1894b09129&=&format=webp&quality=lossless&width=808&height=790)

9. Crear un procedimiento que liste todos los proveedores de un tipo de producto.

   ```sql
   DELIMITER $$
   
   CREATE PROCEDURE proveedores_por_tipo_producto (
       IN p_tipo_id INT
   )
   BEGIN
       SELECT DISTINCT 
           pr.id AS proveedor_id,
           pr.nombre AS proveedor,
           tp.tipo_nombre AS tipo_producto
       FROM proveedores pr
       JOIN productos_proveedores pp ON pr.id = pp.proveedor_id
       JOIN productos p ON pp.producto_id = p.id
       JOIN tiposproducto tp ON p.tipo_id = tp.id
       WHERE tp.id = p_tipo_id;
   END $$
   
   DELIMITER ;
   ```

   ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390933465695911936/image.png?ex=686a0f3c&is=6868bdbc&hm=086741046b2282b145bf59c2946d9fa5921efebc63f3268bb0bcce8ac8fad382&=&format=webp&quality=lossless)

10. Un procedimiento que devuelva el pedido de mayor valor.

    ```sql
    DELIMITER $$
    
    CREATE PROCEDURE pedido_mayor_valor()
    BEGIN
        SELECT 
            p.id AS pedido_id,
            c.nombre AS cliente,
            p.precio AS valor_total,
            p.fecha
        FROM pedidos p
        JOIN clientes c ON p.cliente_id = c.id
        ORDER BY p.precio DESC
        LIMIT 1;
    END $$
    
    DELIMITER ;
    ```
    
    ![image-20250704125320878](https://media.discordapp.net/attachments/1337463162940817490/1390933954982318090/image.png?ex=686a0fb1&is=6868be31&hm=cf0851b8f923017bf726b0387f1ccfa7e674013cdc75739e7994b3ecb8043fa7&=&format=webp&quality=lossless)