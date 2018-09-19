# ASR-2018
- [Introducción a Shell Scripting](#IntroScripting)
  - [Ejercicios](#Ej2)
  - [Conceptos Basicos](#ConceptosBasicos)
  - [Redireccionamiento de Entrada/Salida](#RedireccionamientoES)
  - [Trabajando con el Bash](#Bash)
  - [Estructura de un Script](#EstructuraScript)
- [Tabla de comandos](#TablaComandos)
  


<a name="IntroScripting"/>

## 2. Introducción a Shell Scripting

<a name="Ej2"/>

### Ejercicios Scripting

#### 1. Averigua cuál es el intérprete de comandos por defecto en tu sistema.
``` 
echo $0
```


#### 2. Lanza sh desde el terminal, comprueba que realmente se está ejecutando y regresa al IC original.
```
sh
echo $0 
exit
```


#### 3. Guarda los nombres de las entradas (contenido) del directorio /etc en el fichero de nombre file.tmp.

``` 
ls -l /etc > file.tmp
```
#### 4. Añade al Fichero del punto anterior los nombres de las entradas (contenido) del directorio /bin.
```
ls /bin >> file.tmp
```

#### 5.En una sola instrucción, guarda los nombres de las entradas (contenido) de los directorios /etc y /usr en el fichero file.tmp.
```

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
```
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
- ![#f03c15](https://placehold.it/15/f03c15/000000?text=+)  desde el catalogo: ./ miScript.sh (Duda: que es el catalogo, como se añaden?)
 - configurando la variable de entrono PATH: miScript.sh

Podemos definir distintios tipos de variables: locales, de entorno, parametros.
___

<a name="TablaComandos"/>

patata
## Tabla de comandos:

Comando | Flags | Info
--------|-------| ---
cat     |       |"concatenar" imprime el contenido de un fichero en la salida estandar concatenando (en caso de que hubiese), los ficheros pasados por los argumentos
touch   |       |si no existe un fichero, lo crea, y si existe, cambia la fecha de modificacion del mismo
time    |       |muestra el tiempo de ejecucion de un script. el tiempo que muestra se divide en dos. el tiempo "user" es el tiempo computable al estado "run", el tiempo "sys, corresponde al tiempo que se encuentran en otros estados 
ps      |       |muestra los porocesos activos
jobs    |       |muestra las tareas (no procesos) de fondo de una misma sesion de terminal. Para cambiar entre las tareas que estan en forground y el background usamos los comandos ```fg``` y ```bg```
ls      |-a     |muestra los ficheros que empiezan por ```.```   
ls      |-l     |formato largo
wc      |       |"wordcount". Cuenta lineas, palabras, ...
history |       |muestra el historial de comandos de una sesion del terminal en orden 
