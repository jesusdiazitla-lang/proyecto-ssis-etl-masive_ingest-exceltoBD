proyecto-ssis-etl-masive_ingest-exceltoBD
Proyecto de ingesta masiva de datos desde múltiples archivos Excel hacia una base de datos SQL Server, utilizando SQL Server Integration Services (SSIS).

🗂️ Descripción
Este proyecto implementa un proceso ETL (Extract, Transform, Load) completo que:

Extrae datos desde múltiples libros de Excel (uno por tabla)
Transforma y valida los datos en cada Data Flow Task
Carga la información en una base de datos SQL Server relacional

El paquete SSIS incluye una tarea inicial que limpia las tablas antes de cada carga, garantizando que los datos siempre estén actualizados sin duplicados.
Arquitectura del proyecto:
<img width="880" height="449" alt="image" src="https://github.com/user-attachments/assets/38710356-5dd0-4c91-b165-3f8b9ab8c5b4" />

Tablas cargadas
TablaDescripciónCHANNELSCanales de ventaCOUNTRIESPaísesPRODUCTSProductosPROMOTIONSPromocionesTIMESDimensión de tiempoCUSTOMERSClientesCOSTSCostosSALESVentas (tabla de hechos)

🛠️ Tecnologías utilizadas

SQL Server Integration Services (SSIS)
SQL Server (base de datos destino)
Visual Studio 2022 con extensión SSIS
Microsoft Excel (fuente de datos)


🚀 Cómo ejecutar el proyecto
Prerrequisitos

Visual Studio 2022 con SQL Server Data Tools (SSDT)
SQL Server instalado y corriendo
Los archivos Excel fuente disponibles

Pasos

Clonar el repositorio

bash   git clone https://github.com/jesusdiazitla-lang/proyecto-ssis-etl-masive_ingest-exceltoBD.git

Abrir la solución en Visual Studio

   CargaMultipleExceltoDB.sln

Configurar la conexión a SQL Server en cada Connection Manager
Apuntar los archivos Excel a su ruta local en cada Flat File / Excel Connection Manager
Ejecutar el paquete Package.dtsx

Flujo de ejecución

El Execute SQL Task limpia todas las tablas con DELETE FROM
El Sequence Container ejecuta los Data Flow Tasks en orden
Cada Data Flow Task lee un Excel diferente y lo carga a su tabla correspondiente
El orden respeta las Foreign Keys: primero dimensiones, luego hechos
