Herramienta para ejecutar comandos simultáneamente en múltiples hosts.

---
### Comando de instalación:

```
sudo apt install pssh
```

---
### Ejecución de comando

```
pssh -h hosts.txt -l <usuario> -i "hostname"
```

- `-h hosts.txt` utiliza un archivo de en el que están los hosts con los que utilizaremos el pssh.
- `-l <usuario>` indica el usuario.
- `-i "hostname"` el comando que se ejecutará en los host.