# EXAMEN_dbSalesWine
USE  master
DROP DATABASE IF EXISTS dbSalesWine
CREATE DATABASE dbSalesWine ;
GO

--USAR BASE DE DATOS  dbSalesWine
USE  dbSalesWine;
GO
----------------TABLA CLIENTE------------------------------------
CREATE TABLE PERSONA (
    IDPER int  NOT NULL IDENTITY(100, 1) UNIQUE,
    DNIPER char(8) UNIQUE,
    NOMPER varchar(80), 
    APEPER varchar(80),
	CELPER char(9),
    EMAPER varchar(100), 
	FECNACPER DATE,
    TIPPER char(1),---persona C (cliente), V (vendedor)  y J (jefe).,
	ESTPER char(1) DEFAULT 'A',
)
GO
----------------TABLA PRODUCTO------------------------------------

CREATE TABLE PRODUCTO (
    CODPRO VARCHAR(3),
    NOMPRO varchar(80) ,
	TIPPRO char(1),
	VOLPRO INT,
	PAISPRO char(1),
    PREPRO DECIMAL (8,2),
    STOCKPRO INT,
    ESTPRO char(1) DEFAULT 'A' ,
    CONSTRAINT PRODUCTO_pk PRIMARY KEY  (CODPRO)
)
GO
----------------TABLA VENTA------------------------------------
CREATE TABLE VENTA (
    IDVEN int  IDENTITY(1, 100) UNIQUE,
   	FECVEN DATETIME DEFAULT GETDATE(), 
	IDVEND INT FOREIGN KEY REFERENCES PERSONA(IDPER) UNIQUE,
	IDCLI INT FOREIGN KEY REFERENCES PERSONA(IDPER)UNIQUE,
	TIPPAGVEN CHAR(1),
	TIPENTRVEN CHAR(1),
	ESTVEN char(1) DEFAULT 'A' , 
)
GO
----------------TABLA VENTA DETALLE------------------------------------
CREATE TABLE VENTA_DETALLE (
   	IDVENDET int IDENTITY(1,1) UNIQUE,
   	IDVEN INT FOREIGN KEY REFERENCES VENTA(IDVEN) UNIQUE, 
	CODPRO VARCHAR(3) FOREIGN KEY REFERENCES PRODUCTO(CODPRO),
	CANVENDET INT,
)
GO

-- RELACIONES PERSONA VENDEDOR y VENTA

ALTER TABLE VENTA 
ADD CONSTRAINT VENTA_VENDEDOR FOREIGN KEY (IDVEND) 
REFERENCES PERSONA (IDPER)   
GO

--RELACIONES PERSONA CLIENTE y VENTA

ALTER TABLE VENTA 
ADD CONSTRAINT VENTA_CLIENTE FOREIGN KEY (IDCLI) 
REFERENCES PERSONA (IDPER)   
GO	

-- RELACIONES VENTA y VENTA_DETALLE  

ALTER TABLE VENTA_DETALLE 
ADD CONSTRAINT VENTA_DETALLE_VENTA FOREIGN KEY (IDVEN) 
REFERENCES VENTA (IDVEN)   
GO

-- RELACIONES PRODUCTO y VENTA_DETALLE 

ALTER TABLE VENTA_DETALLE 
ADD CONSTRAINT VENTA_DETALLE_PRODUCTO FOREIGN KEY (CODPRO) 
REFERENCES PRODUCTO (CODPRO)   
GO		

--FORMATO DE DD/MM/AA
SET DATEFORMAT dmy
GO

----------------------------------------INSERTAR DATOS A LA TABLA  PERSONA-------------------------
INSERT INTO PERSONA
           (DNIPER,NOMPER,APEPER,CELPER,EMAPER,FECNACPER,TIPPER)
     VALUES
    (15288111, 'Adriana','VASQUEZ CARRANZA',991548789,'adriana.vasquez@saleswine.com','10/03/1985','V'),
	(45781236, 'Carlos','GUERRA TASAYCO',987845123,'carlos.guerra@saleswine.com','20/10/1980','J'),
	(15263698, 'Daniel','LOMBARDI PEREZ',998523641,'daniel.lombardi@saleswine.com','06/07/1982','J'),
	(45123698, 'Roberto','PALACIOS CASTILLO',985236417,'roberto.palacios@saleswine.com','15/10/1988','V'),
	(15264477, 'Carlos','PALOMINO FERNANDEZ',984512557,'carlos.palomino@saleswine.com','30/06/1989','V'),
	(45127866, 'Fabricio','ROSALES ZEGARRA',974815231,'fabricio@yahoo.com','02/03/1975','C'),
	(15487865, 'Rosaura','DAVILA SANCHEZ',974815254,'rosaurao@gmail.com','16/07/1979','C'),
	(46632157, 'Noemi','JUAREZ MARTINEZ',984525741,'noemi.juarez@gmail.com','25/09/1979','C'),
	(47258533, 'Issac','SANCHEZ JOBS',953625147,'issac.sanchez@outlook.com','30/10/1995','C'),
	(15258544, 'Fabiana','CARRIZALES CAMPOS',951144236,'fabiana.carrisales@outlook.com','05/04/1997','C'),
	(44712214, 'Valeria','MENDOZA SOLANO',972544681,'valeria.mendoza@yahoo.com','06/06/1997','C')
GO
SELECT * FROM PERSONA
----------------------------------------INSERTAR DATOS A LA TABLA  PRODUCTO-------------------------
INSERT INTO PRODUCTO
           (CODPRO,NOMPRO,TIPPRO,VOLPRO,PAISPRO,PREPRO,STOCKPRO)
     VALUES
('P01','Ramos Pinto Porto','V','750','P','119.00','60'),
('P02','Santa Julia Cabernet','V','750','A','119.00','45'),
('P03','Pulenta Estate Cabernet Sauvignon','V','750','A','189.90','70'),
('P04','La Rioja Alta Via Alberdi','V','500','E','540.00','80'),
('P05','Amayna Pinot Noir','V','750','C','774.20','100'),
('P06','Pisco Don Santiago Mosto Verde Italia','P','750','P','59.00','75'),
('P07','Pisco Portón Mosto Verde Torontel','P','750','P','89.00','100'),
('P08','Tequila Olmeca Blanco','T','500','M','54.90','85'),
('P09','Tequila Olmeca Reposado','T','750','M','54.90','85'),
('P10','Black Whiskey Don Michael','W','750','P','159.90','70'),
('P11','Whisky Chivas Regal 12 Años','W','500','E','89.90','70')
GO
	


SELECT * FROM PRODUCTO
----------------------------------------INSERTAR DATOS A LA TABLA VENTA------------------------
INSERT INTO VENTA
     (IDVEND,IDCLI,TIPPAGVEN,TIPENTRVEN)
     VALUES
   (100,105,'E','D'),
   (103,107,'T','T'),
   (104,110,'Y','D'),
   (100,106,'Y','T'),
   (103,105,'E','T'),
   (100,109,'P','T'),
   (100,108,'T','T')
GO

SELECT  * FROM VENTA
----------------------------------------INSERTAR DATOS A LA TABLA  VENTA_DETALLE-------------------------
INSERT INTO VENTA_DETALLE
           (IDVEN,CODPRO,CANVENDET)
     VALUES
    (1,'P01',5),
	(1,'P06',2),
	(2,'P01',2),
	(3,'P05',6),
	(3,'P02',10),
	(3,'P03',15),
	(3,'P04',8),
	(4,'P07',6),
    (4,'P01',12),
	(4,'P03',8),
	(4,'P06',7),
	(5,'P08',6),
	(5,'P10',12),
	(5,'P04',8),
    (6,'P02',4),
	(6,'P11',10),
	(6,'P03',2),
	(7,'P04',6),
	(7,'P02',3),
	(7,'P06',1)

GO
SELECT  * FROM VENTA_DETALLE


---------------------------------------------------VISTA DE PERSONA--------------------------------------------

SELECT
    IDPER AS ID,
    DNIPER AS DNI,
	CONCAT(APEPER,', ',NOMPER) AS 'PERSONA',
	CELPER AS CELULAR,
	EMAPER AS EMAIL,
	FORMAT(FECNACPER, 'dd-MMM-yyyy ')as 'FEC.NACIMIENTO',
	CASE TIPPER 
        WHEN 'V' THEN 'Vendedor' 
        WHEN 'J' THEN 'Jefe'
        WHEN 'C' THEN 'Cliente'
    END AS 'Tipo'
FROM PERSONA 
---------------------------------------------------VISTA DE PRODUCTO------------------------------
SELECT
    CODPRO AS CODIGO,
    NOMPRO AS PRODUCTO,
	CASE TIPPRO 
        WHEN 'V' THEN 'VINO' 
        WHEN 'P' THEN 'PISCO'
        WHEN 'T' THEN 'TEQUILA'
		WHEN 'W' THEN 'WHISQUI'
    END AS 'TIPO',
	CONCAT(VOLPRO,'ml.')   AS 'PRECIO ',
		CASE PAISPRO 
        WHEN 'P' THEN 'Peru' 
        WHEN 'A' THEN 'Argentina'
        WHEN 'E' THEN 'España'
		WHEN 'C' THEN 'Chile'
		WHEN 'M' THEN 'Mexico'
    END AS 'PAIS',
	CONCAT('S/.',PREPRO)   AS 'PRECIO ',
	STOCKPRO AS STOCK,
	CASE ESTPRO 
        WHEN 'A' THEN 'Activo' 
    END AS 'ESTADO'
FROM PRODUCTO
-----------------------------------------------VISTA VENTA-----------------------------------------------------


SELECT 
VENTA.IDVEN AS VENTA,
FORMAT(FECVEN, 'dd-MM-yy - H.MM') AS 'FEC.VENTA',
CONCAT (UPPER (VENDEDOR.APEPER), ', ',VENDEDOR.NOMPER) AS VENDEDOR,
CONCAT (UPPER (CLIENTE.APEPER), ', ',CLIENTE.NOMPER) AS CLIENTE,
CASE VENTA.TIPPAGVEN
	WHEN 'E' THEN 'EFECTIVO'
	WHEN 'T' THEN 'TARJETA'
	WHEN 'Y' THEN 'YAPE'
	WHEN 'P' THEN 'PLIN'
END AS 'TIPO PAGO',
CASE VENTA.TIPENTRVEN 
	WHEN 'D' THEN 'Delivery'
	WHEN 'T' THEN 'Tienda'
END AS 'TIPO ENTREGA',
CASE VENTA.ESTVEN
	WHEN 'A' THEN 'Activo'
	WHEN 'I' THEN 'Inactivo'
END AS 'EST.VENTA'
FROM VENTA 
INNER JOIN PERSONA VENDEDOR ON VENTA.IDVEND = VENDEDOR.IDPER
INNER JOIN PERSONA CLIENTE ON VENTA.IDCLI = CLIENTE.IDPER;	

--------------------------------------------------------------VISTA VENTA_DETALLE----------------------------------------------

SELECT
    IDVENDET AS 'ID DETALLE',
    IDVEN AS 'ID VENTA',
	NOMPRO AS PRODUCTO,
	CANVENDET AS CANTIDAD
FROM VENTA_DETALLE
INNER JOIN PRODUCTO ON VENTA_DETALLE.CODPRO = PRODUCTO.CODPRO

----------------------------------------------------------FIN------------------------------------------------------------------
