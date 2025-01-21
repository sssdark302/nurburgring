# Programa de optimización de lista de compra

## Objetivo

El usuario debe poder pasar una lista de compra en un formato específico (por determinar) y el programa
debe devolver una ruta que debe seguir el usuario en base al tiempo, el precio total de la compra y la distancia a recorrer.

El programa debe interactuar con las paginas web de los supermercados con un scraper en python para obtener los precios de los productos 
y a su vez con una API de google maps para saber la ubicación del usuario y de los supermercados.

La implementacion de una IA está por determinar.

## Índice
 
* UMLS
  * Diagrama de clases
  * Casos de uso
  * Diagrama de secuencia
* Código
  * Estructura MVC.
    * Modelos 
    * Vista
    * Controladores
    * Servicios
    * Repositorios
  * Comunicación 
    * GraphQL como API
  * Scraper
    * Python en un programa aparte, se llama con un comando de la API
    * Debe poder hacer una web scraping despues de haber recibido la lista y ubicación
    * Debe devolver un JSON con los precios de los productos
  * API google maps/Ubicación
    * No debe ser pago
    * Debe poder recibir la ubicación del usuario y de los supermercados
* Docker
  * Almacena las bases de datos
  * Almacena el programa
  * Almacena el scraper
  * Almacena la API
  * Los elementos deben estar almacenados en una red de contenedores 
  * Debe poder ser desplegado en cualquier sistema operativo

## Código

### Singleton 

Para la entrada de datos (la lista) al programa, que posteriormente los dirigirá al scraper, 
es necesario que no se genere mas de una instancia de la clase que maneja la entrada de datos.

A su vez, es necesario que la respuesta que corresponde a cada usuario tampoco se genere mas de una vez.

### Basea de datos

Crear una tabla para cada usuario puede ser una opción si se quiere almacenar la información de cada usuario 
para que una IA sea entrenada para modelar el comportamiento de cada usuario. Tambien que la base de datos no se borre, 
lo que puede generar problemas en el servidor debido a la cantidad de datos.
Se necesitaria una cantidad de informacion del usuario mayor para modelar el comportamiento, como:
* Datos exactos del producto (gramos)
* Que el usuario concientemente quite de la lista los productos que tiene, no que simplemente los ignore

La opción más fácil es que la base de datos se borre cada vez que el usuario cierre la sesión. 
Darle una tabla a cada usuario de manera temporal para que se asegure la separacón de datos.

### Lectura 

La lista se pasa en formato JSON, se parsea y se envía al scraper. 

### Scraper

La lista pasada a JSON debe ser reconocible por el scraper en un formato específico para evitar confusiones.
Comas y puntos, punto y coma, son separaciones que debe reconocer, pero las -, _, /, etc, no.

El scraper debe poder devolver efectivamente el producto buscando las palabras clave que estén escritas en la lista.

**Problema Nº1:** Un producto que el usuario escriba estará posiblemente repetido en varios formatos. Ejemplo: "Queso de cabra"
En lonchas? En dados? En una rueda? Los gramos? El usuario debe poder especificar la cantidad y el formato del producto a ser posible.

Una alternativa sería que el scraper devuelva una lista de los productos encontrados independientemente del formato del mismo.

Otra es la de implementar una red neuronal para que sepa exactamente que formatos y diferencias existen en cada producto. 
Sería la más compleja pero la mas eficiente con diferencia. A su vez, si se pudiese guardar el historial de compras, 
la red neuronal no trabajaría de más porque podría existir un algoritmo que prediga el formato en base a compras anteriores.   


 