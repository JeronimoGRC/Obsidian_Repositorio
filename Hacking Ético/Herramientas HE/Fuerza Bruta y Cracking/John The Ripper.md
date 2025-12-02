### Uso
La herramienta tiene como objetivo crackear contraseñas que están hasheadas.

---
### Flags

- `--wordlist=<lista_palabras>` -> Indica la lista de contraseñas a crackear.
- `--format=<tipo>` -> Indica el formato del hash de las contraseñas.

---
### Hallar el hash y formatos en John

Para hallar el hash utilizaremos la herramienta `hash-identifier`.

***Formatos***:

- `raw-sha1`
- `raw-sha512`
- `raw-sha256`
- `raw-md5`
- `phpass`
#### Formatos del /etc/shadow:

- `crypt` -> Su prefijo en el fichero `/etc/shadow` es: `$y$`
- `md5sum` -> `$1$`
- `sha256crypt` -> `$5$`
- `sha512crypt` -> `$6$`

#### Ver la lista de formatos en John the Ripper

```
john --list=formats | grep <formato>
```

---
### Ejemplo

```
john --format=<formato> --wordlist=rockyou.txt <fichero_contraseñas.txt>
```

En este apartado `--wordlist=rockyou.txt` en vez de poner rockyou.txt también podemos poner la ruta de otro diccionario o `/usr/share/wordlist/rockyou.txt.gz`.

**John The Ripper** no vuelve a ejecutar el mismo fichero para ello ejecutamos este comando para ver las contraseñas halladas.

```
john --show --format=<formato> <fichero>
```

---

## Fichero del botín de John

El fichero donde se guardan las contraseñas halladas con sus hashes en la carpeta del usuario y dentro una carpeta oculta llamada `.john`. En el caso de que trabajemos con root estará en la siguiente dirección:
 
```
/root/.john/john.pot
```

---
## Fichero `/etc/shadow`

En el fichero `/etc/shadow` encontramos contraseñas hasheadas de los usuarios del equipo en el siguiente formato:

```
$id$salt$hash
```

Para poder encontrar la coincidencia con una contraseña con este formato la copiamos tal cual en un fichero para poder hallar la coincidencia con John.

Para extraer los hashes en un formato que pueda procesar el **John the Ripper** haremos el siguiente comando:

```
cat /etc/shadow | cut -d":" -f1,2
```

Recomendable poner en el diccionario de las passwords hasheadas poner al principio `1:` y por consiguiente los demás números en orden para que en la salida nos aparezcan a cual pertenecen cada uno:

```
1:090cdb75bd6e6d34eb35a1f788ae774166d4a36d519617e329829d2088471c21
2:f59e28f8bb6499ddf0521ed7a60aeda00957aeb7ee605caf17a216f7a9a26d08
```
