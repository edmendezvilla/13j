 1.-Crear un función y un trigger para validar que el numero de cedula del cliente tenga 10 números (no letras) en la tabla cliente.
          DELIMITER //

CREATE FUNCTION validate_nui(nui VARCHAR(10)) RETURNS BOOLEAN
BEGIN
    DECLARE valid BOOLEAN;

    IF LENGTH(nui) = 10 AND nui REGEXP '^[0-9]+$' THEN
        SET valid = TRUE;
    ELSE
        SET valid = FALSE;
    END IF;

    RETURN valid;
END //

DELIMITER ;

FROM client c;
<img src="![alt text](image-1.png)" 

2.Crear un función y un trigger para que cada vez que se inserte un nuevo registro en la tabla item se disminuya el stock de la tabla product.

       CREATE OR REPLACE FUNCTION disminuir_stock()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE product
    SET stock = stock - NEW.quantity
    WHERE id = NEW.product_id;
    RETURN NEW;

CREATE TRIGGER trigger_disminuir_stock
AFTER INSERT ON item
FOR EACH ROW
EXECUTE PROCEDURE disminuir_stock();



<img src="![alt text](image.png)" 
 


3.-Crear un función y un trigger para la tabla invoice donde valide que el campo create_at sea del año actual (fecha sistema).

        CREATE OR REPLACE FUNCTION check_invoice_created_at_year() 
RETURNS TRIGGER AS $$
BEGIN
    -- Verificar si el año de la fecha de creación es igual al año actual
    IF EXTRACT(YEAR FROM NEW.created_at) <> EXTRACT(YEAR FROM CURRENT_DATE) THEN
        RAISE EXCEPTION 'La fecha de creación debe ser del año actual';
    END IF;
    RETURN NEW;
END;

<img src="![alt text](image-2.png)" 

4.- Crear un función y un trigger para la tabla client y validar que el correo tenga un @.

CREATE OR REPLACE FUNCTION validate_email_format()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.email !~* '^.+@.+$' THEN
        RAISE EXCEPTION 'El correo electrónico debe tener un formato válido';
    END IF;
    RETURN NEW;
END;


CREATE TRIGGER check_email_format
BEFORE INSERT OR UPDATE ON client
FOR EACH ROW
EXECUTE PROCEDURE validate_email_format();
<img src="![alt text](image-5.png)" 
<img src="!![alt text](image-6.png)"






