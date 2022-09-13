# Descargando-datos-en-XML
Con esta actividad aprenderemos como descargar datos en formato XML desde una URL; así mismo, practicaremos el análisis y selección de la información necesaria para desarrollar un proyecto de análisis de datos.  
Para llevar a cabo un manejo practico de la información, se creara un DataFrame con los datos necesarios y se guardara en un archivo tipo parquet. 

### Paso 1. Descarga de datos en formato XML y análisis del árbol que se genera:
```{r}
#Mandamos llamar la librería xml2, y leemos nuestro archivo XML desde la URL de GitHub proporcionada: 
library(xml2)
poetas <- read_xml('https://github.com/mcd-unison/ing-caract/raw/main/ejemplos/integracion/ejemplos/wikipedia-poetas.xml')
```
Una vez descaragdos los datos, podemos observar el árbol en el que vienen almacenados. A primera instancia puedo ver que son muchas paginas y que cada una tiene diferentes valores que llegan hasta un cuarto nivel: 
![image](https://user-images.githubusercontent.com/111605081/189778930-12861941-3cca-4aaa-b38b-29c52a29af6a.png)

