# Descargando-datos-en-XML
Con esta actividad aprenderemos como descargar datos en formato XML desde una URL; así mismo, practicaremos el análisis y selección de la información necesaria para desarrollar un proyecto de análisis de datos.  
Para llevar a cabo un manejo practico de la información, se creara un DataFrame con los datos necesarios y se guardara en un archivo tipo parquet. 

### Paso 1. Descarga de datos en formato XML y análisis del árbol que se genera:
```{r}
#Mandamos llamar la librería xml2, y leemos nuestro archivo XML desde la URL de GitHub proporcionada: 
library(xml2)
poetas <- read_xml('https://github.com/mcd-unison/ing-caract/raw/main/ejemplos/integracion/ejemplos/wikipedia-poetas.xml')
```
