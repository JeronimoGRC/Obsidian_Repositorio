---

---

## Carpetas más importantes de Linux

---
### 🧩 `/bin` -> Binarios esenciales para los usuarios.

- Algunos sistemas modernos lo enlazan a `/usr/bin`

### 🧩 `/sbin` -> Binarios esenciales para la administración del sistema.

- Requieren, normalmente, privilegios de *root*

### 🔛 `/boot` -> Archivos necesarios para el arranque.

### `/dev` -> Dispositivos representados como archivos.

### ⚙️ `/etc` -> Archivos de configuración del sistema.

### 🏠`/home` -> Directorios personales de los usuarios.

### 👨‍💼 `/root` -> Directorio personal del usuario root.

### `/usr` -> Software y datos compartidos.

- Subdirectorios importantes:
	- `/usr/bin` -> programas
	-  `/usr/sbin` -> herramientas administrativas
	- `/usr/lib` -> bibliotecas
	- `/usr/share` -> datos independientes de arquitectura

### `/var` -> Datos variables.

- Subdirectorios importantes:
	- `/var/www/html` -> Raíz del servidor Apache.
	- `/var/log` -> Logs del sistema.
	- `/var/lib` -> Estado de servicios.

### ⏱️ `/tmp` -> Archivos temporales.

### `/opt` -> Software adicional instalado manualmente.

### 📸 `/media` -> Montaje automático de dispositivos extraíbles. 

---
## 🧑‍💼📁 Configuraciones global de usuario

Se ubican en la carpeta `/etc`:

- `/etc/passwd` -> Lista de usuarios.
- `/etc/shadow` -> Hashes de las contraseñas.
- `/etc/group` -> Grupos del sistema.
- `/etc/sudoers` -> Permisos de sudo.
- `/etc/pam.d` -> Configuración de autenticación PAM por servicio.

## 🛜 Configuración de red

- `/etc/hosts` -> Archivo de asociación manual IP <-> hostname.
- `/etc/hostname` -> Nombre del equipo.
- `/etc/hosts.allow` y `/etc/hosts.deny`-> Control de acceso TCP.
- `/etc/network/interfaces` -> Configuración clásica de interfaces.

## Servicios y demonios

- `/etc/systemd/system` -> Servicios personalizados.
- `/lib/systemd/system` -> Servicios instalados por paquetes.

## [[SSH]]

- `/etc/ssh/sshd_config` -> Configuración del servicio **SSH**.
- `~/.ssh/authorized_keys` -> Claves autorizadas.

## 📎Tareas programadas

- `/etc/crontab`
- `/etc/cron.d/`
- `/etc/cron.daily/`

## 📋Logs y auditorías

- `/var/log/`:
	- `auth.log`
	- `secure`
	- `kern.log`
	- `apache2/`, `nginx/`

---

## Kali Linux

### 🔧Herramientas y binarios clave

- `/usr/bin` -> Contiene la mayoría de todas las herramientas.
- `/usr/share/` ->
	- `/usr/share/wordlists`->
		- `rockyou.txt`.
		- Listas para fuzzing y fuerza bruta.
	- `/usr/share/metasploit-framework/`-> Módulos de explotación, payloads, exploits usuales.
	- `/usr/share/sqlmap/` -> Scripts internos de [[SQLmap]].
	- `/usr/share/nmap/` -> Scripts de [[NMAP]] en formato `.nse`.
	- `/usr/share/john` -> Incluye un diccionario de contraseñas y reglas internas 
	- `/usr/share/exploitdb/` -> Base de datos local de exploits usado con searchsploit.
	- `/usr/share/webshells/` -> Scripts para shell inversa


### Configuración de Proxy

- `/etc/proxychains.conf` -> Pivoting, encadenamiento de proxys

### Herramientas que no siguen estructura estándar.

Ejemplo frecuente:

- `/opt/BloodHound/`
- `/opt/linpeas/`
- `/opt/kerbrute/`