# Iniciaci&oacute;n a Git

**Git** es una base de datos de versionamiento, lo que permite guardar informacion sobre los cambios ocurridos dentro de un directorio repositorio, estos cambios los hace por archivo en caso de los binarios y por linea o fragmento de archivo en caso
de texto plano.

Los *comandos git* nos permitiran movernos atraves del historial de versiones, realizar cambios en el repositorio, clonarlo o agregar ramas alternativas a la principal, para ello hay que saber como funcionan estos comandos.

## Estados de directorios

### Estados  de Seguimiento

Al momento de generar un nuevo proyecto o repositorio, es necesario establecer cuales son los ficheros y directorios en los que se efectuara el control de versiones, para separar uno de otro se utilizan los terminos ***Tracked*** y ***Untracked***

* Tracked
  * Hace referencia a los ficheros que se van a ***Rastrear*** para poder llevar un control de sus cambios, estos entran a la Zona de ***Working Directory***
* Untracked
  * En cambio, ***Untracked***, deja de lado los ficheros y directorios, provocando que estos no tengan un historial de cambios y no se pueda regresar a alguna versi&oacute;n inicial o anterior estos estan en la Zona de ***Directory***

### Estados de ficheros

Dentro del proyecto, los ficheros pueden pasar por 4 estados b&aacute;sicos que ayudaran a tener un mejor control de los mismos, los cuales son:
* Unchanged - Sin Cambios
  * Son ficheros que *no han sido alterados ni modificados* de ninguna forma, es decir, estos ficheros no se tomarán en cuenta a la hora de almacenar cambios.
* Modified - Modificado
  * Son ficheros que sufireron alguna *modificación*, pero sus cambios aun no han sido guardados ni controlados
* Staged - Preparado
  * Son ficheros cuyos cambios han sido *preparados* para su almacenamiento, pero aun estan a la espera de la confirmacion de almacenamiento. Todos los ficheros que se encuentren en estado ***Staged*** se almacenan en el ***Área de Preparación*** o ***Staging Area***
* Commited - Confirmado
  * Son ficheros cuyos cambios han sido *preparados* y *confirmados* para su almacenamiento.

## Estados de Versionamiento

Los ssitemas de versionamiento como GIT suelen contar con varios estados o zonas en los que se ubican los cambios, cada uno tiene sus nombres y funciones pero aqu&iacute; solo veremos los de git. A estos estados tambien se los conoce como Flujo

### Flujo de Git

* Directory
  * Es el directorio raiz o carpeta base del proyecto antes de su inicializaci&oacute;n, tambien se puede decir que es la parte f&iacute;sica del proyecto.

* Working Directory
  * ***Working Directory*** es la zona en la que se almacenan todos los ficheros y sub-directorios del proyecto, aqu&iacute; dentro los ficheros y sub carpeta pueden adquirir dos propiedades o estados, ***Tracked*** y ***Untracked***

* Staggin Area
  * La ***Staging Area*** &oacute; ***&Aacute;rea de Preparaci&oacute;n*** es la encargada de almacenar los ficheros que van siendo preparados para el almacenamiento y se encuentran a la espera de otros ficheros o de una confirmacion.
* Git Repository
  * En el ***Repositorio de Git*** ó ***Git Repository*** es donde se encuentran nuestros cambios almacenados y versionados.

## Ramas de Git

Para llevar un mejor control deversiones e incluso, sergmentar el trabajo de una sección sin interferir con alguna otra 

## Comandos B&aacute;sicos

### init

Este comando se utiliza para iniciar el proyecto en el cual vamos a aplicar el control de versiones y todas las demas ventajas de una base de datos de versionamiento

### add *[uri]*

Este comando admite una direccion referente a un fichero o directorio *[uri]*, y nos permite agregarla al control de versiones, es decir, registrarla en la base de datos de versionamiento para poder llevar un control de este directorio o fichero *(siempre y cuando no este incluido en el .gitignore)*.
En caso de necesitar agregar todo lo que se encuentre  se utiliza ***"."***.

***Ejemplo git add . -> agregamos todo el proyecto al control.***

### status

Este comando nos permite saber el estado actual de nuestro proyecto con respecto a cambios y estado de la version, y si estos han sido almacenados o no.

### rm --[flags] *[uri]*

***"rm"*** nos permite borrar o remover un directorio o fichero y puede ir acompa&ntilde;ado de "flags" para cambiar su comportamiento. Este comando no se puede usar sin antes indicarle como eliminar los ficheros que ingresamos.

* --cached
  * Elimina los archivos del ***Git Repository*** y del ***Staging Area***, pero mantiene los mismos dentro de nuestro ***Working Directory***, es decir, con este comando podemos pasar los ficheros de estado ***tracked*** a ***untraked***
* --force
  * Elimina los archivos tanto de ***Git*** como del disco duro. Al ***Git*** *guardar todos los estados de los ficheros* y su *historial*, es posible volver a *recuperar este fichero*, pero para ello hay que hacer uso de comandos *específicos*.

### commit [flags]

El comando Commit confirma los cambios realizados en los ficheros siempre y cuando estos hayan pasado al estado de preparados, sino se dejaran fuera. El comando de confirmación no se suele usar sin adjuntar un mensaje (a menos que sea un ammend) mediante la flag -m y una cadena **String**. Flags Disponibles:

* -a
  * Nos permite agregar automáticamente todos los cambios realizados al Staging Area sin usar el comando git add
* -m
  * Esta flag nos permite adjuntar el mensaje que identificara los cambios realizados en git al incluir una cadena de **String** con el mismo.
* --ammend
  * Esta flag nos permite incluir los cambios actuales al commit anterior, se usa en caso de que no se hayan realizado todos los cambios o que un fichero no se puso en stag.


<hr>


## history

Para ver el historial de comandos ejecutados escribimos history en la terminal git/linux
y para volver a ejectuar esas lineas usamos *!+num.*

***Ejemplo: !23 nos volveria a ejectuar la linea #23 de nuestro historial.***


# Ejemplo: Mi blog personal

<p>aqu&iacute; he realizado un peque&ntilde;o proyecto de pruebas que me permite practicar mis habilidades con la herramienta de <b>GitHub</b>. </p>
<h2>Lista de Actualizaciones</h2>

      v:0.0.0     - Creando archivo de readme para practicar el manejo de los comandos de git.
      v:0.0.1     - Agregamos el index para trabajar y empezamos con el etiquetado de versiones.
      v:0.0.2     --> Index.html      -> cambios en el titulo del documento
                                      -> agregando primer titulo Quien soy?
                                      -> agregando segundo titulo:    conociendome
                                      -> agregando parrafo para conociendome  y redactando. 
      v:0.0.3     --> Index.html      -> Eliminando etiquetas de lista incesarias
                                      -> agregando archivos de prueba:
                  --> Prueba.txt      -> agregando lineas para practicar resets
                  --> No_tocar.txt    -> agregando lineas para probar la  manera de retener ese cambio sin guardar en los comits
            // v:0.0.4 --> Prueba.txt      -> Cambiando Lineas.
      v:0.0.5     --> No_tocar.txt    -> estado cambiado a untracked
                  --> Index.html      -> corregido cambios.
      v:0.0.6 --> Prueba.txt      -> ediciones.
      v:0.0.7 --> Prueba.txt      -> ediciones para el soft.
      v:0.0.7f--> Creacion de nueva Branch.
      v:0.0.8f--> Agregar Css ejemplo.css.
    branch feature borrada. (f)
    branch cabecera creada. (c)
      v:0.0.9c    --> index.html      -> agregado div cabecera.
      v:0.0.10c   --> index.html      -> estructura de la cabecera.
                  --> css/ejemplo.css -> estilo de cabecera.
      v:0.0.11    --> index.html      -> modificando contenido.
                  --> css/ejemplo.css -> modificando fuente a arial.
    branch staging-develop (d)
      v:0.0.12d   --> Editada la imagen para el fondo
                  --> index.html      -> cambios al fondo de container e imagen de fondo body
      v:0.0.13    --> Main Merge Staging-Develop
    branch footer creada. (f)
      v:0.0.14f   --> index.html      -> agregando footer
                  --> css/ejemplo.css -> estilizando footer

    Experimental functions loadeds!
    Branch experimento (e) creada.
      v:0.0.15e   --> readme.md       -> experimental
