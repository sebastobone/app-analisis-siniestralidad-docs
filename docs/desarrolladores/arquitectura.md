<!--markdownlint-disable MD007-->

# Arquitectura

Visión general del stack tecnológico y los componentes principales de la aplicación.

## Capas de la aplicación

```mermaid
graph TB
    subgraph Frontend["Frontend (Jinja2 + Vanilla JS)"]
        HTML["HTML Templates<br/>src/templates"]
        JS["JavaScript Modules<br/>src/static"]
    end

    subgraph ExcelClient["Plantilla Excel (cliente)"]
        VBA["Macros VBA<br/>src/vba"]
    end

    subgraph API["FastAPI — src/app.py"]
        direction LR
        R_AUTH["auth.py<br/>Login · JWT · sesión"]
        R_ADMIN["admin.py<br/>CRUD usuarios · roles"]
        R_MAESTRO["maestro_analisis.py<br/>Crear / listar análisis"]
        R_ENT["entradas.py<br/>Subir datos / queries"]
        R_CTRL["controles_informacion.py<br/>Generar controles"]
        R_PLANT["metodos_plantilla.py<br/>Plantillas de Excel"]
        R_COM["comunes.py<br/>Descargas · metadatos"]
    end

    subgraph Core["Lógica de negocio"]
        direction LR
        INFO["informacion/<br/>tera_connect · carga_manual<br/>almacenamiento · mocks"]
        CTRL["controles_informacion/<br/>generacion · cuadre_contable<br/>sap · evidencias"]
        MET["metodos_plantilla/<br/>generar (xlwings) · preparar<br/>resultados · guardar_traer · tablas_resumen"]
        BASES["bases/<br/>base_siniestros<br/>base_primas_expuestos<br/>completar_diagonal (chainladder)"]
        VAL["validation/<br/>entradas · queries<br/>segmentacion"]
    end

    subgraph Infra["Infraestructura"]
        DB["db.py + models.py<br/>SQLite · SQLModel<br/>Usuario · Analisis · Metadata"]
        CFG["configuracion.py<br/>.env.private"]
        LOG["logger_config.py<br/>Loguru"]
        UTILS["utils.py · constantes.py"]
    end

    subgraph Ext["Fuentes de datos externas"]
        TERA["Teradata<br/>teradatasql"]
        UPLOAD["Archivos del usuario<br/>(carga manual)"]
    end

    subgraph Storage["Almacenamiento local"]
        SQLITE["data/database.db"]
        PARQUET["data/{analisis_id}/<br/>*.parquet · *.xlsm"]
    end

    HTML <--> JS
    JS <-->|"Fetch API"| API
    VBA <-->|"HTTP directo"| API
    API --> Core
    Core --> Infra
    Core <--> Ext
    Infra <--> Storage
```

## Backend

- **Framework:** [FastAPI](https://fastapi.tiangolo.com/) (Python), punto de entrada `src/app.py`.
- **Routers:** `auth.py` (login, JWT, sesión), `admin.py` (CRUD de usuarios y roles), `maestro_analisis.py` (crear/listar análisis), `entradas.py` (subir datos y queries), `controles_informacion.py` (generar controles), `metodos_plantilla.py` (plantillas de Excel), `comunes.py` (descargas y metadatos).
- **Lógica de negocio**, organizada por dominio: `informacion/` (extracción Teradata, carga manual, almacenamiento), `controles_informacion/` (generación de controles, cuadre contable, SAP, evidencias), `metodos_plantilla/` (generar, preparar, resultados, guardar/traer, tablas resumen), `bases/` (bases de siniestros/primas/expuestos, completar diagonal con `chainladder`), `validation/` (entradas, queries, segmentación).
- **Procesamiento de datos:** [Polars](https://pola.rs/), usando `LazyFrame` para optimizar los pipelines internos.
- **Verificación de tipos:** [`zuban`](https://github.com/zubanls/zuban).
- **Logging:** [Loguru](https://github.com/Delgan/loguru), configurado en `logger_config.py`.
- **Notificaciones en tiempo real:** *Server-Sent Events (SSE)*, soportado nativamente por FastAPI.

## Frontend

- **Renderizado del lado del servidor:** plantillas [Jinja2](https://jinja.palletsprojects.com/) en `src/templates`.
- **Interactividad:** módulos de JavaScript "vanilla" (sin framework) en `src/static`, con verificación de tipos propia (ver changelog v1.2.0) que consumen la API vía `fetch`.
- `construir_static.py` construye/empaqueta los archivos estáticos.

## Cliente de plantilla Excel (VBA)

- Además del frontend web, las **macros VBA** de la plantilla de Excel actúan como un segundo cliente de la API: hacen peticiones HTTP directamente al backend (por ejemplo, para guardar o traer estimaciones), autenticadas con un token único por plantilla y usuario.
- Este es el mecanismo de interacción en **tiempo de ejecución**, una vez el usuario ya tiene el archivo `.xlsm` — distinto de cómo se genera el archivo en primer lugar (ver [Plantillas de Excel](#plantillas-de-excel)).

## Almacenamiento

- **Base de datos:** SQLite (`data/database.db`), con [SQLModel](https://sqlmodel.tiangolo.com/) como ORM. Tablas principales: `Usuario`, `Analisis`, `Metadata`.
- **Archivos por análisis:** cada análisis tiene su propia carpeta `data/{analisis_id}/`, con archivos `.parquet` (datos procesados) y `.xlsm` (plantillas de Excel).
- Copias de seguridad de `data/` mediante el script `db_backup.py`.

### Modelo de datos

```mermaid
erDiagram
    Usuario {
        int id PK
        str nombre_usuario
        str contrasena_hash
        str rol "admin | actuario | auditor"
        str[] negocios
        bool activo
        datetime fecha_hora_creacion
        datetime fecha_hora_actualizacion
        int usuario_creador
        int usuario_actualizador
        datetime fecha_hora_ultimo_ingreso
    }

    Analisis {
        int id PK
        str nombre
        str negocio
        str tipo_analisis "triangulos | entremes"
        date mes_inicio
        date mes_corte
        bool cierre_contable
        str[] aperturas_siniestros
        str[] aperturas_primas
        str[] aperturas_expuestos
        int usuario_creador FK
        int usuario_actualizador FK
        datetime fecha_hora_creacion
        datetime fecha_hora_actualizacion
    }

    Metadata {
        int id PK
        str ruta_archivo
        str nombre_original
        str tipo_archivo "entrada | query | segmentacion | plantilla | output | reporte"
        str origen "extraccion_db | carga_manual | generado_sistema"
        str etapa_proceso "crudo | pre_cuadre | post_cuadre | consolidacion | analisis"
        str tipo_entrada "siniestros | primas | expuestos "
        str tipo_control
        bool plantilla_cargada
        int analisis_id FK
        int[] archivos_padres
        int numero_filas
        datetime fecha_hora_creacion
        datetime fecha_hora_actualizacion
        int usuario_creador FK
        int usuario_actualizador FK
    }

    Usuario ||--o{ Analisis : "crea / actualiza"
    Analisis ||--o{ Metadata : "tiene"
```

## Plantillas de Excel

Las estimaciones (triángulos y entremés) se realizan en plantillas de Excel con macros **VBA**. El backend y la plantilla interactúan en dos momentos distintos:

- **Descarga del archivo:** al descargar una plantilla, `routers/comunes` usa [`xlwings`](https://www.xlwings.org/) para incrustar el token autenticador en el archivo. Esta es la razón por la que el servidor de producción [corre sobre una máquina Windows con Excel instalado](despliegue.md#servidor).
- **Interacción en tiempo de ejecución:** una vez el usuario tiene el `.xlsm`, el backend deja de intervenir vía `xlwings` — son las [macros VBA las que llaman a la API directamente por HTTP](#cliente-de-plantilla-excel-vba).

Los módulos VBA están versionados en `src/vba/` y se sincronizan con `plantilla.xlsm` mediante el script `actualizar_vba.py`.

## Integraciones externas

- **Teradata:** fuente de los datos de siniestros, primas y expuestos, vía el driver [`teradatasql`](https://pypi.org/project/teradatasql/). Las credenciales se configuran desde la interfaz web (no desde variables de entorno), salvo para [ejecutar pruebas localmente](entorno.md#configurar-variables-de-entorno).

## Autenticación y roles

- Acceso mediante usuario y contraseña; la sesión se maneja con un **JWT en cookie** (`src/routers/auth.py`).
- Roles definidos en el modelo `Usuario`: **admin** (gestión de usuarios y accesos), **actuario** (usuario de negocio, crea y opera análisis) y **auditor** (solo lectura, ver la [guía para auditores](../auditores/inicio.md)).
- Cada usuario tiene además una lista de `negocios` a los que tiene acceso.

## Flujo de trabajo del usuario

```mermaid
flowchart TD
    A([Inicio]) --> B["1 · Login</br>src/routers/auth.py → JWT cookie"]
    B --> C["2 · Crear análisis</br>src/routers/maestro_analisis.py → Analisis en BD → segmentacion.xlsx"]
    C --> D{Origen de datos}

    D -->|Teradata| E["3a · Subir y correr queries SQL</br>src/routers/entradas.py → data/.../queries/"]
    D -->|Manual| F["3b · Subir Excel/csv/txt/parquet</br>src/routers/entradas.py → data/.../carga_manual"]

    E --> G["4 · Generar controles · Cuadre contable · Carga AFOs</br>src/routers/controles_informacion.py</br>→ cuadre_contable</br>→ validación SAP</br>→ reportes Excel"]
    F --> G

    G --> H["5 · Plantillas Excel</br>src/routers/metodos_plantilla.py → Crear / abrir plantilla → Preparar plantilla → Generar plantilla → Guardar aperturas → Cargar plantillas"]

    H --> I["6 · Consolidar resultados · Descargar informe Actuario Responsable"]

    I --> J([Descargar análisis])
```

## Estructura del repositorio

```mermaid
graph LR
    subgraph Inicio
        APP[app.py]
        CST[construir_static.py]
    end

    subgraph Routers
        RA[auth.py]
        RAD[admin.py]
        RCOM[comunes.py]
        RE[entradas.py]
        RC[controles_informacion.py]
        RM[metodos_plantilla.py]
        RMAN[maestro_analisis.py]
    end

    subgraph Frontend
        ST[static/]
        TEM[templates/]
    end

    subgraph informacion
        TC[tera_connect.py]
        CM[carga_manual.py]
    end

    subgraph controles_informacion
        GEN[generacion.py]
        SAP[sap.py]
        CC[cuadre_contable.py]
        BP[bases_plantilla.py]
    end

    subgraph metodos_plantilla
        GE[generar/]
        PR[preparar.py]
        GT[guardar_traer.py]
        BA[bases/]
        CD[completar_diagonal/]
        RES[resultados.py]
    end

    subgraph transversales
        AL[almacenamiento.py]
        UTL[utils.py]
        MDL[models.py]
        CNS[constantes.py]
    end

    subgraph validaciones
        VENT[entradas.py]
        VQUE[queries.py]
        VSEG[segmentacion.py]
    end

    APP --> RC & RM & RE & RA & RAD & RCOM & RMAN
    RE --> TC & CM
    TC & CM --> AL
    RC --> GEN
    GEN --> SAP & CC & BP & AL
    RM --> GE & PR & GT & RES
    GE --> BA
    PR --> BA
    GE --> CD
    TC --> VENT & VQUE
    CM --> VENT
    GT --> AL
    RES --> AL
    RMAN --> VSEG
    APP --> ST & TEM
    CST --> ST & TEM
```
