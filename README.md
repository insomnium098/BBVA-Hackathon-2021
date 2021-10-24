
## Overview
Este pipeline esta implementado completamente en AWS, permite el procesamiento y normalización de los resultados financieros
de bancos en pdf a tablas csv y a un dashboard, permitiendo tener un overview de los
bancos competidores de forma fácil y automatizada.

Además permite tener una plantilla especial para cada banco, permitiendo su
modificación para procesar otros bancos no analizados actualmente.

![Diagrama](/imagenes/diagrama_general.jpeg)

## Procesamiento de pdf a tablas

El codigo se encuentra dividido en dos partes:
* PDF a tablas (1_textractor): El resultado se encuentra en la carpeta "Data"
* Procesar y extraer la informacion de las tablas (2_Procesar): El resultado se encuentra en la carpeta 3_tablas_data_base


## PDF a tablas  
Se subieron los pdfs a Amazon S3 y posteriormente se realiza lo siguiente:


* Se procesan todos los pdf dentro de Carpeta y se guardan localmente en una carpeta
con el mismo nombre que Carpeta.

python3 textractor.py --documents s3://testingbbvahackathon/Banorte/ --tables

Dentro de la carpeta local se crea un folder para cada pdf contenido en Carpeta.
En la carpeta de cada pdf se encuentra un directorio para cada una de las paginas.
Para cada uno de estos directorios hay 3 archivos, dos de los cuales son txt donde
se encuentran las palabras encontradas en el pdf.
En el archivo csv esta la tabla extraida de dicha pagina

## Procesar y extraer la informacion de las tablas
Para procesar los resultados de textractor se utilizará la funcion ab_team la
cual recibe un solo argumento params el cual es un diccionario con las siguientes
especificaciones:

* 'data_path' - ruta a los pdf's
* 'file_name' - ruta y nombre del csv de salida
* 'Entidad financiera' - diccionario que contiene los siguientes elementos:
    * 'keywords' - Lista que contiene las palabras clave en la tabla.
                     El codigo solo tomara en cuenta las tablas que contengan todas
                     las palabras en keywords
    * 'black_list' - Lista que contiene palabras prohibidas en la pagina.
                       A veces varias tablas coinciden con el criterio de keywords,
                       mediante las palabras en black_list se desechan las paginas
                       en las cuales se encuentre al menos una palabra en la
                       black_list
    * 'white_list' - Lista con los nombres de los archivos de la entidad financiera
                       donde se tiene permitido buscar las tablas

* 'dicts' - Lista con diccionarios especiales para cada entidad financiera.
            Para asegurar la coherencia de las tablas procesadas se necesita un
            diccionario que pueda homogenizar los nombres de las columnas entre
            las tablas de cada entidad financiera

La funcion ab_team devuelve un dataframe que contiene la informacion condensada
de todas las entidades financieras incluidas en params

Para cada banco se extrajeron 3 tablas:
 * Estado de Resultados
 * Balance General Activos
 * Balance General Pasivos

## Base de datos

Los csv resultantes fueron cargados en una BD E/R en el servicio de Amazon RDS

## Dashboard

El dashboard se realizó en Amazon QuickSight, por su funcionamiento se enviaron
invitaciones al dashboard a los jueces del evento. Para acceder al dashboard es necesario
aceptar la invitación e ingresar a QuickSight, ahi el dashboard aparecerá


![Diagrama](/imagenes/resultado_consolidado.jpeg)

![Diagrama](/imagenes/balance_general_activos.jpeg)

![Diagrama](/imagenes/balance_general_pasivos.jpeg)
