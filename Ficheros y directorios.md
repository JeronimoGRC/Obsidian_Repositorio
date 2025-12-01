---

---
---

# Directorios

## Linux Normal

---

### ⚙️ **/etc** — Configuración del sistema

Archivos importantes:

- `/etc/passwd` → usuarios
    
- `/etc/shadow` → hashes de contraseñas en el siguiente formato: `$id$salt$hash`
    
- `/etc/group` → grupos
    
- `/etc/sudoers` → permisos sudo
    
- `/etc/hosts` → resolución local
    
- `/etc/hostname` → nombre del host
    
- `/etc/network/interfaces` → configuración de red (distros basadas en Debian)

---

### 🔐 **/sbin** y **/usr/sbin**

Herramientas administrativas para root (`iptables`, `service`, `mount`).

---
### 🧩 **/usr**

Contiene:

- `/usr/bin` → programas
    
- `/usr/sbin` → herramientas admin
    
- `/usr/lib` → librerías
    
- `/usr/share` → archivos compartidos (iconos, manuales, datos)

---
### 🔐 **/root**

Home del usuario root.

---
### 🧪 **/tmp**

Archivos temporales borrados al reiniciar.

---
### 📦 **/opt**

Programas instalados manualmente o externos.

---
### 🖥️ **/dev**

Dispositivos del sistema:

- `/dev/sda` (disco)
    
- `/dev/tty`
    
- `/dev/`

## Kali Linux

### 🧰 **/usr/share/**

Contiene los recursos de todas las herramientas de seguridad:
- `/usr/share/nmap/scripts/` → scripts NSE
    
- `/usr/share/metasploit-framework/` → módulos de Metasploit
    
- `/usr/share/wordlists/`
    
    - Incluye **rockyou.txt**
---
### 🧨 **/opt/** — Herramientas instaladas manualmente

Kali lo usa muchísimo:

- `/opt/metasploit/`
    
- `/opt/wordlists/`
    
- `/opt/burpsuite/`
    
- Herramientas externas de GitHub (gobuster, linpeas, etc.)
---
### 📝 **/etc/** (Kali añade configuraciones extra)

Además de los ficheros típicos de Linux, Kali incluye:

- `/etc/metasploit-framework/`
    
- `/etc/nmap/`
    
- `/etc/ssh/`
    
- `/etc/hosts.allow` y `/etc/hosts.deny` (control de accesos)

---
# Archivos

## 🧑‍💻 **Usuarios, contraseñas y grupos**

- `/etc/passwd` -> Lista de usuarios del sistema
- `/etc/shadow` -> Hashes de contraseñas
- `/etc/group` -> Grupos del sistema
- `/etc/gshadow` -> Grupos seguros

---

## 🔏 **Políticas de contraseñas y autenticación**

- `/etc/login.defs` -> Reglas globales de contraseñas
- `/etc/pam.d/` -> Configuración de PAM
- `/etc/pam.d/common-auth` -> Reglas de autenticación
- `/etc/pam.d/common-password` -> Complejidad de contraseñas

---

## **🌐 Red, servicios y hosts**

- `/etc/hosts` -> Resolución local de nombres
- `/etc/hostname` -> Nombre de la máquina
- `/etc/network/interfaces` -> Configuración manual de red
---

## 🔥 **Firewall y seguridad del kernel**

- `/etc/ufw` -> Configuración del firewall UFW
- `/etc/ufw/ufw.conf` -> Activación y políticas
- `/etc/sysctl.conf` -> Parámetros del kernel 

---
## 🛠️ **Archivos importantes de configuración de servicios**

### ⬛ **[[SSH]]**

- `/etc/ssh/sshd_config` -> Configuración del servidor SSH
- `/etc/ssh/ssh_config` -> Configuración del cliente SSH

### 🌐 **Apache**

- `/etc/apache2/apache2.conf` -> Configuración principal
- `/etc/apache2/ports.conf` -> Puertos
- `/etc/apache2/sites-available/` -> Virtual hosts
- `/var/www/html/` -> Ubicación de la página de Apache2

### 📨 **FTP (vsftpd)**

- `/etc/vsftpd.conf` -> Configuración FTP segura