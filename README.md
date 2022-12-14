# <span style="color:#009688">M02 UF2</span>

## <span style="color:#4dd0e1">LENGUAJE SQL</span>

Las sentencias de bases de datos se pueden agrupar en:

- [ ] <span style="color:#009688">DDL</span> (<span style="color:#4dd0e1">Data Definition Language</span>): Definición de la estructura de almacenamiento de datos. Create, drop, alter…

- [ ] <span style="color:#009688">DML</span> (<span style="color:#4dd0e1">Data Manipulation Language</span>): Consulta y modificación de datos. Select, Insert, update, delete…

- [ ] <span style="color:#009688">DCL</span> (<span style="color:#4dd0e1">Data Control Language</span>): administra el acceso a los datos. Grant, revoke…

### Ordenes <span style="color:#009688">DDL</span> sobre base de datos

Para crear una base de datos se realiza con la orden ``CREATE DATABASE`` aunque ``CREATE SCHEMA`` es sinónima, es decir que hará lo mismo
    
    CREATE DATABASE NOMBRE_SCHEMA

Con la orden ``DROP [DATABASE | SCHEMA]`` borramos una base de datos

	DROP DATABASE NOMBRE_SCHEMA

### Sintaxis de <span style="color:#009688">CREATE TABLE</span> (crear tabla)

    CREATE TABLE nombre_tabla (
        nombre_columna tipocolumna [restricciones_columna],
        [nombre_columna tipocolumna [restricciones_columna], ]
        [restricciones_tabla]
    );

Donde las <span style="color:#4dd0e1">restricciones_columna</span> pueden ser:
- <span style="color:#009688">PRIMARY KEY</span> 🡪 Especifica que esta columna es clave primaria.
NOTA: Una clave primaria puede contener hasta 16 columnas.
  - No puede haber más de una clave primaria por tabla.
  - Las columnas que la definen deben ser NOT NULL. (Se hace por defecto).
- <span style="color:#009688">FOREING KEY</span> 🡪 Permite establecer una clave en esta tabla con la clave primaria de otra.
- <span style="color:#009688">NOT NULL</span> 🡪 Especifica que la columna debe recibir un valor en la creación o la modificación. La propiedad not null no se admite en el ALTER TABLE.
- <span style="color:#009688">NULL</span> 🡪 Especifica que la columna puede tomar valores null.
- <span style="color:#009688">DEFAULT</span> 🡪 Fuerza un valor para este campo si no existe.
- <span style="color:#009688">AUTO_INCREMENT</span> 🡪 Propiedad aplicada únicamente a columna numérica entera y permite que el sistema genera automáticamente los números de manera incremental.
- <span style="color:#009688">UNIQUE</span> 🡪 Permite asignar un valor único a un campo, por lo que todos los valores son diferentes en esa columna. Puede haber varias por tabla.
- <span style="color:#009688">CHECK</span> 🡪 Especifica una comprobación en el valor de esta columna. 

### Ejemplo
```roomsql
    CREATE TABLE PROFESOR (
        ID INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
        NOMBRE VARCHAR(100) NOT NULL,
        APELLIDO VARCHAR(100) NULL,
        EDAD DATE NOT NULL,
        PROVINCIA VARCHAR(100) DEFAULT 'Barcelona',
        SUELDO INT CHECK(> 5000),
        ASIGNATURA VARCHAR(100) UNIQUE,
    ) AUTO_INCREMENT = 1000;
```
* Si no especifico el <span style="color:#6aa84f">AUTO_INCREMENT</span> empieza en 1.
* Un único <span style="color:#6aa84f">AUTO_INCREMENT</span> por tabla y siempre en la <span style="color:#4dd0e1">Primary Key</span>.

### Ejemplo
```roomsql
    create table usuario(
        numero int auto_increment,
        documento char(8),
        nombre varchar(30),
        estudios enum('ninguno','primario','secundario','universitario'),
        primary key (numero)
    )
```
* Forma exclusiva de MySql para crear la <span style="color:#4dd0e1">Primary Key</span>.


### <span style="color:#6aa84f">CONSTRAINTS</span>

    CREATE TABLE nombre_tabla (
        nombre_columna tipocolumna [restricciones_columna],
        [nombre_columna tipocolumna [restricciones_columna], ]
        [restricciones_tabla]
    );

Podemos, del mismo modo, crear restricciones <span style="color:#6aa84f">(CONSTRAINTS)</span> al final de la especificación de tablas (<span style="color:#4dd0e1">restricciones_tabla</span>) con los comandos:
- <span style="color:#6aa84f">CONSTRAINT</span> nombrerestriccion <span style="color:#009688">DEFAULT</span> valor FOR nombrecolumna
- <span style="color:#6aa84f">CONSTRAINT</span> nombrerestriccion <span style="color:#009688">PRIMARY KEY</span> (nombrecolumna1,....)
  - NOTA: Es la única manera de hacer una clave primaria compuesta.
- <span style="color:#6aa84f">CONSTRAINT</span> nombrerestriccion <span style="color:#009688">UNIQUE</span> (nombrecolumna1, ...)
- <span style="color:#6aa84f">CONSTRAINT</span> FK_ nombrerest <span style="color:#009688">FOREIGN KEY</span> (nombrecol) <span style="color:#009688">REFERENCES</span> NombreTabla (columnaId)
  - NOTA: Es la única manera de hacer una clave foranea.

### Ejemplo

```roomsql
    CREATE TABLE PROFESOR (
        ID INT NOT NULL AUTO_INCREMENT,
        NOMBRE VARCHAR(100),
        APELLIDO VARCHAR(100),
        EDAD DATE,
        PROVINCIA VARCHAR(100),
        SUELDO INT,
        ID_ASIGNATURA VARCHAR(100),
        CONSTRAINT PK_PROFESOR PRIMARY KEY (ID),
        CONSTRAINT UNIQUE_PROFESOR_ASIGNATURA UNIQUE (NOMBRE),
        CONSTRAINT PROVINCIA_DEFAULT DEFAULT 'Barcelona' FOR PROVINCIA,
        CONSTRAINT FK_PROFESOR_ASIGNATURA FOREIGN KEY (ID_ASIGNATURA) REFERENCES ASINTATURAS (ID)
    );
```

## <span style="color:#009688">Actividad 1</span>
Dadas las tablas, <span style="color:#4dd0e1">CLIENTES</span> y <span style="color:#4dd0e1">FACTURAS</span>, crea un script para cada una de ellas:

    CLIENTES
    CIF (PK)
    Nombre (no nulo)
    Direccion
    Poblacion (default ‘Barcelona’)
    Web
    Correo
---

    FACTURAS
    Idfactura (PK - autoincremental empieza en 100 y no nula)
    Fechafactura
    Total (número con dos decimales)
    Iva (check de mínimo 21)
    Descuento (Permite nulos)
    CIF (FK CLIENTE)

<details>
<summary><span style="color:#6aa84f;font-size:150%">Solución</span></summary>

```roomsql
    CREATE TABLE CLIENTES (
        CIF char(9) primary key auto_increment,
        nombre varchar(30) not null,
        direccion varchar(100),
        poblacion varchar(100) default 'Barcelona',
        web varchar(60),
        correo varchar(40)
    );
    
    CREATE TABLE facturas (
        Idfactura int auto_increment primary key not null,
        Fechafactura date,
        total decimal(10,2),
        iva decimal(10,2),
        descuento decimal(10,2) check(>20) null,
        CIF char(9) ,
        constraint fk_clientes_facturas foreign key(CIF) references clientes(CIF)
    ) auto_increment = 100;
```
</details>

## <span style="color:#009688">Actividad 2</span>
Ahora ejecuta el script en la base de datos.

---
