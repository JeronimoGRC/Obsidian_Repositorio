Herramienta que funciona a través del servicio [[SSH]] para ejecutar comandos simultáneamente en múltiples hosts.

---
### Comando de instalación:

```
pip install git+https://github.com/lilydjwg/pssh
```

---
### Ejecución de comandos

```
mkdir -p /root/.ssh
chmod 700 /root/.ssh
ssh-keyscan <hosts> >> /root/.ssh/known_hosts
chmod 600 /root/.ssh/known_hosts
```

Creamos para el usuario root el fichero `.ssh` con sus permisos correspondientes. Y escribimos el comando `ssh-keyscan` para escanear las claves de todos los host, para posteriormente darle los permisos necesarios.

```
pssh -h hosts.txt -i -A "<comando>"
```

- `-h hosts.txt` utiliza un archivo de en el que están los hosts con los que utilizaremos el pssh.
- `-l <usuario>` indica el usuario. Si no lo pongo es con el usuario con el que estoy trabajando
- `-i "<comando>"` el comando que se ejecutará en los host.

`-A` para forzar a que me pida una contraseña o frase de paso.

***Es necesario configurar de forma correcta el fichero `sshd_config` para la conexión y la ejecución remota de comandos***.




