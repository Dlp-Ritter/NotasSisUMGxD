
## 1. Crear y montar volumen (sin docker-compose)
🔹 Paso 1: Crear volumen

``` sh
docker volume create mssql_backups
```

Esto crea un volumen persistente administrado por Docker.

🔹 Paso 2: Iniciar el contenedor montando el volumen

```sh
do`ker run -e 'ACCEPT_EULA=Y' \
    -e 'SA_PASSWORD=TuPassw0rdSeguro' \
    -p 1433:1433 \
    -v mssql_backups:/var/opt/mssql/backups \
    --name sqlserver \
    -d mcr.microsoft.com/mssql/server:2019-latest
```

Esto monta el volumen en /var/opt/mssql/backups dentro del contenedor, útil para backups.

💾 3. Backup y restauración usando volumen

🔹 Backup SQL

```sql
BACKUP DATABASE bodega_simple
TO DISK = '/var/opt/mssql/backups/bodega_simple.bak'
WITH INIT, FORMAT;
```
🔹 Copiar backup al host (si lo deseas)

```sh
docker cp sqlserver:/var/opt/mssql/backups/bodega_simple.bak .
```

🔄 Restaurar backup

Desde SSMS, Azure Data Studio o un cliente sqlcmd:

```sh
-- Verifica si la DB ya existe y bórrala si es necesario
DROP DATABASE IF EXISTS bodega_simple;

-- Restaurar
RESTORE DATABASE bodega_simple
FROM DISK = '/var/opt/mssql/backups/bodega_simple.bak'
WITH MOVE 'bodega_simple' TO '/var/opt/mssql/data/bodega_simple.mdf',
     MOVE 'bodega_simple_log' TO '/var/opt/mssql/data/bodega_simple_log.ldf',
     REPLACE;
```


⚠️ Nota: Los nombres 'bodega_simple' y 'bodega_simple_log' deben coincidir con los logical names del .bak. Usa:

RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backups/bodega_simple.bak';

...para obtenerlos antes de restaurar.
🧾 4. Implementar Bitácora con ejemplo completo
🔸 Crear tabla bitacora (si aún no la tienes):

CREATE TABLE bitacora (
    id_evento INT IDENTITY(1,1) PRIMARY KEY,
    id_usuario INT NOT NULL,
    evento VARCHAR(100) NOT NULL,
    descripcion TEXT NULL,
    fecha_hora DATETIME DEFAULT GETDATE(),
    FOREIGN KEY (id_usuario) REFERENCES usuarios(id_usuario)
);

🔸 Insertar desde aplicación (por ejemplo en login)

INSERT INTO bitacora (id_usuario, evento, descripcion)
VALUES (2, 'login', 'Usuario accedió al sistema');

🔸 Insertar desde trigger (por ejemplo en productos)

CREATE TRIGGER tr_update_producto
ON productos
AFTER UPDATE
AS
BEGIN
    INSERT INTO bitacora (id_usuario, evento, descripcion)
    SELECT 1, 'update_producto', 
           CONCAT('Producto ID ', i.id_producto, ' fue actualizado')
    FROM inserted i;
END;

    Puedes modificar el id_usuario si lo manejas en sesión desde tu app.