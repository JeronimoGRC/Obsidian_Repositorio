
---

El servicio **SFTP** es el simplemente el servicio [[FTP]] funcionando a través del servicio [[SSH]], eso lo convierte en un protocolo seguro y cifrado, además de que al correr por *SSH* funciona por el puerto 22.

---

### Conexión SFTP

```
sftp <usuario>@<IP>
```

---

### Comandos dentro de la sesión SFTP

#### Remoto

| Comando           | Función                                                                 |
| ----------------- | ----------------------------------------------------------------------- |
| `cd <directorio>` | Cambiar el directorio **remoto** (en la máquina a la que te conectaste) |
| `pwd`             | Muestra el directorio **remoto actual**                                 |
| `ls` o `dir`      | Lista los archivos en el directorio **remoto**                          |

#### Local

| Comando            | Función                                         |
| ------------------ | ----------------------------------------------- |
| `lcd <directorio>` | Cambiar el directorio **local** (en tu máquina) |
| `lpwd`             | Muestra el directorio **local actual**          |
| `lls`              | Lista los archivos en el directorio **local**   |
