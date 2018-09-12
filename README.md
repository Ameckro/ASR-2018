# ASR-2018

## 2. Introducción a Shell Scripting

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



### Apuntes
$0 es el nombre el del programa/script que se esta ejecutando

el caracter ```~``` el interprete de comandos lo sustituye por direcotorio padre. Es un metacaracter

el caracter ```*``` el interprete de comandos busca elementos con el patron de caracteres especificado despues de del metacaracter(```*```)

Tabla de comandos:

Comando | Info
---|---
cat | imprime el contenido de un fichero en la salida estandar
touch | si no existe un fichero, lo crea, y si existe, cambia la fecha de modificacion del mismo
