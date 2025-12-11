# Estructura de la plantilla

## Hojas comunes

- **Resumen**: Totales de siniestros, primas, y expuestos por apertura y ocurrencia. Incluye resultados actuariales (siniestralidad última, frecuencia última, severidad última) más comentarios, metodologías y criterios de estimación. Contiene únicamente información de siniestros típicos.

- **Atípicos**: Igual que la hoja resumen, pero contiene únicamente información de siniestros atípicos.

- **Histórico**: Igual que la hoja resumen, pero con información de análisis anteriores. Presenta una columna que indica el mes del cierre correspondiente (*mes_corte*) y el tipo de análisis (si fue triángulos o entremés). Contiene información de siniestros típicos y atípicos.

!!! note "Nota"
    Estas hojas se generan automáticamente al preparar la plantilla. Las periodicidades de las aperturas corresponden a las especificadas en el [archivo de segmentación](../config/segmentacion.md#propiedades-de-cada-apertura).

## Hojas para análisis de triángulos

- **Indexaciones**: Permite definir vectores de indexación para la severidad, en caso de que se vaya a utilizar las metodologías de indexación por fecha de ocurrencia o por fecha pago. De lo contrario, se puede dejar vacía.

- **Frecuencia**: Permite hacer análisis de triángulos de frecuencia, sea por pago o por incurrido.

- **Severidad**: Permite hacer análisis de triángulos de severidad, sea por pago o por incurrido, y opcionalmente por una metodología de indexación por fecha de ocurrencia o fecha de pago.

- **Plata**: Permite hacer análisis de triángulos de plata, sea por pago o por incurrido.

## Hojas para análisis de entremés

- **Entremés**: Tiene la misma estructura de la hoja Resumen, pero cuenta con columnas adicionales para estimar las diferentes metodologías del entremés.

- **Completar diagonal**: Permite estimar la completitud de la diagonal y actualizar la siniestralidad última en función de cómo se viene desarrollando el entremés.
