# Procedimientos

1. Guardar en la base de datos cada que un vehículo pase por un peaje, verificando que el usuario tiene saldo disponible y el precio del peaje dependiendo de la categoría del vehículo si es el especial el valor especial de lo contrario el valor por defecto

```sql
DELIMITER $$

USE `peajes`$$

DROP PROCEDURE IF EXISTS `p_insertar_paso`$$
CREATE DEFINER=`root`@`localhost` PROCEDURE `p_insertar_paso`(IN v_peaje_id INT, IN v_rfid VARCHAR(15))
BEGIN
        DECLARE v_propietario_id INT;
        DECLARE v_id_persona INT;
        DECLARE v_pspagos DEC(10,2);
        DECLARE v_psvalor_especial DEC(10,2);
    DECLARE v_valor_a_pagar DEC(10,2);
    DECLARE v_saldo_disponible DEC(10,2);
        /*Consultar el valor generico de pago y codigo del vehiculo*/
        SELECT    categoria.psvalor_por_defecto, propietario.id
        INTO    v_pspagos, v_propietario_id
        FROM     propietarios propietario,
            vehiculos vehiculo,
            categorias categoria
        WHERE    propietario.vehiculo_id = vehiculo.id AND
            categoria.id = vehiculo.categoria_id AND
            propietario.rfid = v_rfid;
        /*Punto 1 - Aquí usted debe intentar buscar si el peaje tiene un valor especial*/
        /*TO DO*/
    -- Traigo el valor del peaje especial
    SELECT cp.psvalor
    INTO v_psvalor_especial
    FROM
      propietarios p,
      vehiculos v,
      categorias c,
      categorias_peajes cp,
      peajes pe
    WHERE
      p.vehiculo_id = v.id
      AND
      c.id = v.categoria_id
      AND
      p.rfid = v_rfid
      AND
      pe.id = cp.peajes_id
      AND
      c.id = cp.categorias_id
      AND
      pe.id = v_peaje_id;
    -- Añado el valor a pagar si es especial o si no el por defecto
    IF v_psvalor_especial IS NULL THEN
        SET v_valor_a_pagar = v_pspagos;
    ELSE
      SET v_valor_a_pagar = v_psvalor_especial;
      END IF;
        /*Punto 2 - Aquí usted debe intentar Validar si hay suficiente dinero para cubrir
                el paso del peaje*/
        /*TO DO*/
    -- Traigo el saldo de la persona con el rfid
    SELECT pssaldo
    INTO v_saldo_disponible
    FROM
      propietarios p,
      personas pe
    WHERE
      p.persona_id = pe.id
      AND
      p.rfid = v_rfid;
    -- Verifico que tenga dinero para pagar sino tiene que saque un error
    -- si tiene que lo inserte en paso del peaje
    IF v_saldo_disponible < v_valor_a_pagar THEN
      SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT =  'No hay cantidad suficiente para pasar por el peaje, haga una recarga';
    END IF;
    INSERT INTO pasos
        ( peaje_id,    propietario_id,    pspago )
        VALUES
        ( v_peaje_id,    v_propietario_id,    v_valor_a_pagar);
        /*Punto 3 - Aquí usted debe debitar del saldo de la persona el valor del peaje*/
    SELECT p.id
    INTO v_id_persona
    FROM
      propietarios p,
      personas pe
    WHERE
      p.persona_id = pe.id
      AND
      p.rfid = v_rfid;
    UPDATE personas
    SET pssaldo = v_saldo_disponible -  v_valor_a_pagar
    WHERE id = v_id_persona;
        /*TO DO*/
        END$$

DELIMITER ;
```



