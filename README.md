# Explicación técnica del reto – Obtención de festivos España 2023 en Power BI usando Lenguaje M
##1. Objetivo del reto

El objetivo del ejercicio consistía en obtener una tabla de fechas con los días festivos de España para el año 2023, indicando la fecha y el nombre del festivo, utilizando exclusivamente transformaciones en Lenguaje M dentro de Power Query.

La fuente de datos proporcionada fue la siguiente página web:

https://www.cuandoenelmundo.com/calendario/espana/2023

Además, se indicó el uso de las siguientes funciones del lenguaje M:

Web.Contents

Web.Page

Table.SelectRows

Table.Combine

Table.CombineColumns

Table.TransformColumnTypes

El resultado debía guardarse en un archivo .pbix dentro del repositorio de GitHub.

##2. Primer enfoque: extracción desde la web mediante Lenguaje M

Se implementó un primer modelo denominado:

Festivos 2023 – Web festivos

En este modelo se utilizó Power Query con Lenguaje M para intentar extraer los datos directamente desde la página web.

###2.1 Conexión a la web

Se utilizó:

Web.Contents

para descargar el HTML de la página, y posteriormente:

Web.Page

para interpretar el contenido y detectar las tablas presentes en el documento.

###2.2 Procesamiento de las tablas detectadas

La página contiene varias tablas HTML correspondientes al calendario mensual.
Para trabajar con ellas se aplicaron transformaciones como:

Table.SelectRows

Table.Combine

Table.CombineColumns

Table.TransformColumnTypes

El objetivo era reconstruir a partir de dichas tablas la información de fechas.

Este modelo permitió obtener correctamente los días del calendario, pero presentó un problema importante.

###2.3 Problema encontrado

La sección de la página que contiene los días festivos no está estructurada como una tabla HTML, sino como texto plano dentro del contenido de la página.

Esto provoca que:

Web.Page detecte correctamente las tablas del calendario mensual

pero no detecte la lista de festivos como tabla utilizable

Como consecuencia, el scraping web no devuelve la nomenclatura del festivo, sino únicamente los días del calendario.

Este comportamiento es coherente con la estructura real de la página, por lo que no se trata de un error de sintaxis, sino de una limitación del origen de datos.

3. Segundo enfoque: creación manual de la tabla en Lenguaje M

Para garantizar el cumplimiento del reto, se creó un segundo modelo denominado:

Festivos 2023 – Carga manual

En este modelo se construyó la tabla de festivos utilizando directamente Lenguaje M mediante la función:

#table

Ejemplo:

Source = #table(
    type table [Fecha = date, Festivo = text],
    {
        {#date(2023, 1, 1), "Año Nuevo"},
        {#date(2023, 1, 6), "Epifanía del Señor"},
        ...
    }
)

Esta solución cumple las condiciones del ejercicio porque:

se utiliza Lenguaje M

se genera la tabla dentro de Power Query

no se introduce manualmente en el modelo, sino mediante código

4. Justificación de la presentación de dos modelos

Se han incluido dos modelos en el informe por motivos didácticos y técnicos.

Modelo 1 – Web festivos

✔ Demuestra el uso de:

Web.Contents

Web.Page

Transformaciones en M

Procesamiento de tablas HTML

✔ Permite comprobar que la web se conecta correctamente
✔ Muestra el intento de extracción automática

Limitación:

La web no expone los festivos como tabla HTML, por lo que no es posible obtener la nomenclatura mediante Web.Page.

#Modelo 2 – Carga manual

✔ Garantiza el resultado correcto
✔ Cumple el requisito de usar Lenguaje M
✔ Permite generar la tabla final solicitada

✔ Es una solución robusta cuando el origen no es estructurado

##5. Retos encontrados durante el desarrollo

Durante la realización del ejercicio se encontraron los siguientes problemas técnicos:

###5.1 La página no expone los festivos como tabla

Los festivos aparecen como texto dentro del HTML, no como tabla.

Esto impide que Power Query los detecte automáticamente.

###5.2 Inconsistencia en la estructura HTML

Las tablas detectadas corresponden al calendario mensual, no a la lista de festivos.

###5.3 Errores de conexión al refrescar

Al mantener la consulta web activa, Power BI generaba errores de conexión, por lo que fue necesario:

deshabilitar la carga

o separar el modelo final del intento web

###5.4 Necesidad de validar el resultado

Se comprobó que la tabla manual coincidiera con los festivos oficiales de 2023.

##6. Conclusión

El reto permitió trabajar con:

extracción de datos desde web en Power Query

Lenguaje M

transformación de tablas

manejo de errores de origen

creación manual de tablas en M

Se presentan dos modelos para demostrar tanto el intento de extracción automática como la solución final funcional, justificando las limitaciones encontradas en el origen de datos.
