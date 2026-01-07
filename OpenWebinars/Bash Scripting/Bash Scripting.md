
---
## Objetivo

Simplificar y/o automatizar el día a día del administrador o usuario del sistema

- BASH nace en 1988 como una "shell renacida" basada en C, funciona como alternativa libre a Bourne Shell. 

### Tipos de usos

- Agilizar procesos largos:
	- Interacción con comandos del sistema.
	- Simplicidad.
	- Operadores matemáticos, bucles, casos.
- Automatizar acciones repetitivas:
	- Creación de servicios/daemons.
	- Programación en crontab.
	- Lanzamiento automático en login de usuario.
- Mejorar la experiencia del usuario.

---
## Fundamentos Básicos

#### Fichero bash

Extensión: `.sh`
Primera línea del contenido: `#!/bin/bash` 

Ejemplo básico:

```
#!/bin/bash 

echo "Hola Mundo"
```


#### Tratamiento de un fichero bash

Métodos de ejecución:
- `./script.sh`
- Usando su ruta completa
- Situándolo en un directorio especial `/usr/local/bin o /usr/bin`, si situamos los scripts en esas carpetas al querer ejecutar los scripts no tendremos que incluir la ruta, ya que el sistema entiende que esa es su localización por defecto.

Permisos necesarios:

Necesita permisos de lectura y de ejecución.
Tener en cuenta los permisos del script.

![[Pasted image 20251229113721.png]]

`Crontab` nos permite decidir cuando ejecutarlo, ejecuciones en segundo plano

#### Variables

- String
- Numericas
- Arrays o listas
- Salidas de un comando en forma de string

Las variables deben llamarse utilizando `$` delante`.


![[Pasted image 20251229114824.png]]

Salida:

![[Pasted image 20251229114917.png]]

![[Pasted image 20251229115209.png]]

#### Listas o Arrays

![[Pasted image 20251229120726.png]]

#### Condicional

![[Pasted image 20251229121001.png]]

#### Operadores matemáticos y sintaxis

##### Sintaxis:

- No obliga a tabular
- MUY susceptible a los espacios
- Permite uso de operadores matemáticos y puertas logicas
- Los comentarios se indican con `#`

##### Operadores matemáticos
- `+`
- `-`
- `*`
- `/`
- `%`
##### Comparaciones lógicas

- `-lt` -> (<) -> menor que
- `-gt` -> (>) -> mayor que
- `-le` -> (<=) -> menor o igual que
- `-ge` -> (>=) -> mayor o igual que
- `-eq` -> (`==`) -> igual
- `-ne` -> (!=) -> distinto
- `-n` -> not null
- `-z` -> null

Ambas opciones funcionan correctamente.

##### Operadores condicionales de fichero

- `-e` Si existe el fichero/directorio.
- `-f` Si existe el fichero y NO es un directorio.
- `-s` Si el fichero no está vacío.
- `-d` Si es un directorio.
- `-r` Si el directorio tiene permisos de lectura.
- `-w` Si el directorio tiene permisos de escritura.
- `-x` Si el directorio tiene permisos de ejecución.

##### Puertas lógicas

- && (-a)
- || (-o)
- AND (Y)
- OR (O)

Ejemplo

![[Pasted image 20251229125536.png]]

Para evitar confusiones se ha utilizado `!=` 

![[Pasted image 20251229130239.png]]

Pregunta si tiene permisos de lectura (`-r`) y de ejecución (`-x`)

![[Pasted image 20251229130848.png]]

Pregunta si es o no un directorio

![[Pasted image 20251229130826.png]]

Para debuggear:

```
sh -x scripts.sh
```

Para visualizar posibles errores en el código añadimos lo siguiente:

```
set -u:
```

---

## Argumentos de entrada

#### ¿Qué son?

Son valores que le indicamos al script antes o durante la ejecución.

#### Tipos

- Input "$0", "$1", "$2"
- Input $* (string) o $@ (lista)
- Input $# (cant. de valores facilitados)
- Argumento de entrada interactivo: Input "read"

#### Argumento de entrada previo

`$0` suele almacenar el nombre del script en una variable.
A partir de la variable $1 se almacena todos los valores indicados previamente.


![[Pasted image 20251230132852.png]]

![[Pasted image 20251230132952.png]]
#### Argumento de entrada interactivo

La función **read** nos permitirá escribir un valor, que bash asignará a una variable

![[Pasted image 20251230133534.png]]

![[Pasted image 20251230133431.png]]


---

## Bucles y casos

**while**:
- No es autoincremental.
- Se ejecutan unas acciones en bucle siempre que se cumplan una condición.

**Usos**:
- Bucles matemáticos.
- Bucles basados en string.
- Bucles infinitos.

```
#!/bin/bash

i=5

echo "Cuenta atrás..."

while (( i >= 1 ))
do
        echo $i
        sleep 1
        ((i--))
done
```

`sleep` -> espera x cantidad de segundos marcada.

Ejemplo de `while true`:

```
while true 
do 
        echo "No voy a parar hasta que tu me interrumpas"
        sleep 1
done
```

**for**:
- Se ejecutan unas acciones em bucle siempre que se cumpla la condición.
- Autoincremental.
- Indexa valores.

**Usos**:
- Basados en recorrer y usar secuencial/lista de valores dentro de un rango y ejecutar acciones de forma condicional (anidación).

```
#!/bin/bash

lista=(Alberto Maria Pepe Carla)

for i in ${lista[@]}
do
        echo $i
done
```


Ejemplo de **for** con un **if** dentro:

```
for i in ${lista[@]}
do 
        if [[ $i == A* ]]
        then
                echo $i
        fi
done
```

Solo mostrará los nombres de la lista que empiecen por `A`

**case**:
- Orientado a al interacción con el usuario.
- Muy útil para control de procesos.
- Contempla posibilidades.
- En las opciones puede haber varias opciones, es decir, opciones anidades.

**Uso**:
- Casos basados en control de procesos.
- Casos basados en menús de usuario.
- Casos personalizados (haz lo que quieres).

Ejemplo:

```
#!/bin/bash

case $1 in
        [Ss]tart)
        echo "Iniciando el programa"
        ;;
        [Ss]top)
        echo "Parando el programa"
        ;;
        help|*)
        echo "Para usar el script usa stop | start"
        ;;
esac
```

En este ejemplo podemos ver el caso 1 con funciones anidadas. `[Ss]` significa  que puede empezar por s mayúscula o minúscula. `help|*` significa que introduzca la palabra help o cualquier otra cosa.

Para ejecutar el programa tenemos estos ejemplos.

```
bash case.sh stop
bash case.sh Start
bash case.sh ssdjlñdsk
```

Ejemplo práctico de todos los bucles:

![[Pasted image 20251231130021.png]]

Declaramos un array que contenga los nombres de los directorios, iniciamos el case con el primer caso que va a variar en función de si queremos comprobar o crear un directorio.

En la opción de check comprobamos la existencia de los directorios recorriendo el array y utilizando el `-d`.

En la opción de make creamos un directorio si al iterar la lista comprobamos que no existe, en el caso de que exista avisamos al usuario de su existencia.

La última opción es por si el usuario introduce otra opción no controlada.

Salida:

![[Pasted image 20251231130647.png]]


---

## Como usar ficheros foráneos

Es un fichero externo que usaremos como variable. De forma recomendable será texto plano y podrá ser tratado como argumento de entrada y/o como variable interna.

#### Recorrer un fichero de input

- `while read LINE`

![[Pasted image 20251231184922.png]]

![[Pasted image 20251231185720.png]]

`for`

![[Pasted image 20251231190237.png]]

![[Pasted image 20251231190326.png]]

Ejemplo complejo

![[Pasted image 20251231191542.png]]

Salida:

![[Pasted image 20251231191512.png]]