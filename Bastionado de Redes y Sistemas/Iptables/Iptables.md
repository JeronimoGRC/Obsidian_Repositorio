
---

## 1. El pilar fundamental: Entender las Tablas y las Cadenas

Para dominar `iptables`, primero debes quitarte la idea de que es "un simple muro". Es un sistema de tuberías. Un paquete entra por una interfaz física, el kernel decide qué hacer con él y finalmente sale por otra interfaz (o muere en el camino).

### Las Tablas principales

1. **`FILTER` (Tabla por defecto):** Se usa para decidir si un paquete entra, sale o se descarta. Sus cadenas son:
    
    - **`INPUT`:** Tráfico destinado **al propio cortafuegos** (ej. hacerle SSH al firewall).
        
    - **`OUTPUT`:** Tráfico que **nace en el propio cortafuegos** hacia fuera.
        
    - **`FORWARD`:** Tráfico que **cruza el cortafuegos** de una red a otra (ej. de la LAN a la DMZ, o de la LAN a Internet). El cortafuegos aquí actúa como un _router_.
    
2. **`NAT` (Network Address Translation):** Se usa para modificar las direcciones IP origen o destino (enmascaramiento, reenvío de puertos). Sus cadenas estrella son:
    
    - **`PREROUTING`:** Traduce la IP de **destino** antes de tomar la decisión de enrutamiento (ej. redireccionar puertos o "DNAT").
        
    - **`POSTROUTING`:** Traduce la IP de **origen** después de que el paquete ha sido enrutado (ej. enmascarar IPs de la red interna o "SNAT/MASQUERADE").

![[Pasted image 20260510125040.png]]

---
## 2. El Camino del Paquete: Prerouting, Postrouting, Input, Output y Forward

Estas cinco palabras son las **cadenas nativas** de `iptables`. Representan los puntos exactos del sistema operativo por los que pasa un paquete de red.

Para entenderlos, imagina que tu servidor es un **centro de distribución de paquetes postales**:

### 🛃 PREROUTING (Traducir antes de decidir el rumbo)

Es la primera aduana por la que pasa **cualquier paquete que entra** al servidor a través de una tarjeta de red. Ocurre _antes_ de que el sistema operativo mire la dirección de destino y decida a dónde enviarlo (de ahí su nombre: _Pre-Enrutamiento_).

- **¿Para qué sirve?** Se usa casi exclusivamente en la tabla `nat` para hacer **DNAT** (Destination NAT / Redirección de puertos).
    
- **Ejemplo práctico:** Si quieres que cuando alguien acceda a la IP de tu firewall por el puerto `80`, el paquete se desvíe al `Servidor 1` de tu red interna. En `PREROUTING` le cambias la dirección de destino antes de que el sistema intente procesarlo localmente.


### 📥 INPUT (Destinado al propio servidor)

Si en el paso anterior el sistema operativo determina que el paquete va dirigido a la IP de tu propio servidor (por ejemplo, alguien quiere hacer SSH al firewall o cargar una web que el propio firewall aloja), el paquete entra en la cadena `INPUT`.

- **¿Para qué sirve?** Para proteger al propio servidor de ataques y gestionar quién puede comunicarse con él.
    
- **Ejemplo práctico:** Permitir que solo tu IP de administrador pueda conectar por el puerto `22` (SSH) al firewall.


### 📤 OUTPUT (Nacido en el propio servidor)

Es el camino opuesto a `INPUT`. Esta cadena evalúa **únicamente** los paquetes que son generados y enviados por el propio sistema operativo del servidor hacia el exterior.

- **¿Para qué sirve?** Controlar qué conexiones tiene permitido iniciar tu propio servidor (por ejemplo, limitar que solo pueda salir a actualizar paquetes o consultar servidores DNS de confianza).
    
- **Ejemplo práctico:** Permitir que tu servidor haga pings hacia fuera, pero impedir que navegue por webs no autorizadas.


### 🔀 FORWARD (De paso hacia otro lugar)

Esta cadena se activa cuando el paquete entra por una interfaz de red (ej. la LAN) pero va dirigido a una dirección IP que no pertenece a este servidor (ej. Internet o la DMZ). El servidor aquí actúa como un **puente o router**.

- **¿Para qué sirve?** Es el corazón de la seguridad de red en escenarios con DMZ o subredes. Controla qué máquinas pueden hablar con cuáles a través del firewall.
    
- **Ejemplo práctico:** Permitir que los ordenadores de la LAN accedan a la DMZ, pero prohibir que las máquinas de la DMZ inicien conexiones hacia la LAN para evitar el pivotaje (movimiento lateral de un atacante).


### 📦 POSTROUTING (Modificar antes de que el paquete salga a la calle)

Es la última aduana. Ocurre justo en el momento en que el paquete va a abandonar físicamente el servidor a través de una tarjeta de red, después de haber tomado todas las decisiones de enrutamiento (_Post-Enrutamiento_).

- **¿Para qué sirve?** Se usa en la tabla `nat` para hacer **SNAT** (Source NAT) o **MASQUERADE** (Enmascaramiento).
    
- **Ejemplo práctico:** Cuando un PC de tu red interna quiere navegar por Internet. Antes de que el paquete salga por la interfaz pública del firewall, en `POSTROUTING` se cambia su IP privada de origen por la IP pública del firewall para que la respuesta sepa a dónde volver.

**Las Acciones o Decisiones: Accept, Drop, Reject y Log**

Cuando un paquete coincide con los criterios de una regla (por ejemplo: "viene por TCP, al puerto 80, desde la IP X"), `iptables` debe aplicar una acción. Estas acciones se definen con el parámetro `-j` (_jump_).

### ✅ ACCEPT (Permitir el paso)

El cortafuegos abre la barrera y deja que el paquete continúe su viaje.

- **Consecuencia:** Si el paquete iba a `INPUT`, llega al servicio del servidor; si iba a `FORWARD`, cruza hacia la otra red.
    
- **Ejemplo:** `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` (Permite entrar al SSH).


### 🕳️ DROP (Descartar silenciosamente / El "Agujero Negro")

El cortafuegos destruye el paquete de forma inmediata y **no envía ninguna respuesta** al emisor.

- **Consecuencia:** Para el atacante o cliente, la conexión simplemente se queda "en espera" (_hanging_) hasta que se agota el tiempo de espera por software (_timeout_).
    
- **Ventaja de seguridad:** Es la acción ideal para interfaces públicas (Internet). No da pistas al atacante: este no sabe si tu servidor está encendido, si tiene un firewall bloqueándolo o si la IP simplemente no existe. Ralentiza drásticamente los escaneos de puertos de los hackers.

### ⛔ REJECT (Rechazar con notificación)

El cortafuegos destruye el paquete, pero **le envía un paquete de error de vuelta al emisor** (un mensaje ICMP "Port Unreachable" o un flag TCP RST).

- **Consecuencia:** Al emisor se le corta la conexión de manera instantánea. Su pantalla mostrará de inmediato un error tipo "Connection refused" (Conexión rechazada).
    
- **Cuándo usarlo:** Es muy útil en redes internas de confianza (LAN), ya que permite que las aplicaciones de los usuarios sepan de inmediato que ese puerto está cerrado, evitando esperas innecesarias de _timeout_. No se recomienda usar en interfaces de cara a Internet porque confirma al atacante que tu servidor está activo y filtrando tráfico.

### 📝 LOG (Registrar para auditoría)

Esta acción es especial: **no decide el destino del paquete**. Simplemente escribe un registro (un aviso) en el archivo de logs del sistema (`/var/log/syslog` o `/var/log/kern.log`) informando de que un paquete ha coincidido con la regla, y luego el paquete continúa bajando para ser evaluado por las siguientes reglas de la lista.

- **¿Para qué sirve?** Para monitorizar comportamientos sospechosos o depurar fallos en tus reglas de red sin llegar a bloquear el tráfico todavía.
    
- **Ejemplo práctico:** Registrar cada intento de conexión SSH que sea rechazado para saber si alguien intenta adivinar contraseñas:

    ```
    # 1. Primero registramos el intento con un prefijo descriptivo
    sudo iptables -A INPUT -p tcp --dport 22 -j LOG --log-prefix "ALERTA_SSH_RECHAZADO: "
    # 2. Inmediatamente después, destruimos el paquete
    sudo iptables -A INPUT -p tcp --dport 22 -j DROP
    ```

---

## 3. Práctica: Laboratorio de Redes y Segmentación (LAN, DMZ e Internet)

Tomando como base las topologías de tus ejercicios, vamos a configurar paso a paso la seguridad de un escenario real.

### Paso A: Establecer la base de seguridad (Lista blanca)

Bloqueamos todo el tráfico entrante y de paso por defecto para que solo pase lo que autoricemos de manera explícita.

```
# 1. Bloquear entradas al firewall y reenvíos entre interfaces
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP

# 2. Permitir que el firewall sí pueda comunicarse hacia fuera
sudo iptables -P OUTPUT ACCEPT

# 3. Permitir tráfico interno del propio firewall (localhost)
sudo iptables -A INPUT -i lo -j ACCEPT

# 4. Aplicar la regla de estado para reenvíos y tráfico propio
sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

---

### Paso B: Reglas de enrutamiento y aislamiento (Evitar pivotaje DMZ -> LAN)

La **DMZ (Zona Desmilitarizada)** suele albergar servidores web o SSH expuestos a Internet. Si un atacante compromete un servidor en la DMZ, no debe poder saltar (pivotar) a tu red interna (LAN).

#### Objetivo 1: Permitir que la LAN acceda a los servicios de la DMZ (HTTP y SSH)

Bash

```
# Permitir SSH (puerto 22) desde la LAN hacia la DMZ
sudo iptables -A FORWARD -i eth_lan -o eth_dmz -p tcp --dport 22 -j ACCEPT

# Permitir HTTP (puerto 80) desde la LAN hacia la DMZ
sudo iptables -A FORWARD -i eth_lan -o eth_dmz -p tcp --dport 80 -j ACCEPT
```

- **Explicación:** Indicamos las interfaces de entrada (`-i`) y salida (`-o`) correspondientes para asegurarnos de que el tráfico fluye en el sentido correcto.

#### Objetivo 2: Impedir radicalmente que la DMZ inicie conexiones hacia la LAN

Si aplicas las políticas por defecto anteriores (`FORWARD DROP`), este aislamiento ya ocurre de forma automática. Sin embargo, para dejarlo explícito y asegurarte de que un error en otra regla no lo rompa:

```
sudo iptables -A FORWARD -i eth_dmz -o eth_lan -j DROP
```

- **Explicación:** Cualquier paquete nuevo (`NEW`) que intente cruzar desde la interfaz de la DMZ hacia la interfaz de la LAN es destruido inmediatamente. El pivotaje queda anulado.

---
## 4. Práctica: Laboratorio de Traducción de Direcciones (NAT)

El cortafuegos no solo protege, también funciona como la puerta de enlace (Router) de tu infraestructura.

### Escenario 1: Salida a Internet (Enmascaramiento o SNAT)

Quieres que tus clientes de la red interna (Debian) salgan a Internet simulando que el tráfico proviene de la IP del propio Router/Firewall.

- **Opción A (IP externa dinámica):**

    ```
    sudo iptables -t nat -A POSTROUTING -o eth_externa -j MASQUERADE
    ```

    - **¿Cómo funciona?** Justo antes de que el paquete abandone el firewall (`POSTROUTING`), el sistema cambia la dirección IP origen del cliente local por la IP que tenga la interfaz externa (`eth_externa`) en ese instante.

- **Opción B (IP externa estática confiable):**

    ```
    sudo iptables -t nat -A POSTROUTING -o eth_externa -j SNAT --to 172.16.2.129
    ```

    - **¿Cómo funciona?** Es idéntico al enmascaramiento pero más eficiente para servidores, ya que le fuerzas estáticamente a utilizar la IP pública o externa fija proporcionada.
---
### Escenario 2: Publicar Servicios al Exterior (Redirección de Puertos o DNAT)

Quieres que cuando alguien de fuera consulte la IP del firewall en el puerto 80, acabe viendo el servidor web Apache de tu **Servidor 1** (IP interna `172.16.1.10`).

#### Paso 1: Cambiar la dirección de destino (DNAT)

```
sudo iptables -t nat -A PREROUTING -i eth_externa -p tcp --dport 80 -j DNAT --to-destination 172.16.1.10:80
```

- **Explicación:** Al usar `PREROUTING`, el paquete se modifica **nada más llegar** a la tarjeta de red externa. El cortafuegos cambia la dirección de destino original (la suya propia) por la IP interna del Servidor 1.

#### Paso 2: Permitir el paso del paquete modificado

No basta con cambiar la dirección; la cadena `FORWARD` debe permitir que cruce.

```
sudo iptables -A FORWARD -i eth_externa -o eth_interna -p tcp -d 172.16.1.10 --dport 80 -j ACCEPT
```

- **Explicación:** Esta regla autoriza explícitamente el reenvío del paquete hacia la dirección de destino final en la red interna.

---
### Escenario 3: Publicar puertos SSH alternativos

Para evitar que escaneen el puerto SSH (22) del firewall, puedes redirigir un puerto externo no estándar (ej. 2222) hacia el puerto SSH de un servidor interno (**Servidor 2** con IP `172.16.1.20`):

```
# Redirección en la tabla nat
sudo iptables -t nat -A PREROUTING -i eth_externa -p tcp --dport 2222 -j DNAT --to-destination 172.16.1.20:22

# Autorización en la cadena FORWARD
sudo iptables -A FORWARD -i eth_externa -o eth_interna -p tcp -d 172.16.1.20 --dport 22 -j ACCEPT
```

---
## 5. Control del tráfico de diagnóstico (Reglas de Ping/ICMP)

Por seguridad, es una práctica común ocultar el firewall ante pings externos (evita que escáneres automáticos detecten que el host está activo), pero manteniendo la capacidad de diagnóstico interna.

#### Objetivo: Impedir pings externos al firewall, pero permitir que el firewall y los servidores hagan ping hacia fuera.


```
# 1. Bloquear cualquier petición entrante de ping (Echo Request) desde el exterior
sudo iptables -A INPUT -i eth_externa -p icmp --icmp-type echo-request -j DROP

# 2. Permitir que los servidores internos y el propio firewall hagan ping hacia fuera
# (El tráfico de vuelta se autoriza automáticamente por la regla de estado ESTABLISHED)
sudo iptables -A FORWARD -p icmp --icmp-type echo-request -j ACCEPT
```

- **Explicación:** El protocolo ICMP maneja diferentes tipos de mensajes. El tipo `echo-request` es la pregunta ("¿estás ahí?"), y el `echo-reply` es la respuesta. Al bloquear únicamente la petición entrante de la interfaz externa, el firewall se vuelve invisible para el exterior sin perder la capacidad de hacer pings salientes.

---
## 6. Checklist de persistencia de reglas

Recuerda que todas las reglas escritas en consola se perderán si el sistema se apaga. Asegura tu trabajo:

```
# Instalar el paquete de persistencia
sudo apt update && sudo apt install iptables-persistent

# Si haces cambios nuevos, guárdalos manualmente en el archivo de configuración:
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

---
## Escenario SIN DMZ (Estructura Plana o Básica)

En un diseño sin DMZ, solo existen dos zonas de red: la **Red Externa (Internet)** y la **Red Interna (LAN)**.

Si tienes servidores que ofrecen servicios hacia el exterior (como un servidor Web Apache o un servidor SSH), estos se ubican **dentro de la misma red interna** donde trabajan los usuarios y los clientes locales.

### ¿Cómo funciona el tráfico?

- El cortafuegos recibe las peticiones desde Internet en su IP pública.
    
- Mediante reglas de **DNAT**, redirige el tráfico (puertos 80, 443, 22) directamente a las máquinas de la red interna.
    
- El cliente externo interactúa directamente con un equipo que comparte segmento de red con tus bases de datos, equipos de escritorio de la empresa y recursos críticos.

### El gran peligro: El Pivotaje (Movimiento Lateral)

Si un atacante en Internet explota una vulnerabilidad en tu servidor web (por ejemplo, a través de una inyección de código) y toma el control de esa máquina:

- Al estar el servidor en la misma LAN, el atacante **ya está dentro de tu red segura**.
    
- Puede realizar un escaneo local de manera directa, acceder a otras máquinas de la red interna, descifrar credenciales o infectar con malware los equipos de los empleados sin que el cortafuegos perimetral pueda interceptar ese tráfico (ya que el tráfico local no cruza el firewall).

---
## Escenario CON DMZ (Estructura Segmentada de 3 Patas)

![[Pasted image 20260510125022.png]]

La **DMZ (Zona Desmilitarizada)** es una red intermedia, aislada y vigilada por el cortafuegos. En este diseño, dividimos la infraestructura en tres zonas claramente separadas:

1. **Internet (Insegura):** El mundo exterior.
    
2. **DMZ (Semi-segura):** Contiene únicamente los servidores que _tienen_ que ser accesibles desde fuera (Web, Correo, DNS, SSH público).
    
3. **LAN (Segura):** Equipos de usuarios, servidores de base de datos internos y controladores de dominio que _nunca_ deben exponerse a Internet.

![[Pasted image 20260510124959.png]]

### ¿Cómo funciona el tráfico?

El cortafuegos actúa como un guardia de tráfico estricto mediante la cadena `FORWARD`:

- **Desde Internet a la DMZ:** Permitido bajo control estricto (solo puertos específicos como el 80 o 443).
    
- **Desde la LAN a la DMZ:** Permitido (los empleados necesitan entrar al servidor web interno o administrarlo por SSH).
    
- **Desde la DMZ a la LAN:** **ESTRICTAMENTE BLOQUEADO** (`DROP`).

### La Garantía de Seguridad ante un ataque

Si el servidor web ubicado en la DMZ es comprometido por un atacante:

- El hacker tiene el control del servidor, pero está atrapado en la DMZ.
    
- Si intenta realizar un escaneo de puertos o iniciar una conexión SSH hacia la red interna (LAN), se topa de frente con la regla restrictiva del firewall:

    ```
    sudo iptables -A FORWARD -i eth_dmz -o eth_lan -j DROP
    ```

- El atacante no puede "pivotar" (moverse lateralmente) hacia las bases de datos de la empresa ni comprometer los equipos de los usuarios locales. Su alcance queda limitado únicamente a la zona desmilitarizada.

---
## Tabla Comparativa Resumida

|**Característica**|**Sin DMZ**|**Con DMZ**|
|---|---|---|
|**Zonas de Red**|2 zonas (Interna y Externa).|3 zonas (Interna, Externa y DMZ).|
|**Ubicación de Servidores Públicos**|Dentro de la LAN (junto a los usuarios).|Aislados en la DMZ.|
|**Seguridad de la LAN**|**Baja**. Si un servidor cae, toda la red interna está en peligro.|**Alta**. La LAN queda protegida aunque un servidor web sea comprometido.|
|**Complejidad de Configuración**|Muy baja (reglas sencillas de DNAT directo).|Media-Alta (múltiples interfaces de red y control estricto del flujo entre ellas).|
|**Flujo de Pivotaje**|Directo y sin restricciones físicas de red.|Bloqueado por el Firewall (`FORWARD -i eth_dmz -o eth_lan -j DROP`).|
