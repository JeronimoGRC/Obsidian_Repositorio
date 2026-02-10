
---

### Definiciones Fundamentales

- **LFI (Local File Inclusion):** Vulnerabilidad que permite a un atacante leer archivos locales en el servidor debido a una falta de saneamiento en funciones que incluyen archivos (ej. `include()` en PHP).
    
- **RCE (Remote Code Execution):** El objetivo final donde el atacante logra ejecutar comandos arbitrarios en el sistema operativo del servidor.
    
- **Envenenamiento de Logs (Log Poisoning):** Técnica que consiste en escribir código malicioso (payload) en los archivos de registro (logs) del servidor para luego ejecutarlo mediante una vulnerabilidad LFI.
    
---
### El Código Vulnerable (Ejemplo)

El problema reside en usar entradas del usuario (`$_GET`) directamente en funciones de inclusión:

```
$file = $_GET['file'];
include($file); // Vulnerabilidad aquí: permite incluir archivos como /etc/passwd o logs.
```

---
### Vectores de Ataque Comunes

Para que el ataque funcione, se necesita un archivo de log que registre datos controlables por el usuario:

| **Servicio** | **Archivo de Log Objetivo**   | **Vector de Inyección (Payload)**                      |
| ------------ | ----------------------------- | ------------------------------------------------------ |
| **Apache**   | `/var/log/apache2/access.log` | Encabezado **User-Agent** en la petición HTTP.         |
| **SSH**      | `/var/log/auth.log`           | Campo de **nombre de usuario** en el intento de login. |

### Proceso del Ataque (Paso a Paso)

- Interceptar la petición con [[Burp Suite]].
- Modificamos el parámetro **User-Agent** con un código malicioso.
![[Pasted image 20260210191441.png]]
![[Pasted image 20260210191651.png]]
- Ejecutar comandos mediante LFI.

```
vulnerabilities/fi/?page=../../../../../../var/log/apache2/access.log&cmd=hostname
```
