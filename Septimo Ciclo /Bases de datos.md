## En linux:
Como iniciar el contenedor de docker, porque no  tengo activo el demonio por defecto para no gastar recursos de manera innecesaria.

 ```bash
  sudo systemctl start docker
  ```

En este caso el nombre del contenedor creado es sql1, asi que iniciamos ese:

 ```bash
 sudo docker start sql1
  ```

Y listo solo restaria ir dbeaver e iniciar a ocuparlo.

## En windows:
Ya todo esta hecho, si lo necesito es solo de ir y usar...



### 1. **Transacciones en SQL Server**

Una **transacción** es un conjunto de operaciones de base de datos que se ejecutan como una unidad. Se utiliza para garantizar la **atomicidad**, **consistencia**, **aislamiento** y **durabilidad** de las operaciones (propiedades ACID). Si alguna operación dentro de una transacción falla, todas las operaciones anteriores pueden ser revertidas (con `ROLLBACK`), asegurando la integridad de la base de datos.

#### Ejemplo de transacción:

```sql 
BEGIN TRANSACTION;

-- Inserción en la tabla de empleados
INSERT INTO Employees (Name, Position, Salary)
VALUES ('Juan Perez', 'Desarrollador', 5000);

-- Actualización de salario
UPDATE Employees
SET Salary = Salary + 500
WHERE Position = 'Desarrollador';

-- Si todo está bien, confirmamos la transacción
COMMIT TRANSACTION;
```

En este ejemplo:

    La transacción comienza con BEGIN TRANSACTION;.
    Se insertan y actualizan datos en las tablas.
    Si todo se ejecuta correctamente, usamos COMMIT TRANSACTION; para hacer que todos los cambios sean permanentes.
    Si algo falla en medio de la transacción, podemos revertir todo con ROLLBACK TRANSACTION;.

Rollback en caso de error:

```sql 
BEGIN TRANSACTION;

BEGIN TRY
    -- Insertar un nuevo empleado
    INSERT INTO Employees (Name, Position, Salary)
    VALUES ('Carlos Garcia', 'Analista', 4000);
    
    -- Hacer algo que podría fallar, por ejemplo, violar una restricción
    INSERT INTO Employees (Name, Position, Salary)
    VALUES ('Carlos Garcia', 'Analista', 4000); -- Error por duplicado

    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    -- Si algo falla, revertimos todo
    ROLLBACK TRANSACTION;
    PRINT 'Se ha producido un error, la transacción ha sido revertida.';
END CATCH
```

En este ejemplo:

    Si ocurre un error (como un duplicado en la clave primaria), la transacción será revertida con ROLLBACK, y la base de datos no se verá afectada.

## Funciones en SQL Server

Una función en SQL Server es un bloque de código que realiza una operación específica y devuelve un valor. Las funciones pueden ser escalares (devuelven un solo valor) o de tabla (devuelven una tabla).
Ejemplo de función escalar:

Esta función calcula el salario anual de un empleado a partir de su salario mensual.

```sql 
CREATE FUNCTION CalculateAnnualSalary (@MonthlySalary DECIMAL)
RETURNS DECIMAL
AS
BEGIN
    RETURN @MonthlySalary * 12;
END;
```

Uso de la función:

```sql 
SELECT dbo.CalculateAnnualSalary(3000);
```

En este caso, la función CalculateAnnualSalary toma un parámetro (@MonthlySalary) y devuelve el salario anual multiplicando el salario mensual por 12.
Ejemplo de función de tabla:

Una función de tabla devuelve un conjunto de filas y columnas.

```sql CREATE FUNCTION GetEmployeesByPosition (@Position NVARCHAR(50))
RETURNS TABLE
AS
RETURN (
    SELECT Name, Position, Salary
    FROM Employees
    WHERE Position = @Position
);
```

Uso de la función:

```sql SELECT * 
FROM dbo.GetEmployeesByPosition('Desarrollador');
```
En este caso, la función GetEmployeesByPosition devuelve una tabla con los empleados que tienen el puesto de "Desarrollador". Se puede usar la función en una consulta como si fuera una tabla.
Resumen de conceptos:

    Transacción: Agrupación de operaciones que se ejecutan como una unidad. Si ocurre un error, todas las operaciones se revierten.
    Función escalar: Devuelve un solo valor basado en la lógica que se le proporcione.
    Función de tabla: Devuelve un conjunto de resultados (una tabla) que puede ser utilizada en una consulta.


## Bitácoras de transacciones

### Con Triggers (Método manual)

#### Paso 1: Crear la tabla principal

Crear la tabla Empleados que contenga información de los empleados.
```sql
CREATE TABLE Empleados (
	Id INT PRIMARY KEY,
	Nombre NVARCHAR(100),
	Puesto NVARCHAR(100),
	Salario DECIMAL(10,2)
);
```

#### Paso 2: Crear la tabla de bitácora

Esta tabla almacenará los cambios realizados en la tabla Empleados.
```sql
CREATE TABLE Empleados_Bitacora (
	Id INT,
	Nombre NVARCHAR(100),
	Puesto NVARCHAR(100),
	Salario DECIMAL(10,2),
	TipoOperacion NVARCHAR(10),
	FechaOperacion DATETIME DEFAULT GETDATE()
);
```
#### Paso 3: Crear los Triggers

##### Trigger para INSERT

Cada vez que se inserte un registro en Empleados, se guardará en la bitácora.
```sql 
CREATE TRIGGER trg_Empleados_Insert
ON Empleados
AFTER INSERT
AS
	BEGIN
	INSERT INTO Empleados_Bitacora (Id, Nombre, Puesto, Salario, TipoOperacion,
	FechaOperacion)
	SELECT Id, Nombre, Puesto, Salario, 'INSERT', GETDATE()
	FROM inserted;
END;
```

##### Trigger para UPDATE

Cada vez que se actualice un registro, se guardará la versión anterior en la bitácora.
```sql
CREATE TRIGGER trg_Empleados_Update
ON Empleados
AFTER UPDATE
AS
BEGIN
	INSERT INTO Empleados_Bitacora (Id, Nombre, Puesto, Salario, TipoOperacion,
	FechaOperacion)
	SELECT Id, Nombre, Puesto, Salario, 'UPDATE', GETDATE()
	FROM deleted;
END;
```

##### Trigger para DELETE

Cada vez que se elimine un registro, se guardará en la bitácora.
```sql
CREATE TRIGGER trg_Empleados_Delete
ON Empleados
AFTER DELETE
AS
BEGIN
	INSERT INTO Empleados_Bitacora (Id, Nombre, Puesto, Salario, TipoOperacion,
	FechaOperacion)
	SELECT Id, Nombre, Puesto, Salario, 'DELETE', GETDATE()
	FROM deleted;
END;
```

#### Paso 4: Realizar pruebas
* Insertar, actualizar y eliminar datos en la tabla Empleados.
* Consultar la bitácora para verificar que los cambios fueron registrados correctamente.

```sql
SELECT * FROM Empleados_Bitacora;
```
#### Consultas hechas para la verificacion de funcionaliad (insert, update, delete)

Insertamos un nuevo empleado
```sql
INSERT INTO Empleados (Id, Nombre, Puesto, Salario)
VALUES (1, 'Nestor Acebo', 'Tecnico', 3500);
```
  
  Insertamos otro
```sql
INSERT INTO Empleados (Id, Nombre, Puesto, Salario)
VALUES (2, 'Jaime', 'Consultor', 4500);
```
  Verificamos la tabla empleados y vemos si contienen datos,

```sql
SELECT * FROM Empleados;
```
  
Revisando la bitácora para ver las dos datos ingresados

```sql
SELECT * FROM Empleados_Bitacora;
```
  

Actualizar el salario del empleado Nestor

``` sql
UPDATE Empleados
SET Salario = 5000
WHERE Id = 1;
```
  
 Verificamos el registro actualizado

```sql
SELECT * FROM Empleados;
```
  
Verificamos la bitácora para ver el UPDATE

```sql
SELECT * FROM Empleados_Bitacora;
```
  
Eliminamos al empleado Jaime

```sql
DELETE FROM Empleados
WHERE Id = 2;
```

Verificamos la eliminación

```sql
SELECT * FROM Empleados;
```
  
Volvemos a la bitácora para ver el DELETE

```sql
SELECT * FROM Empleados_Bitacora;
```
### Usando SYSTEM_VERSIONING (tablas temporales).

#### Paso 1: Crear la tabla con SYSTEM_VERSIONING

```sql
CREATE TABLE EmpleadosVersionados (
	Id INT PRIMARY KEY,
	Nombre NVARCHAR(100),
	Puesto NVARCHAR(100),
	Salario DECIMAL(10,2),
	FechaInicio DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN
		CONSTRAINT DF_Empleados_FechaInicio DEFAULT SYSUTCDATETIME(),
	FechaFin DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN
		CONSTRAINT DF_Empleados_FechaFin DEFAULT '9999-12-31 23:59:59.9999999',
	PERIOD FOR SYSTEM_TIME (FechaInicio, FechaFin)
) WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmpleadosVersionados_Historial));
```

#### Paso 2: Insertar datos

```sql
INSERT INTO EmpleadosVersionados (Id, Nombre, Puesto, Salario)
VALUES (1, 'Juan Pérez', 'Desarrollador', 3000);
```
#### Paso 3: Actualizar y eliminar datos

```sql
UPDATE EmpleadosVersionados SET Salario = 3500 WHERE Id = 1;
DELETE FROM EmpleadosVersionados WHERE Id = 1;
```
#### Paso 4: Consultar el historial

```sql
SELECT * FROM EmpleadosVersionados FOR SYSTEM_TIME ALL;
```
#### Paso 5: Recuperar datos en un momento específico

```sql
SELECT * FROM EmpleadosVersionados
FOR SYSTEM_TIME AS OF '2024-03-10 09:00:00';
```
