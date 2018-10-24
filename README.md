# ASR-2018
- [2: Introducción a Shell Scripting](#IntroScripting)
  - [Ejercicios](#Ej2)
    - [Pagina 9](#EjPag9)
    - [Pagina 15](#EjPag15)
    - [Pagina 19](#EjPag19)
  - [Conceptos Basicos](#ConceptosBasicos)
  - [Redireccionamiento de Entrada/Salida](#RedireccionamientoES)
  - [Trabajando con el Bash](#Bash)
  - [Estructura de un Script](#EstructuraScript)
- [3: Gestion de recursos](#GestionRecursos)
  - [Sistema de ficheros](#SisFich)
  - [Entorno de trabajo](#EntTrabajo)
  - [Cuentas, Grupos, Contabilidad](#CCC)
  - [Procesos y Memoria](#ProcesosYMem)
- [4: Arranque y parada. Procesos periódicos. Gestión del software](#ArranqueParada)
- [Anexo: Tabla de comandos](#TablaComandos)


<a name="IntroScripting"/>

## 2. Introducción a Shell Scripting

<a name="Ej2"/>

### Ejercicios Scripting

<a name="EjPag9"/>

(pag 9)
#### 1. Averigua cuál es el intérprete de comandos por defecto en tu sistema.
``` bash
echo $0
```


#### 2. Lanza sh desde el terminal, comprueba que realmente se está ejecutando y regresa al IC original.
```bash
sh
echo $0 
exit
```

#### 3. Guarda los nombres de las entradas (contenido) del directorio /etc en el fichero de nombre file.tmp.

``` bash
ls -l /etc > file.tmp
```
#### 4. Añade al Fichero del punto anterior los nombres de las entradas (contenido) del directorio /bin.
```bash
ls /bin >> file.tmp
```

#### 5.En una sola instrucción, guarda los nombres de las entradas (contenido) de los directorios /etc y /usr en el fichero file.tmp.
```bash
ls /etc > file.tmp; ls /usr >> file.tmp
```
---

<a name="EjPag15"/>

(pag 15)
#### 1. Programar un script que muestre el nombre del script.
```bash
#!/bin/bash
# Programar un script que muestre el nombre del script.
echo $0
exit 0
```
#### 2. ¿Dónde debería ser copiado el script para que se pueda ejecutar como cualquier comando del sistema (sin hacer referencia al directorio en el que está), como por ejemplo, $ ls?
```
Deberia estar copiado en el directorio indicado en la variable de entorno PATH: /home/user/bin/
```
#### 3. Programar un script que obtenga un texto (cualquiera) por la salida estándar y otro por la salida estándar de errores.
```bash

```
#### 4. Modifcar el script del punto anterior para que desde el interior del propio script redirija la salida estándar al Fichero “nombre-del-script-output.txt” y la de errores a “nombre-del-script-errors.txt”.
```bash

```
---

<a name="EjPag19"/>

(pag 19)
#### 1. Programa un script que calcula y muestra la suma de tres números que recibe como parámetro.
```bash
#!/bin/sh
#  Programa un script que calcula y muestra la suma de tres números que recibe como parámetro.
expr $1 + $2 + $3
exit 0
```
#### 2. Programa un script que calcula y muestra la suma de los números que recibe como parámetro. El número de parámetros no se conoce a priori.
```bash
#!/bin/sh
#  Programa un script que calcula y muestra la suma de tres números que recibe como parámetro.
suma=0
for i in $* 
 do 
  suma=`expr $suma + $i`
 done  

echo $suma
exit 0
```
#### 3. Como el anterior, pero en lugar de recibir los números como parámetros, los recibe a través del teclado.
```bash
#!/bin/sh
#Como el anterior, pero en lugar de recibir los números como parámetros, los recibe a través del teclado.

read VAR
suma=0
for i in $VAR 
 do 
  suma=$((i+suma))
 done  
echo $suma
exit 0
```
#### 4. Programa un script que muestre sólo los nombres de los Ficheros del directorio principal, es decir, sin la parte correspondiente al directorio en el que están ni la extensión. Ejemplo: /home/user/File1.txt ⇒ File1
```bash
#!/bin/sh
#Programa un script que muestre sólo los nombres de los Ficheros del directorio principal, es decir, sin la parte correspondiente al directorio en el que están ni la extensión. Ejemplo: /home/user/File1.txt ⇒ File1
result=`find /home/*`
for file in $result 
 do 
   trim=${file##*/}
   echo ${trim##*.}
 done
exit 0
```
#### (pag 23) 1. Programa un script que compruebe que recibe dos parámetros.
```bash
#!/bin/sh
#Programa un script que compruebe que recibe dos parámetros.
if [ $# -eq 2 ] ; then
  echo "hay dos parametros"
else
  echo "ERROR: no hay 2 parametros"
fi
exit 0
```
#### (pag 23) 2. script que recibe como parámetro dos números e indica cuál es el de mayor valor.
```bash
#!/bin/sh
#script que recibe como parámetro dos números e indica cuál es el de mayor valor
if [ $# -eq 2 ]
  then if [ $1 -gt $2 ] 
    then echo $1 
    else echo $2
  fi
  else
    echo "ERROR: no hay 2 parametros"
    exit 1
fi
exit 0
```

#### (pag 23) 3. script que comprueba que el Fichero que recibe como parámetro existe
```bash
#!/bin/sh
#script que comprueba que el Fichero que recibe como parámetro existe
if [ $# -eq 1 ]
  then if [ -f $1 ]
    then echo "Existe el fichero"
    else echo "No Existe"
  fi
  else echo "Parametros incorrectos"
fi
exit 0
```

#### (pag 23) 4. script que comprueba que el directorio que recibe como parámetro existe y después cuenta cuántos subdirectorios y cuántos ficheros contiene
```bash
#!/bin/sh
#script que comprueba que el directorio que recibe como parámetro existe y después cuenta cuántos subdirectorios y cuántos ficheros contiene
if [ $# -eq 1 ]
  then if [ -d $1 ]
    then 
      contDir=0      
			contFich=0
      for fich in $1/*
      do
	if [ -d "$fich" ]
	then contDir=$(($contDir + 1))
	fi 
	if [ -f "$fich" ]
	then contFich=$(($contFich + 1))
	fi        
      done
    echo "El directorio tiene $contDir subdirectorios"    
		echo "El directorio tiene $contFich ficheros"
    else echo "No Existe"
  fi
  else echo "Parametros incorrectos"
fi
exit 0
```
#### (pag 23) 5. Como en el caso anterior, pero contando el número de Ficheros con extensión txt
```bash
#!/bin/bash
#Como en el caso anterior, pero contando el número de Ficheros con extensión txt
if [ $# -eq 1 ]
  then if [ -d $1 ]
    then 
			contFich=0
      for fich in $1/*
      do
	if [ -f "$fich" ]
	then if [ ${fich##*.} = "txt" ]
	  	then contFich=$(($contFich + 1))
	     fi
	fi        
      done
      echo "El directorio tiene $contFich ficheros .txt"
    else echo "No Existe"
  fi
  else echo "Parametros incorrectos"
fi
exit 0
```

#### (pag 23) 7. script que muestra cuántas extensiones diferentes hay en un directorio, junto con el número de ocurrencias de cada una. Ejemplo: 
#### ’txt’ ⇒ 4
#### ’pdf’ ⇒ 2
. . .
```bash
#!/bin/bash

if [ $# -eq 1 ]
	then if [ -d $1 ]
    then 
      for fich in $1/*.*
      do
	if [ -f $fich ]
	  then echo ${fich##*.} >> extensiones.aux
	fi        
      done
      extensiones=$(sort < extensiones.aux | uniq)
      for ext in $extensiones
      do
      	cont=0
	for fich in $1/*.$ext
	do
	  cont=$(($cont+1))
	done
	echo "$ext => $cont"
      done

      rm extensiones.aux

    else echo "No Existe"
  fi
  else echo "Parametros incorrectos"
fi
exit 0
```

<a name="ConceptosBasicos"/>

### Conceptos basicos
$0 es el nombre el del programa/script que se esta ejecutando

El caracter ```~``` el interprete de comandos lo sustituye por el direcotorio del usuario. Es un metacaracter

El caracter ```*``` el interprete de comandos busca elementos con el patron de caracteres especificado despues de del metacaracter(```*```)

En el directorio ```/dev``` se almacenan los dispositivos del ordenador (pantalla, teclado, ...). El directorio ```/dev/null``` es un direcotio especial, en el que todo lo que se escribe, se pierde (se borra automaticamente). El directorio ```/dev/zero```, contiene cadenas de ```0``` (bytes, que pueden representar cadenas de caracteres). El directorio ```/dev/random```, genera bytes aleatorios (aunque se puede manipular para obtner unas cadenas con expresiones concretas)

Un terminal es una interfaz, un dispositivo de entrada y salida con el que se comunica con un shell (bash, sh, dash, ...). Puede ser grafica o con comandos. Existen terminales virtuales como las conexiones ssh

Existen variables en el interprete de comandos que se definen con ```$```. Existen dos tipos: vatiables de entorno (mayuscula) y variables locales (minuscula).

<a name="RedireccionamientoES"/>

### Redireccionamiento Entrada/Salida:
- stdin: ```<```
```
cat < fichero
mail usuario < fichero
```
- stdout: ```>``` (ó ```1>```).
```
ls > fich
# o ls 1> fich
```
- Append: ```>>```
```
ls >> fich
```
- stderr: ```2>```
```
ls no-existe 2> errores
ls no-existe 2>> errores
```

<a name="Bash"/>

### Trabajando con el interprete Bash
El caracter ```;``` permite ejecutar varios comandos de manera secuencial, en una sola línea 

El caracter ```&``` se introduce al final de un comando, entrando en modo "spawn" lo que permite ejecutar otro comando,aunque el comando anterior no haya terminado

El caracter ```!``` es un comando que ejecuta el ultimo comando que coincida con el patron de caracteres que le preceden:
Si tenemos el siguiente historial de comandos:
``` 
1 ls -l
2 history
```
Si ejecutamos: ```!l ``` ejecutara el comando ```ls -l ```

```cntrl+c``` abortar la ejecucion del programa (lo mata)
```cntrl+z``` detener (pausar) la ejecucion del proceso
```cntrl+d``` fin de la entrada estanda
el comando ```stty``` nos permite cambiar esos atajos

para terminar un proceso se usa el comando ```kill``` seguido del PID (que se obtiene del comando ```ps```). Tambien se pueden usar ciertas señales para ajustarn el comando ```kill -9 [ PID]```

<a name="EstructuraScript"/>

### Estructura de un script 
Un script esta compuesto por las siguientes partes:
- **cabecera**: se suele empezar por: ```#!/bin/sh``` o  ```#!/bin/bash```. Con esto, el script se ejecutará con el IC indicado 
- **descripción**
- **sentencias**
- **devolver el codigo de finalizacion**: En caso de que no haya error => ```exit 0```. En caso contrario ```exit cod_error```

Ejemplo de un script completo:
```bash
#!/bin/bash
# Un comentario random
echo "test"
exit 0
```
Para ejecutar un script:
```sh miScript.sh```

Puede que se necesiten permisos de ejecucion. En ese caso le daremos los permisos necesarios:
```chmod +rx miScript.sh```

Se puede ejecutar un script de 3 formas:
- con path absoluto: ```/home/user/bin/miScript.sh```
- desde el direcorio actual ./ miScript.sh
- configurando la variable de entrono PATH (```/home/user/bin/```): miScript.sh


Podemos definir distintios tipos de variables: locales, de entorno, parametros.

Una variable contiene una **cadena de caracteres**

Para leer del teclado usamos el comando ```read VAR1 VAR2```

Para obtener la lista de argumentos utilizaremos ```$*``` o ```$@```

Para obtener el numero de argumentos utilizaremos ```$#```

En bash la variable ```$?``` almacena el resultado de la última operación realizada en el sistema


El comando ```expr``` resuelve operaciones aritméticas. Por ejemplo ```expr 2 + 3```. Para asignar el contenido de la expresion a una variable, se utiliza ```VAR = `expr 2 + 5` ```

El comando ```eval```, evalua un string (lo ejecuta) 
```bash
FILE="/home/user1/file.txt"
echo ${FILE#/home/}  # al utilizar la `#` elimina la primera aparicion de `/home/`

```
Comillas simple vs doble
La comilla simple no interpreta el contenido del string:
```bash
A="Algo"
B='$A'   #B tiene el string $A
```
La comilla doble si lo interpreta:
```bash
A="Algo"
B="$A"   #B tiene el string Algo
```
 
El acento grave (``` ` ```), nos permite ejecutar un string como un comando en medio de otro sting:
```bash
echo "hola, estos son los procesos abiertos: `ps`"
```

Estructuras de Control:
Si una condicion:
  - se cumple => devuelve 0
  - no se cumple => devuelve 1

El comando test permite evaluar expresiones condicionales
El doble ampersan (```&&```) se utiliza para hacer un if: ``` comando1 && comando2 === if comando1 then comando2 ``` 
El ```||``` se utiliza para hacer un else: ``` comando1 || comando2 === if comando1 !=0 then comando2 ``` 

<a name="GestionRecursos"/>

## 3: Gestion de recursos

<a name="SisFich"/>

### Sistema de ficheros

Comando         | Info
---		| ---
cat             |"concatenar" imprime el contenido de un fichero en la salida estandar concatenando (en caso de que hubiese), los ficheros pasados por los argumentos
more		|muestra el contenido de un fichero pagina a pagina. El tamaño de dicha pagina, se delimita por el tamaño de la terminal (version primitiva -> version mas moderna: ```less```)
less		|similar al comando ```more```pero con mas funcionalidades
grep		|busca el contenido de un fichero en busca de lineas que contengan un patron. Si no se le especifica un fichero, lo busca en la entrada estandar
wc		|cuenta el numero de lineas, palabras, y bytes de un fichero
touch		|crea un fichero con el nombre pasado como parametro. Si ya existe, modifica la fecha de acceso
rm		|borra un fichero (por defecto, no borra directorios)

Comando         | Info
---		| ---
df		|muestra el espacio disponible en disco de las unidades montadas
du		|resumen recursivo del uso del disco de los ficheros  del directorio pasado como parametro
du -s		|resumen solo los ficheros del directorio pasado como parametro

Permiso de escritura de un directorio es el permiso de borrado,creado o modificado de ficheros de ese directorio.

Permiso de ejecucion del directorio es para hacer cd o para acceder a un subdirectorio de ese direcotrio.

Permiso de lectura del un directorio es ver su contenido.

La mascara no guarda los permisos, es la mascara inversa. ejemplo 0000(en verdad seria 7777) todos los permisos(especiales-propietario-grupo-otros).

Un sistema cuenta con discos. Esos discos se particionan porque de esa manera se puede hacer una mejor gestion. En linux, el comando que crea particiones es ```fdisk``` que hace que se creen nuevas entradas en la tabla de particiones. Siempre se tiene que hacer sobre discos que no esten en funcionamiento (que no esten montados). Sobre la particion (o el disco entero) se establece la estructura de ficheros (crear la estructura de ficheros o formato). En linux se formatea con ```mkfs```. Para asignar un disco sobre un directorio, es necesario que el directorio en que se quiere asignar el disco, este vacio. 

En algunos casos, nos puede interesar tener un dico cifrado (se evita que desde otro ordenador accedieran remotamente)
Con RAID(Redundant Array of Independent Disks), podemos almacenar una copia para que en caso de perdida del disco original, se pueda recuperar.

Las particiones, permiten acceder mas rapido, pero producen fragmentacion. Son menos flexibles.

Montar significa asignar/ubicar una particion a un direcotrio. Cuando se apaga una maquina, se desmontan los dicos. Cuando encendemos la maquina, los discos ya estan montados, eso es porque en el fichero ```/etc/fstab```, que contiene como se montan las particiones (esten o no montados)

Las particiones se identifican (en el directorio ```/dev```)en kis ficheros sda 1, sdb2, ... .Tambien se puede identificar con el UUID (Univeraly Unique IDentifier). Tambien se almacena en el directorio ```/dev/disk```. El UUID tambien se usa para identificar software, como la MAC


Un fichero ```core``` o de imagen, se genera cuando un programa aborta en modo desarrollador, donde se almacena el codigo del programa, la pila con el estado de las variables, los datos. Son ficheros muy grandes.

El comando ```find``` tiene una opcion que es ```-mtimpe``` : fecha de modificacion ```-atime```: fecha de ultimo acceso.                tiene una opcion para permisos ```mode```: exactamente ese, ```-mode``` solo los que estan a 1, ```/mode``` es un or de permisos. 
                      ```-o``` es un (or). Para poner un parentesis, se utiliza: ```\(``` y ```\)```. Para poner un ```;```, ponemos ```\;```

<a name="EntTrabajo"/>

### Entorno de trabajo	 

El entorno tiene definida una serie de variables.
La variable PATH varia entre usuarios (admin-root y entre los usuarios ). En la variable LD_LIBRARY_PATH, contiene una lista de librerias dinamica compiladas para los usuarios

El comando ```set``` muestra el contenido de las variables de entorno. 

La variable PWD contiene el directorio actual (algunas apps, no modifican dicha variable cuando acceden a los directorios)
La variable BASH contiene el directorio del terminal
La variable LOGNAME contiene el nombre del usuario conectado

Para asignar variables de entorno, usaremos el comando: ```export var``` o ```export var=5```.
Las variables de entorno creadas por el padre, son heredadas por los hijos (Procesos)(Son una copia). Por ejemplo, si en el bash, abrimos sh, y preguntamos por una variable de entorno definida por el padre, funciona. Pero, si el hijo, crea una variable de entorno, el padre no la tiene.

El comando ```witch``` o ```whereis``` busca un comando en la variable PATH y en el caso del ```whereis``` tambien en el man.

Hay comandos, como el ```ls```, que usan un alias: ```alias ll='ls -al'```. Al ejecutar el comando ```ll``` ejecutará ```ls -al```. Al igual que las variables de entorno, los alias pueden ser locales, o de entorno. 

Los ficheros start-up (o scripts), son los fihceros que se ejecutan cuando se hace login o en algun caso, al instalar algun problema. Tienen extensiones (```csh```,```*rc*```, ). Cada usuario tiene un shell asociado. Este shell esta definido el el fichero ```passwd``` como ultimo argumento. Cada usuario puede modificar sus ficheros de start-up y puede añadir nuevas variables de entorno o nuevos alias

Si un hijo, quiere modificar una variable de entorno del padre, usara ```.``` o el comando interno del bash ```source```

<a name="CCC"/>

### Cuentas, Grupos, Contabilidad

El fichero clave del directorio ```/etc``` es el fichero passwd, que contiene los usuarios instalados (1 liena por comando). El SO tiene definidos otros usuarios internos.
Cada linea tiene la siguiente estructura: root(nombre):pass(si x entonces la pass esta en el fichero ```/etc/shadow```):id:grupo principal: idgrupo

En el fichero ```/etc/shadow```, no solo contiene la contraseña, sino que tambien contiene cuando se creó la cuenta, si se le forzará cambiar la contraseña y cuando, si la cuenta caduca y cuando.   

La variable de entorno ```FS``` es la variable file separator. Por defecto es el blanco ( " " )
Para redirigir un mensaje en la salida de errores: ```echo algo >&2```


Al arrancar el sistema, se ejecuta la BIOS, cuyo objetivo es buscar el cargador del SO. La bios es parametizable. Por ejemplo, un parametro que podemos cambirar, es el orden de busqueda del cargador. En un servidor, la BIOS tiene que estar protegida con una contraseña. Si encuentra entre el orden establecido de los dispositvos conectados el cargador lo ejecuta. El cargador (o bootloader), inicia el sistema operativo y ejecuta el proceso ```vinit```. Dicho proceso es el encargado de poner en marcha todos los servicios del sistema. En el fichero ```/etc/inittab``` se indica que modo de arranque tiene el sistem. 0 -> halt, 1 -> single user(root), 2..5-> multi user, 6 reboot. Cada uno de esos modos (niveles), en el directorio ```/etc``` hay unos directorios ```rc1, rc2, ... rc6``` y en cada uno de los directorios, hay una serie de ficheros que empiezan por ```K``` (se ejecutan al parar la maquina (KILL)) y otros que empiezan con ```S``` (se ejecutan al iniciar la maquina (START)). Cada uno de esos ficheros ademas, tien un numero indicando el orden de ejecucion. Ese numero se establece por las dependencias internas de las aplicaciones
___

<a name="TablaComandos"/>

## Tabla de comandos:

Comando              | Flags | Info
---------------------|-------| ---
cat                  |       |"concatenar" imprime el contenido de un fichero en la salida estandar concatenando (en caso de que hubiese), los ficheros pasados por los argumentos
touch                |       |si no existe un fichero, lo crea, y si existe, cambia la fecha de modificacion del mismo
time                 |       |muestra el tiempo de ejecucion de un script. el tiempo que muestra se divide en dos. el tiempo "user" es el tiempo computable al estado "run", el tiempo "sys, corresponde al tiempo que se encuentran en otros estados 
ps                   |       |muestra los porocesos activos
jobs                 |       |muestra las tareas (no procesos) de fondo de una misma sesion de terminal. Para cambiar entre las tareas que estan en forground y el background usamos los comandos ```fg``` y ```bg```
ls                   |-a     |muestra los ficheros que empiezan por ```.```   
ls                   |-l     |formato largo
wc                   |       |"wordcount". Cuenta lineas, palabras, ...
history              |       |muestra el historial de comandos de una sesion del terminal en orden 
ssh [usuer]@[ip]     |       |establece una conexion segura con una sesion remota
eval                 |       |ejecutar un string como comando
expr                 |       |evaluar expresiones aritmeticas
read                 |       |
grep                 |       |filtrar el primer string que contenga el segundo string: "aaa aa a " grep "a"
who                  |       |devuelve los usuarios conectados
tail                 |       |posicionarte al inicio del fichero, puedes no mostrar el inicio con ejemplo: ```ail --lines=+2```
uniq                 |       | se utiliza para eliminar lineas repetidas y con ``` uniq -c``` te cuenta de cada tipo cuantas ha eliminado.
sort                 |       | ordena las lineas alfabeticamente (posible uso: ordenar una lista por iguales)
umask               |       | seguido del 3 numeros(777 ejemplo) para cambiar los permisos.Sin nada se ven los permisos del usuario, recordar que el numero tiene que ser el inverso al que queremos.


