# 📦 proyecto-ssis-etl-masive_ingest-exceltoBD

Proyecto de **ingesta masiva de datos** desde múltiples archivos Excel hacia una base de datos SQL Server, utilizando SQL Server Integration Services (SSIS).

---

## 🗂️ Descripción

Este proyecto implementa un proceso ETL (Extract, Transform, Load) completo que:

- **Extrae** datos desde múltiples libros de Excel (uno por tabla)
- **Transforma** y valida los datos en cada Data Flow Task
- **Carga** la información en una base de datos SQL Server relacional

El paquete SSIS incluye una tarea inicial que **limpia las tablas** antes de cada carga, garantizando que los datos siempre estén actualizados sin duplicados.

---

## 🏗️ Arquitectura del proyecto

<img width="880" height="449" alt="image" src="https://github.com/user-attachments/assets/38710356-5dd0-4c91-b165-3f8b9ab8c5b4" />

---

## 📋 Tablas cargadas

| Tabla | Tipo | Descripción |
|-------|------|-------------|
| `CHANNELS` | Dimensión | Canales de venta |
| `COUNTRIES` | Dimensión | Países |
| `PRODUCTS` | Dimensión | Productos |
| `PROMOTIONS` | Dimensión | Promociones |
| `TIMES` | Dimensión | Dimensión de tiempo |
| `CUSTOMERS` | Dimensión | Clientes |
| `COSTS` | Hecho | Costos por producto/canal/tiempo |
| `SALES` | Hecho | Ventas (tabla de hechos principal) |

---

## 🗄️ Estructura de la base de datos

El modelo sigue un **esquema estrella** típico de Data Warehouse:

```
CHANNELS ──┐
COUNTRIES ─┤── CUSTOMERS ─┐
PRODUCTS ──┤               ├──► SALES (Hecho)
PROMOTIONS ┤               │
TIMES ─────┴───────────────┘
           │
           └──────────────────► COSTS (Hecho)
```

> El script completo de creación de la base de datos está en 📄 [`SHDatabase_schema.sql`](./SHDatabase_schema.sql)

Para crear la base de datos ejecuta en SQL Server Management Studio:
```sql
-- 1. Abre el archivo SHDatabase_schema.sql
-- 2. Ejecuta el script completo (F5)
-- Esto creará la BD, todas las tablas y las Foreign Keys
```

---

## 🛠️ Tecnologías utilizadas

- **SQL Server Integration Services (SSIS)**
- **SQL Server** (base de datos destino)
- **Visual Studio 2022** con extensión SSIS
- **Microsoft Excel** (fuente de datos)

---

## 🚀 Cómo ejecutar el proyecto

### Prerrequisitos
- Visual Studio 2022 con SQL Server Data Tools (SSDT)
- SQL Server instalado y corriendo
- Los archivos Excel fuente disponibles

### Pasos

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/jesusdiazitla-lang/proyecto-ssis-etl-masive_ingest-exceltoBD.git
   ```

2. **Crear la base de datos**
   - Abre SQL Server Management Studio
   - Ejecuta el script 📄 [`SHDatabase_schema.sql`](./SHDatabase_schema.sql)

3. **Descargar los archivos Excel fuente**
   - Ve a la carpeta 📁 [`tables/`](./tables) del repositorio
   - Descarga los 8 archivos `.xls` y colócalos en tu carpeta `tables/` local:
     ```
     tables/
     ├── Channels.xls
     ├── Costs.xls
     ├── Countries.xls
     ├── Customers.xls
     ├── Products.xls
     ├── Promotions.xls
     ├── Sales.xls
     └── Times.xls
     ```

4. **Abrir la solución** en Visual Studio
   ```
   CargaMultipleExceltoDB.sln
   ```

5. **Configurar la conexión** a SQL Server en cada Connection Manager

6. **Apuntar los archivos Excel** a su ruta local en cada Excel Connection Manager

7. **Ejecutar el paquete** `Package.dtsx`

---

## 🔄 Flujo de ejecución

1. El **Execute SQL Task** limpia todas las tablas con `DELETE FROM`
2. El **Sequence Container** ejecuta los Data Flow Tasks en orden
3. Cada **Data Flow Task** lee un Excel diferente y lo carga a su tabla correspondiente
4. El orden respeta las **Foreign Keys**: primero dimensiones, luego hechos

---

## 📁 Estructura del repositorio

```
📁 proyecto-ssis-etl-masive_ingest-exceltoBD/
├── 📄 README.md
├── 📄 SHDatabase_schema.sql       ← Script de creación de la BD
├── 📄 CargaMultipleExceltoDB.slnx ← Solución Visual Studio
├── 📁 CargaMultipleExceltoDB/
│   └── 📄 Package.dtsx            ← Paquete principal SSIS
└── 📁 tables/                     ← Archivos Excel fuente
    ├── Channels.xls
    ├── Costs.xls
    ├── Countries.xls
    ├── Customers.xls
    ├── Products.xls
    ├── Promotions.xls
    ├── Sales.xls
    └── Times.xls
```

---

## 👤 Autor

**jesusdiazitla-lang**  
[GitHub](https://github.com/jesusdiazitla-lang) · [LinkedIn](https://linkedin.com/in/TU_USUARIO)

---

## 📌 Próximos pasos

- [ ] Proyecto SSAS — Cubo OLAP para análisis multidimensional de los datos cargados
