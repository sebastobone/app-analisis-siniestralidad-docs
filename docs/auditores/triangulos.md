# Revisar triángulos

El negocio puede haber aplicado una de estas dos metodologías:

- **Frecuencia y Severidad**: la siniestralidad última se obtiene como el producto de ambas.
- **Plata**: la siniestralidad última se calcula directamente.

El procedimiento de revisión es el mismo en ambos casos:

- Si se utilizó **Frecuencia y Severidad**, las hojas relevantes serán **"Frecuencia"** y **"Severidad"**.
- Si se utilizó **Plata**, la hoja relevante será **"Plata"**.

Para entender la estructura de las hojas de análisis, consulte la [guía de uso de triángulos](../uso/plantilla/triangulos.md#estructura-de-la-hoja-de-analisis).

## Revisar una apertura

En la hoja de análisis:

1. Seleccione la **apertura** y el **atributo** que desea revisar.
2. Presione el botón **Cambiar apertura y traer estimación**.

!!! example "Ejemplo"
    El negocio **Movilidad** comunicó que utilizó la metodología de **Plata**. Como auditor, iniciaré la revisión en la apertura **01_040_MOTOS RESTO_MOTOS RESTO** para el atributo **bruto**. Debo seleccionar lo siguiente y luego presionar **Cambiar apertura y traer estimación**:
    ![Ejemplo auditor](assets/ejemplo_auditor.png)

Para cambiar de apertura, repita el proceso: seleccione la nueva apertura en los segmentadores y presione **Cambiar apertura y traer estimación**.

### Criterios de estimación

#### Factores excluidos

![Exclusiones](../uso/assets/plantilla/exclusiones.png)

#### Estadísticos de factores

- Ventanas de tiempo para estadísticos
- Vector de factores seleccionados

![Estadísticos](../uso/assets/plantilla/estadisticos.png)

#### Triángulo base

![Triángulo base](../uso/assets/plantilla/base.png)

#### Tabla resumen

- Metodología de pago o incurrido
- Ultimate por ocurrencia
- Metodología por ocurrencia
- Indicador por ocurrencia
- Comentarios por ocurrencia

![Triángulo base](../uso/assets/plantilla/tabla_resumen.png)

## Resultados consolidados

En la hoja **“Resumen”** encontrará los resultados consolidados de siniestralidad para todas las aperturas.
