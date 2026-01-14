
---

Para mantener seguros nuestros ficheros a través de encriptación simétrica y asimétrica.

![[Pasted image 20251201195355.png]]


### GPG
---

#### Uso

Es una herramienta de criptografía para cifrar, descifrar y firmar archivos y mensajes.

---

### Comandos

Para **cifrado simétrico**. 

- Cifrar un archivo.

```
gpg -c <archivo>
```

o

```
gpg --symmetric <archivo>
```

- Descifrar archivo.

```
gpg -d <archivo>.gpg
```

o

```
gpg --decrypt <archivo>.gpg
```

Para **cifrado asimétrico**:

- Generar el par de claves (pública y privada).

```
gpg --full-generate-key
```

Para mostrar las claves generadas:

```
gpg --list-keys
```

Tenemos que tener en cuenta que usaremos como identificador de las claves el correo que le hemos introducido en los valores que nos preguntan:

#### Exportar clave pública.

```
gpg -a --export <correo_identificador> > <clave_publica>.gpg.asc
```

	 `-a` Para que la salida sea ASCII

Una vez pasemos el `.asc` a la otra máquina, debemos hacer el siguiente comando para importar la clave.

```
gpg --import <archivo>.asc
```

- Cifrar archivos de forma **asimétrica** con la clave pública generada por el receptor.

```
`gpg -a -r <correo_perteneciente_a_la_clave_generada> --encrypt <nombreDelArchivo>
```

- Descifrar archivo de forma **asimétrica**

```
gpg -a --decrypt <archivo_encriptado> > <archivo_desencriptado>
```

- Para firmar un archivo 

```
gpg --encrypt --sign --armor -r nombre_del_compañero <fichero>
```

Explicación:

- `--encrypt` → cifra el archivo.
- `--sign` → lo firma con tu clave privada.
- `--armor` → genera un archivo en ASCII (`.asc`), legible como texto.
- `-r nombre_del_compañero` → destinatario de la encriptación.


