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
- `crypt` -> Su prefijo en el fichero `/etc/shadow` es: `$y$`
---

### Ejemplo

```
john --format=<formato> --wordlist=rockyou.txt <fichero_contraseñas.txt>
```

En este apartado `--wordlist=rockyou.txt` en vez de poner rockyou.txt también podemos poner la ruta de otro diccionario o `/usr/share/wordlist/rockyou.txt.gz`.


En el fichero `/etc/shadow` encontramos contraseñas hasheadas de los usuarios del equipo en el siguiente formato:

```
$id$salt$hash
```

Para poder encontrar la coincidencia con una contraseña con este formato la copiamos tal cual en un fichero para poder hallar la coincidencia con John.