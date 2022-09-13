# Descargando-datos-en-XML
Con esta actividad aprenderemos como descargar datos en formato XML desde una URL; así mismo, practicaremos el análisis y selección de la información necesaria para desarrollar un proyecto de análisis de datos.  
Para llevar a cabo un manejo practico de la información, se creara un DataFrame con los datos necesarios y se guardara en un archivo tipo parquet. 

## Paso 1. Descarga de datos como xml y análisis del árbol que se genera:

```{r}
#Mandamos llamar las librerías xml2 y tidyverse, y leemos nuestro archivo XML desde la URL de GitHub proporcionada:

library(xml2)
library(tidyverse)
poetas_xml <- ('https://github.com/mcd-unison/ing-caract/raw/main/ejemplos/integracion/ejemplos/wikipedia-poetas.xml')
poetas_lista_xml <- as_list(read_xml(poetas_xml))

```
Una vez descaragdos los datos, podemos observar el árbol en el que vienen almacenados. A primera instancia puedo ver que son muchas paginas y que cada una tiene diferentes valores que llegan hasta un cuarto nivel: 
![image](https://user-images.githubusercontent.com/111605081/189778930-12861941-3cca-4aaa-b38b-29c52a29af6a.png)

## Paso 2. Definición de DataFrame basado en algún proyecto de Ciencia de Datos:
Al ver los diferentes nodos que conforman este archivo, pude percatarme que podría sacar información sobre autores que publican en Wikipedia en temas de poesía. De la misma manera, podríamos sacar la información sobre los poetas como tal, aunque esta información se encontrará dentro de uno de los nodos.

## Paso 3. Desarrollo del DataFrame:
```{r}
#El XML se convierte en una lista extremadamente larga. Como el XML está anidado en varias “capas”, se debe desanidar la primera “capa”. unnest_longer es una función en tidyr que divide los valores de la lista en varias filas.
#Se debe colocar la primera capa del XML, en este caso mediawiki, y  nos devolverá los datos dentro de cada etiqueta. Además, se agrega una nueva columna denominada mediawiki_id para indicar a qué etiqueta XML pertenecen los datos. 

xml_df = tibble::as_tibble(poetas_lista_xml) %>%
  unnest_longer(mediawiki)
```


