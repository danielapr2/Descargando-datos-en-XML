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
![image](https://user-images.githubusercontent.com/111605081/189788582-f6276098-3885-4148-a701-efed78d9b7bf.png)

```{r}
#Vemos que los datos que están en las etiquetas “revisión” son las que tienen datos que podrían interesarons, por ello, se descartan las etiquetas distintas usando dplyr::filter. Luego, necesito expandir los atributos de cada licencia en cada columna (es decir, una columna para TIPO, otra columna para DIST, etc.). A diferencia del paso anterior, esta vez se usa unnest_wider para "expandir" los datos en una sola columna a múltiples columnas (por lo tanto, más “anchas”).

lp_wider = xml_df %>%
  dplyr::filter(mediawiki_id == "revision") %>%
  unnest_wider(mediawiki) 
```
![image](https://user-images.githubusercontent.com/111605081/189789088-ea7f46be-3295-4f9c-8164-1a717dcf7ca7.png)


```{r}
# Los datos tienen que ser anidados dos veces. Los siguientes son los resultados después de pasar lp_wider a través del primer y segundo anidamiento. Después del primer anidamiento, cada columna sigue siendo un tipo de lista con una longitud de 1. Los datos se exponen solo después del segundo anidamiento: 

lp_df <- lp_wider %>%
  # 1er anidamiento
  unnest(cols = names(.)) %>%
  # 2do anidamiento
  unnest(cols = names(.)) %>%
  # Convertir tipo de datos
  readr::type_convert() 
```
![image](https://user-images.githubusercontent.com/111605081/189790616-281a3522-5b79-4213-94d4-2d8341eca782.png)



