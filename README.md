

## Procesamiento de pdf a tablas
Para cualquier mencion de textractor haremos referencia al codigo ligeramente
modificado que se encuentra alojado en el repositorio


Una vez teniendo, en s3, una carpeta llamada Carpeta con la siguiente linea
se procesan todos los pdf dentro de Carpeta y se guardan localmente en una carpeta
con el mismo nombre que Carpeta.

python3 textractor.py --documents s3://testingbbvahackathon/Banorte/ --tables

Dentro de la carpeta local se crea un folder para cada pdf contenido en Carpeta.

En la carpeta de cada pdf se encuentra un directorio para cada una de las paginas.
Para cada uno de estos directorios hay 3 archivos, dos de los cuales son txt donde
se encuentran las palabras encontradas en el pdf.
En el archivo csv esta la tabla extraida de dicha pagina

## Procesamiento de tablas 
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
