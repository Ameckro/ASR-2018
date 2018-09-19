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
(pag 9)
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
---
(pag 15)
#### 1. Programar un script que muestre el nombre del script.
```

```
#### 2. ¿Dónde debería ser copiado el script para que se pueda ejecutar como cualquier comando del sistema (sin hacer referencia al directorio en el que está), como por ejemplo, $ ls?
```

```
#### 3. Programar un script que obtenga un texto (cualquiera) por la salida estándar y otro por la salida estándar de errores.
```

```
#### 4. ModiVcar el script del punto anterior para que desde el interior del propio script redirija la salida estándar al Vchero “nombre-del-script-output.txt” y la de errores a “nombre-del-script-errors.txt”.
```

```
---
(pag 19)
#### 1. Programa un script que calcula y muestra la suma de tres números que recibe como parámetro.
```

```
#### 2. Programa un script que calcula y muestra la suma de los números que recibe como parámetro. El número de parámetros no se conoce a priori.
```

```
#### 3. Como el anterior, pero en lugar de recibir los números como parámetros, los recibe a través del teclado.
```

```
#### 4. Programa un script que muestre sólo los nombres de los Vcheros del directorio principal, es decir, sin la parte correspondiente al directorio en el que están ni la extensión. Ejemplo: /home/user/File1.txt ⇒ File1
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
- desde el direcorio actual ./ miScript.sh
- configurando la variable de entrono PATH (```/home/user/bin/```): miScript.sh

Podemos definir distintios tipos de variables: locales, de entorno, parametros.

Una variable contiene una **cadena de caracteres**

Para leer del teclado usamos el comando ```read VAR1 VAR2```

Para obtener la lista de argumentos utilizaremos ```$*``` o ```$@```

Para obtener el numero de argumentos utilizaremos ```$#```

El comando ```expr``` resuelve operaciones aritméticas. Por ejemplo ```expr 2 + 3```. Para asignar el contenido de la expresion a una variable, se utiliza ```VAR = `expr 2 + 5` ```

El comando ```eval```, evalua un string (lo ejecuta) 
```
FILE="/home/user1/file.txt"
echo ${FILE#/home/}  # al utilizar la `#` elimina la primera aparicion de `/home/`

```
Comillas simple vs doble
La comilla simple no interpreta el contenido del string:
```
A="Algo"
B='$A'   #B tiene el string $A
```
La comilla doble si lo interpreta:
```
A="Algo"
B="$A"   #B tiene el string Algo
```
 
El acento grave (``` ` ```), nos permite ejecutar un string como un comando en medio de otro sting:
```
echo "hola, estos son los procesos abiertos: `ps`"
```

Estructuras de Control:
Si una condicion:
  - se cumple => devuelve 0
  - no se cumple => devuelve 1

El comando test permite evaluar expresiones condicionales
El doble ampersan (```&&```) se utiliza para hacer un if: ``` comando1 && comando2 === if comando1 then comando2 ``` 
El ```||``` se utiliza para hacer un else: ``` comando1 || comando2 === if comando1 !=0 then comando2 ``` 



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
