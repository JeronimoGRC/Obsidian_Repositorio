## ¿Qué es?

Es un servicio de conexión remota a otra máquina que usualmente se carga en el puerto 22

Gracias al Open SSH que es un conjunto de herramientas que permiten conectarse de forma segura a sistemas remotos mediante el protocolo SSH:

1. [[PKI-SSH]]
2. [[PSSH]]
3. [[SFTP]]
4. [[SSH-Agent]]
5. [[SSH-Tunnels]]
### Comandos básicos


```
ssh <usuario>@<ip>
ssh -p <puerto> <usuario>@<ip>
```

### Configuración del SSH

A través del archivo `/etc/ssh/sshd_config` que es el fichero del servidor **SSH**:
###### 🔐 Opciones esenciales para bastionado

| Directiva                      | Explicación                                                      |
| ------------------------------ | ---------------------------------------------------------------- |
| `Port 22`                      | Puerto donde escuchará el servidor. Cambiarlo aumenta seguridad. |
| `PermitRootLogin no`           | Permite la configuración de acceder con root.                    |
| `PasswordAuthentication no`    | Solo claves públicas. Más seguro.                                |
| `PubkeyAuthentication yes`     | Habilita el login por clave pública.                             |
| `AllowUsers usuario1 usuario2` | Restringe qué usuarios pueden conectarse.                        |
| `MaxAuthTries 3`               | Marca el máximo de intentos de autenticación.                    |
| `X11Forwarding`                | Permite ejecutar aplicaciones gráficas                           |
