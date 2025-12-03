Herramienta que funciona a través del servicio [[SSH]] para ejecutar comandos simultáneamente en múltiples hosts.

---
### Comando de instalación:

```
pip install git+https://github.com/lilydjwg/pssh
```

---
### Ejecución de comando

```
pssh -h hosts.txt -l <usuario> -i "<comando>"
```

- `-h hosts.txt` utiliza un archivo de en el que están los hosts con los que utilizaremos el pssh.
- `-l <usuario>` indica el usuario. Si no lo pongo es con el usuario con el que estoy trabajando
- `-i "<comando>"` el comando que se ejecutará en los host.

`-A` para forzar a que me pida una contraseña o frase de paso.