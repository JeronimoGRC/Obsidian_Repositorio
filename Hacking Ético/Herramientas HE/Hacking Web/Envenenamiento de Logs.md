
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

1. **Identificación:** Confirmar que existe LFI intentando leer archivos del sistema como `/etc/passwd`.
    
2. **Verificación de Acceso a Logs:** Comprobar si el LFI permite leer los archivos de log (ej. `access.log` o `auth.log`).
    
3. **Inyección (Envenenamiento):** Enviar una petición maliciosa que contenga código PHP simple, como `<?php system($_GET['c']); ?>`.
    
4. **Ejecución:** Acceder al log vía LFI y pasar el comando deseado por el nuevo parámetro (ej. `&c=ifconfig`).
    
5. **Escalada Final:** Usar herramientas como **Metasploit** (módulo `web_delivery`) para obtener una **Shell Inversa** y control total