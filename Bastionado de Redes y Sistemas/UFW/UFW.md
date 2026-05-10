
---
![[Pasted image 20260510123138.png]]

![[Pasted image 20260510123200.png]]
## 1. El Ciclo de Vida del Firewall (Habilitar, Apagar y Reiniciar)

Antes de activar el firewall, es fundamental configurar las políticas por defecto y asegurar el acceso SSH para no perder la conexión con el servidor.

- **Ver el estado actual:**

    ```
    sudo ufw status
    ```

_Muestra si está `active` (activo) o `inactive` (inactivo)._

- **Ver el estado detallado (reglas, políticas por defecto y logging):**

    ```
    sudo ufw status verbose
    ```

- **Habilitar el firewall:**

    ```
    sudo ufw enable
    ```

_Activa UFW y hace que se ejecute en cada inicio del sistema._

- **Deshabilitar el firewall:**

    ```
    sudo ufw disable
    ```

_Desactiva el firewall por completo sin borrar tus reglas._

- **Reiniciar UFW a los valores de fábrica:**

    ```
    sudo ufw reset
    ```

_Desactiva UFW y **elimina todas las reglas** que hayas creado. Útil para empezar de cero._

---

## 2. Configuración Inicial Segura (Mejores Prácticas)

La estrategia recomendada para cualquier servidor es: **bloquear todo lo entrante por defecto, permitir todo lo saliente, habilitar SSH y luego activar el firewall.**

### Paso A: Establecer políticas por defecto

Bash

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

- `deny incoming`: Bloquea cualquier intento de conexión externa hacia tu servidor.
    
- `allow outgoing`: Permite que tu servidor acceda a internet para descargar actualizaciones, paquetes, etc.


### Paso B: Asegurar el acceso SSH (¡Evita bloquearte!)

Puedes permitirlo por nombre de servicio o por puerto:

Bash

```
sudo ufw allow OpenSSH
# O bien:
sudo ufw allow 22/tcp
```

> 💡 **Tip de seguridad (Mitigar Fuerza Bruta):** En lugar de permitir el puerto SSH de forma ilimitada, usa `limit`. Esto bloqueará temporalmente a las IPs que intenten realizar múltiples conexiones fallidas en pocos segundos:
> 
> 
> 
> ```
> sudo ufw limit OpenSSH
> ```

### Paso C: Encender el Firewall


```
sudo ufw enable
```

---

## 3. Gestión de Puertos y Servicios

### Abrir y Cerrar Puertos Individuales

- **Abrir un puerto (TCP y UDP):**

    ```
    sudo ufw allow 80
    ```

- **Abrir un puerto especificando el protocolo (Recomendado):**

    ```
    sudo ufw allow 56/tcp
    ```

- **Bloquear/Denegar un puerto:**

    ```
    sudo ufw deny 56/tcp
    ```
### Rangos de Puertos

Para abrir o cerrar rangos de puertos, **es obligatorio** especificar el protocolo (`/tcp` o `/udp`).

Bash

```
sudo ufw allow 6000:6007/tcp
sudo ufw deny 300:310/tcp
```

### Puertos Múltiples en una Sola Línea

```
sudo ufw allow 80,443/tcp
```

### Usar Nombres de Servicios comunes

UFW traduce automáticamente nombres de servicios estándar a sus puertos correspondientes (gracias al archivo `/etc/services` del sistema).

```
sudo ufw allow http
sudo ufw allow https
```

---

## 4. Control de Accesos por Dirección IP y Subredes

Ideal para restringir el acceso a bases de datos o paneles de administración únicamente a equipos de confianza.

### Control Total de una IP

- **Permitir todo el tráfico desde una IP:**

    ```
    sudo ufw allow from 203.0.113.4
    ```

- **Bloquear por completo una IP atacante:**

    ```
    sudo ufw deny from 91.198.174.190
    ```
### Control de IP a un Puerto Específico

Restringe el acceso de una IP únicamente a un puerto del servidor.

```
sudo ufw allow from 203.0.113.4 to any port 22
```
### Gestión de Subredes (Rangos de IPs mediante CIDR)

- **Permitir una red de oficina entera (Ej. de `.1` a `.254`):**

    ```
    sudo ufw allow from 203.0.113.0/24
    ```

- **Permitir subred solo a un puerto específico (Ej: Base de datos MySQL):**

    ```
    sudo ufw allow from 203.0.113.0/24 to any port 3306 proto tcp
    ```

_(Bloquea el acceso a MySQL de cualquier otra IP externa)._

---

## 5. Reglas asociadas a Interfaces de Red específicas

Si tu servidor cuenta con múltiples tarjetas de red (ej. una interfaz pública `eth0` a Internet y una interna `eth1` para la red local), puedes segmentar tus reglas:

- **Permitir tráfico HTTP solo en la interfaz pública (`eth0`):**

    ```
    sudo ufw allow in on eth0 to any port 80
    ```

- **Permitir tráfico MySQL solo en la interfaz privada (`eth1`):**

    ```
    sudo ufw allow in on eth1 to any port 3306
    ```

- **Bloquear una IP sospechosa que entra por una interfaz concreta:**

    ```
    sudo ufw deny in on eth0 from 91.198.174.192
    ```

---

## 6. Mantenimiento del Firewall: Eliminar Reglas

Existen dos metodologías para borrar reglas que ya no necesitas.

### Método A: Por Número de Regla (El más rápido y recomendado)

1. **Listar las reglas con su número identificador:**

    ```
    sudo ufw status numbered
    ```

    _Salida de ejemplo:_

    ```
    Status: active
    
         To                         Action      From
         --                         ------      ----
    [ 1] 22                         ALLOW IN    Anywhere
    [ 2] 80/tcp                     ALLOW IN    Anywhere
    ```

2. **Eliminar seleccionando el número:**

    ```
    sudo ufw delete 2
    ```
### Método B: Por Sintaxis de la Regla Real

Escribe el comando de borrado seguido de la regla exacta que escribiste para crearla.

Bash

```
sudo ufw delete allow http
sudo ufw delete allow 80/tcp
sudo ufw delete allow from 203.0.113.4
```

---
## 7. Configuración de IPv6 y Logs

### Habilitar Soporte IPv6

Para que tus reglas protejan de igual manera las direcciones IPv6:

1. Abre el archivo de configuración:

    ```
    sudo nano /etc/default/ufw
    ```

2. Asegúrate de que la línea de IPv6 esté configurada en "yes":

    ```
    IPV6=yes
    ```

3. Aplica los cambios recargando el firewall:

    ```
    sudo ufw reload
    ```
### Logs/Registros de Auditoría

Si necesitas diagnosticar problemas de red o ver intentos de conexión bloqueados:

- **Activar registros:**

    ```
    sudo ufw logging on
    ```

- **Desactivar registros:**

    ```
    sudo ufw logging off
    ```