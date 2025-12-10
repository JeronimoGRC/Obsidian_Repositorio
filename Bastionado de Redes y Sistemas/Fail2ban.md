
---
## ¿Qué es?

Es una herramienta de seguridad preventiva que protege servicios frente a ataques de fuerza bruta

---
## Archivos importantes

- `/etc/fail2ban/fail2ban.conf` -> Configuración por defecto ***(no tocar)***
- `/etc/fail2ban/jail.conf->` Configuración personalizada, el original es con extensión `.conf` recomendable trabajar en esa copia con extensión `.local`.

```
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local 
```

- `/etc/fail2ban/filter.d/*.conf` -> Filtros por servicio

---
## Ejemplos de uso

### Ejemplo de SSH

En el fichero `/etc/fail2ban/jail.local:

```
[sshd]
enabled = true
port = 22
backend = systemd
maxretry = 3
findtime = 600
bantime = 600
```

**Resultado**:

- `port = 22` -> Puerto por el que actuara la configuración de fail2ban en el servidor ssh.
- `maxretry = 3` -> Máximos intentos.
- `bantime = 600` -> Tiempo de baneo en segundos
- `findtime = 600` -> Tiempo que disponible para hallar la contraseña


#### Comandos clave

```
fail2ban-client status sshd
fail2ban-client set sshd unbanip <IP>
```


### Ejemplo de FTP

En el fichero `/etc/fail2ban/jail.local:

```
[vsftpd]
enabled = true
port = ftp
logpath = /var/log/vsftpd.log
maxretry = 3
findtime = 600
bantime = 600
```

- `port = ftp` -> El puerto por el que corra el servicio. 
- `logpath = /var/log/vsftpd.log` -> El servicio FTP debe generar logs y los hará en esa ruta

También es necesario configurar para los logs el siguiente fichero `/etc/vsftpd.conf` con el siguiente contenido:

```
xferlog_enabled=YES
log_ftp_protocol=YES
```

#### Comandos clave

```
fail2ban-client status vsftpd
fail2ban-client set vsftpd unbanip <IP>
```

### Ejemplo de MySQL/MariaDB

Este ejemplo es más complejo porque el MySQL/MariaDB no genera el fichero `error.log` por lo tanto Fail2ban.
### Solución:
Usar **systemd-journald** como fuente de logs.

### Filtro MySQL (adaptado a MariaDB real)

```
[Definition]
journalmatch = _SYSTEMD_UNIT=mariadb.service
failregex = \[Warning\]\s+Access denied for user '.*'@'<HOST>' \(using password: YES\)
ignoreregex =
```

### Jail MySQL (sin logpath)

```
[mysql-auth]
enabled = true
backend = systemd
filter = mysql-auth
port = mysql
maxretry = 3
findtime = 600
bantime = 600
```

### Comandos clave

```
fail2ban-client status mysql-auth
fail2ban-client set mysql-auth unbanip <IP>
```

---

## Comandos esenciales

### Instalación

```
apt install fail2ban
```
### Estado general

```
fail2ban-client status
```

### Estado de un jail

```
fail2ban-client status <nombre de la jail>
```

### Desbanear IP

```
fail2ban-client set <NOMBRE_JAIL> unbanip <IP>
```

### Comprobación de configuración

```
fail2ban-client -t
```