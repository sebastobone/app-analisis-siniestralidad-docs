<!--markdownlint-disable MD046-->
<!--markdownlint-disable MD024-->
<!--markdownlint-disable MD007-->

# Versionamiento

## 1.4.0 - 2026-04-19

### ✨ Mejoras

- Los archivos de consistencia histórica ahora respetan cambios en las aperturas. Desde la interfaz web se podrá elegir el nivel de granularidad con el que se generarán estas evidencias.
    - **Nota:** Si existen análisis con una granularidad más agregada que la seleccionada, estos no se incluirán en la evidencia.

- **Plantillas de triángulos**
    - Se añadió una opción para evitar la exclusión automática de factores basada en desviaciones estándar.
    - Se incorpora la metodología **"sin_ibnr"** para forzar el _ultimate_ a ser igual al incurrido.

### 📦 Interno

- Se agrega en la tabla **Analisis** campos para almacenar las aperturas de siniestros, primas, y expuestos.

## 1.3.1 - 2026-04-16

### ✨ Mejoras

- El tamaño máximo de descarga de archivos pasa a ser de 20 MB a 50 MB.

### 🐞 Correcciones

- Corregir botón de cargar AFOs.

### 📦 Interno

- Mover parámetro de tamaño máximo de descarga a variables de entorno.

## 1.3.0 - 2026-04-15

### ✨ Nuevas funcionalidades

- Agregar tabla para visualizar y descargar el archivo de segmentación cargado #68

### 📦 Interno

- Crear diagramas de documentación de la aplicación

## 1.2.4 - 2026-03-29

### 🐞 Correcciones

- Corregir botón **Preparar plantilla** para que actualice la información de hojas ya generadas #64

### 📦 Interno

- Crear tests para probar los queries de cierre contable oficiales almacenados en `referencias/`
- Cambiar type checker por `zuban`
- Eliminar dependencia de `sse-starlette`, pues ahora `fastapi` implementa SSE nativamente
- Actualizar dependencias y vulnerabilidades

## 1.2.3 - 2026-02-04

### 🐞 Correcciones

- Corregir aperturas y modelo de cierre de autonomía
- Mostrar errores tipo HTTP 4XX en frontend para mejorar retroalimentación

## 1.2.2 - 2026-02-02

### 🛠️ Mejoras

- Agregar link para ver ejemplos de queries reales.
- Evitar recolectar LazyFrame para pivotear columnas.

### 🐞 Correcciones

- Corregir link de AFOs de referencia.

## 1.2.1 - 2026-01-14

### 🐞 Correcciones

- Se impide que los botones que hacen peticiones al servidor puedan presionarse más de una vez antes de recibir la respuesta, con el fin de prevenir peticiones duplicadas.

## 1.2.0 - 2026-01-14

### 💥 Cambios disruptivos

- **Límite de descarga de archivos**
Se bloquea la descarga de archivos mayores a **20 MB** desde la interfaz web, con el fin de evitar sobrepasar los límites de transferencia de datos del servidor.
Los archivos que superen este tamaño deberán seguir solicitándose directamente al administrador de la aplicación.

### 📦 Cambios internos

- **Type checking en el frontend**
Se implementa verificación de tipos en el código fuente del frontend para mejorar la robustez, mantenibilidad y detección temprana de errores.

## 1.1.0 - 2026-01-12

### ✨ Nuevas funcionalidades

- **Secciones colapsables**
Ahora las secciones de la interfaz web pueden colapsarse cuando no están en uso, facilitando la navegación y reduciendo el ruido visual.
- **Pop-up para crear análisis**
La creación de nuevos análisis se realiza ahora mediante una ventana emergente, accesible desde un botón ubicado debajo de la tabla de análisis.
- **Mejora en la presentación de tablas**
Se optimiza el formato y la disposición de las tablas para una lectura y navegación más clara.
- **Notificaciones de errores mejoradas**
Las notificaciones de error ahora son más detalladas y se generan de forma consistente para todos los botones de la interfaz web.

### 🐞 Correcciones

- Se notifica al usuario cuando la sesión ha expirado, solicitando un nuevo inicio de sesión.
- El informe del **Actuario Responsable** se genera únicamente cuando el análisis corresponde a un cierre contable y sólo con los archivos del análisis seleccionado.
- Se eliminan correctamente los checkboxes de candidatos a controles cuando los archivos asociados son borrados desde la interfaz web.
- Se elimina el botón para descargar todas las plantillas simultáneamente.
- El botón **“Descargar ejemplo de segmentación”** se reemplaza por un enlace a una sección con múltiples ejemplos descargables.
- Se impide la creación de plantillas si no se ha cargado al menos un archivo para cada entrada requerida (siniestros, primas y expuestos).
- Los botones de la interfaz web se desactivan mientras una petición está en curso, evitando el doble procesamiento.

### 📦 Cambios internos

- Centralización del manejo de errores (HTTP y excepciones generales) en un único módulo.
- Simplificación de los estilos asociados a los checkboxes.

## 1.0.2 - 2026-01-07

### 🐞 Correcciones

- Arreglar botón para descargar ejemplo de archivo segmentación.

## 1.0.1 - 2026-01-05

### 🐞 Correcciones

- Corregir periodos de ocurrencia del formato de Actuario Responsable para periodicidades con meses corridos

## 1.0.0 — Lanzamiento web - 2026-01-04

Esta versión marca un **hito mayor** en la evolución de la aplicación: el paso de una aplicación instalada localmente a una **plataforma web centralizada**, con control de usuarios, almacenamiento en servidor y nuevos flujos operativos.

### 💥 Cambios disruptivos

#### Funcionamiento general

- La aplicación **ya no se instala localmente**. Ahora se accede desde la web: :point_right: [https://predeterminate-bandoliered-gino.ngrok-free.dev/](https://predeterminate-bandoliered-gino.ngrok-free.dev/)
- El acceso requiere **usuario y contraseña**.
- Todos los análisis creados se almacenan en el servidor, incluyendo:
    - Archivos cargados
    - Parametrizaciones
    - Resultados
- Los **archivos de segmentación, AFOs y queries** ahora se cargan exclusivamente desde la **interfaz web**.

#### Plantillas de Excel

- Funcionalidades que antes estaban en la interfaz web ahora se encuentran **dentro de las plantillas**:
    - Botones para ejecución de macros.
    - Listas desplegables de aperturas, atributos y referencias de entremés.
- La hoja **Entremés ya no se genera automáticamente** al ejecutar la macro **Preparar plantilla**.
    - Ahora incluye un botón específico **Generar** dentro de la hoja.

#### Documentación

- Nuevo enlace oficial de la documentación:
    - :point_right: [https://sebastobone.github.io/app-analisis-siniestralidad-docs/latest/](https://sebastobone.github.io/app-analisis-siniestralidad-docs/latest/)

#### Proceso de auditoría

- Los auditores **ya no necesitan descargar la aplicación**.
- Pueden acceder vía web con un rol de **solo lectura**, lo que les permite:
    - Visualizar análisis y archivos
    - Interactuar con la aplicación
    - **Sin posibilidad de modificar** información almacenada en el servidor

#### Repartición de diferencias contables por ocurrencia

- Las diferencias contables ya no se asignan obligatoriamente a la ocurrencia más reciente.
- Se pueden definir porcentajes de distribución entre ocurrencias.
- Se debe agregar la columna **fecha_siniestro** a la hoja **Meses_cuadre_siniestros** del archivo de segmentación.

### ✨ Nuevas funcionalidades

- **Control SOX – Inconsistencia de fechas**
Se genera una nueva evidencia SOX que identifica registros donde la **fecha de siniestro es posterior a la fecha de registro**.
- **Script de migración**
Se implementa un script para migrar y almacenar análisis realizados con versiones anteriores de la aplicación.
- **Panel de administración**
    - Creación, edición y eliminación de usuarios.
    - Gestión centralizada de accesos.
- **Exclusiones por desviaciones estándar**
    - En las plantillas de Excel se incorpora la opción de excluir factores de desarrollo que superen un número configurable de desviaciones estándar.
- **Periodicidades con meses corridos**
    - Ahora es posible definir periodicidades no alineadas al calendario tradicional.
    - Ejemplo: **Semestral (Abr–Sep | Oct–Mar)** permite construir triángulos y entremeses con esos cortes.
- **Archivos base para Empresariales**
    - Los archivos de segmentación y queries de este negocio se incluyen **por defecto en el repositorio**.
    - Disponibles al descargar ejemplos desde la aplicación.

### 🐞 Correcciones

- Se impide la existencia de **aperturas sobrantes** en el archivo de segmentación.
    - Las salidas de los queries deben coincidir **exactamente** con dichas aperturas.
- Se bloquea el ingreso de **fechas nulas** (siniestro o registro) en la carga manual de archivos.
- Los valores nulos en cargas manuales ahora se **completan automáticamente con ceros**.
- Se corrige la lógica que determina si un query corresponde a **subida de tablas**.
- Se implementa **cache busting** para asegurar que la interfaz web siempre cargue la versión más reciente.
- Los **errores del servidor** ahora se muestran explícitamente en la interfaz web.

### 📦 Funcionamiento interno

- Almacenamiento de **metadata asociada a pantallazos SOX**.
- Reemplazo de:
    - `pre-commit` → `prek`
    - `mypy` → `ty`
- La documentación se mueve a un repositorio independiente:
    - :point_right: [https://github.com/sebastobone/app-analisis-siniestralidad-docs](https://github.com/sebastobone/app-analisis-siniestralidad-docs)
- Optimización de los **tests de integración** no relacionados con las plantillas.
- Vectorización del cálculo de **factores de completitud**.

### 📚 Documentación

- Se agrega un **filtro** para alternar entre la documentación de la versión web y la versión anterior.
- Se documenta el **nuevo flujo de trabajo** entre:
    - Interfaz web
    - Plantillas de Excel

## 0.15.3 - 2025-09-19

### 🐞 Correcciones

- Se corrige generación de cajitas de archivos candidatos para controles de expuestos.
- Se corrige generación de evidencia de archivo de segmentación.

## 0.15.2 - 2025-09-16

### 🐞 Correcciones

- Se corrigen links rotos y formatos en el README del repositorio.

## 0.15.1 - 2025-09-16

### ✨ Nuevas funcionalidades

- **Hoja Indexaciones**
Se agrega una tabla por defecto con indicadores comunes.

- **Descargar ejemplos con aperturas propias**
Ahora los ejemplos de archivos de siniestros, primas, y expuestos se generan con las aperturas del análisis, no con aperturas genéricas.

- **Validación de aperturas no nulas**
Ahora se valida también que en el archivo de segmentación no hayan aperturas con valores nulos.

### 🐞 Correcciones

- Se corrige el envío de las referencias del entremés al **preparar plantilla**.
- Al **preparar plantilla**, se actualizan los datos de **plantillas ya generadas**.

### 📚 Documentación

- Se robustece la documentación de las nuevas funcionalidades y de la guía para auditores.

## 0.15.0 - 2025-09-12

### ✨ Nuevas funcionalidades

- **Carga de queries desde interfaz web**
Ahora es posible cargar los queries directamente desde la interfaz web. La opción de carga manual desde la carpeta se mantiene disponible.

### 📚 Documentación

- Se robustece la guía para auditores con imágenes y ejemplos.

## 0.14.0 - 2025-09-10

### ✨ Nuevas funcionalidades

- **Carga manual de AFOs desde la interfaz web**
Ahora es posible cargar los archivos **AFO** directamente desde la interfaz web. La opción de carga manual desde la carpeta se mantiene disponible.

### 📦 Mantenimiento

- Eliminación de dependencias innecesarias para optimizar la instalación de la aplicación.

## 0.13.2 - 2025-09-09

### 🐞 Correcciones

- Se corrigen links rotos en la documentación

## 0.13.1 - 2025-09-09

### 🐞 Correcciones

- Se ajusta la consolidación de **archivos de siniestros, primas y expuestos** para que tenga en cuenta el resultado del proceso de **cuadre contable**.

### 📚 Documentación

- Se incorpora una nueva **guía para auditores**.

## 0.13.0 - 2025-09-08

### 💥 Cambios disruptivos

- **Estructura del archivo de segmentación**
    - Se elimina la columna `apertura_reservas`, la cual ahora se construye de manera interna.
    - Se incorporan nuevas columnas para definir la **metodología de indexación de severidad** en cada apertura.
    - Para **Movilidad** y **SOAT**, las tablas de **primas de asistencia** y **fraude** ya no se cargarán en este archivo, sino a través del nuevo mecanismo de carga manual.
- **Credenciales de Teradata**
Las credenciales ya no se almacenan en el archivo `.env.private`, sino que se ingresan directamente desde la **interfaz web**.

### ✨ Nuevas funcionalidades

- **Metodologías de indexación de severidad**
Se agregan nuevas formas de estimar la severidad mediante indexaciones basadas en **indicadores por fecha de ocurrencia** o **fecha de movimiento**.
- **Carga manual de archivos**
Ahora es posible cargar manualmente los archivos de **segmentación, siniestros, primas y expuestos** desde la interfaz web. Además, se incluye la opción de **descargar ejemplos** de estos archivos como referencia para su construcción.
- **Controles y cuadre contable más versátil**
La generación de controles y el cuadre contable ya no se limita a los archivos extraídos de **Teradata**. Ahora se pueden incluir **uno o varios archivos cargados manualmente** dentro de la comparación.
Se agregan también **checkboxes** para decidir si aplicar o no el proceso de cuadre contable en **primas** y **siniestros**.

### 🐞 Correcciones

- Se mantiene el atributo actual al cambiar de plantilla (ejemplo: si está en _Severidad_ – _Retenido_ y cambia a _Plata_, se conserva la lista desplegable en _Retenido_).
- En los comandos **"Guardar todo"** y **"Traer y guardar todo"**, se corrige la actualización de progreso para que se muestre **en tiempo real**.

### 🛠️ Otras mejoras

- Simplificación del ingreso de parámetros en la interfaz web.
- Generación dinámica de la interfaz web en función de los parámetros ingresados.
- Mejoras en la retroalimentación de las notificaciones dentro de la interfaz web.
- Se agrega un nuevo negocio **“Custom”**, diseñado para análisis personalizados fuera de la lista convencional.

### 📚 Documentación

- Se explica cómo recibir notificaciones por correo sobre nuevas versiones de la aplicación.
- Se agrega la guía para resolver problemas ocasionados por **plantillas de Excel almacenadas en OneDrive o SharePoint**.

## 0.12.3 - 2025-09-04

### 🐞 Correcciones

- **Plantilla de Entremés**: Se corrige el error que hacía que las fórmulas se pegaran como texto al volver a preparar una plantilla previamente preparada.

## 0.12.2 - 2025-08-26

### 🐞  Correcciones

- **Gráfica de factores**: Se ajusta la generación de la gráfica para garantizar su correcto funcionamiento en aperturas con pocos datos.
- **Activación del libro**: Antes de ejecutar cualquier función de la plantilla, el libro de Excel se traerá automáticamente al primer plano. Esto permite trabajar con varios libros abiertos sin interferir con la plantilla.
- **Filtro de fechas**: El filtro de fechas ahora se aplica también durante el procesamiento de información ingresada manualmente (no extraída de Teradata), asegurando uniformidad en todos los datos.

### 📚 Documentación

- Se incorpora una nueva sección de **Preguntas frecuentes (FAQ)**.

## 0.12.1 - 2025-08-25

### ✨ Nuevas funcionalidades

- **Siniestros atípicos de movilidad**: Se actualiza el valor para clasificar un siniestro como atípico en movilidad.

## 0.12.0 - 2025-08-25

### ✨ Nuevas funcionalidades

- **Prepararación automática antes de traer y guardar todo**: Ahora, al ejecutar las funciones **"Traer y guardar todo"** o **"Traer fórmulas entremés"**, se ejecuta automáticamente la función **"Preparar plantilla"**.
- **Preparar plantilla no invasivo**: La función **"Preparar plantilla"** ya no elimina el contenido de las plantillas de triángulos o entremés, evitando la pérdida de modificaciones realizadas para el análisis.
- **Evidencia SOX de consistencia**: Cada vez que se ejecuta **"Preparar plantilla"**, se genera automáticamente una evidencia SOX que valida que la información enviada a la plantilla coincide con la obtenida en el proceso de extracción y cuadre contable.

### 🐞 Correcciones

- **Agrupación de ocurrencias mensuales en análisis anteriores**: En los análisis de triángulos con granularidad distinta a la mensual, ahora las ocurrencias mensuales de análisis previos se agrupan correctamente, permitiendo comparaciones más precisas contra los resultados del triángulo.
- **Segmentación de dependencias**: Durante la instalación de la aplicación, únicamente se descargarán las dependencias necesarias para su funcionamiento, excluyendo las de desarrollo.

### 📚 Documentación

- Revisión y ajuste de la descripción de las funciones **"Preparar plantilla"**, **"Traer y guardar todo"** y **"Traer fórmulas entremés"**, incorporando las nuevas funcionalidades.
- Actualización de los comandos de instalación según la nueva segmentación de dependencias.

## 0.11.0 - 2025-08-03

### ✨ Nuevas funcionalidades

- **Gráfica de factores de desarrollo**: Ahora puedes visualizar los factores de desarrollo de los triángulos a través de una nueva gráfica interactiva. Esta herramienta facilita la interpretación de tendencias y permite un análisis más intuitivo de los datos.
- **'Guardar y Traer Todo' por atributo**: La lógica se ha modificado para considerar únicamente el atributo actual (bruto o retenido) al momento de guardar o cargar información, permitiendo el uso de esta funcionalidad para negocios que no analizan ambos atributos.

### 🐞 Correcciones

- **Query de siniestros de movilidad**: Se actualizó la lógica de casos atípicos en movilidad para mantener la consistencia entre el entremés y los triángulos.

### 📦 Mantenimiento

- **Actualización de dependencias**: Se actualizaron librerías y paquetes del proyecto para garantizar mayor estabilidad y seguridad.

### 📚 Documentación

Se realizó una mejora sustancial a la documentación del proyecto:

- Se añadieron secciones sobre:
    - Análisis de triángulos y entremés
    - Procesamiento y extracción de información
    - Pruebas, versionamiento y resultados
    - Estructura del frontend con imágenes organizadas
- Se reorganizó la documentación para una navegación más clara y amigable.
- Se documentó detalladamente la **nueva gráfica de factores**.
