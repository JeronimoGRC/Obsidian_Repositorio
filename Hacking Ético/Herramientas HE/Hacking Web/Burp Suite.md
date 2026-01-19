
---

**Burp Suite** es una herramienta usada en **seguridad informática** para **interceptar, analizar y modificar** las peticiones HTTP/HTTPS entre un navegador y un servidor web.

Burp Suite actúa como un **proxy local** que se localiza entre el cliente y el servidor para obtener control total sobre las peticiones que se lanzan.

---
## Configuración del Proxy 

En el navegador en opciones se va ***Proxy -> Options*** .

```
HTTP Proxy : 127.0.0.1

Puerto : 8080
```

---

## Certificado para HTTPS

Para interceptar tráfico HTTPS sin errores, Burp actúa como CA (Authority).  
Pasos básicos:

1. En el navegador acceder a:

```
http://burp
```

2. Descargar el certificado.

3. Importarlo como autoridad de confianza (CA).

4. Con esto, el navegador confiará en el cifrado "interceptado" por Burp.

Esto evita advertencias de “conexión insegura”.

---
## Opciones del Burpsuite

Las más destacadas:

- **Proxy** -> Intercepta y modifica el tráfico HTTP/HTTPS
- **Repeater** -> Reenviar peticiones manualmente para probar comportamientos del servidor.
- **Intruder** -> Automatizar ataques basados en fuerza bruta, fuzzing o inyección de parámetros.

---

# Interceptación de peticiones

Para observar y manipular tráfico en vivo:

1. En Burp ir a:
	**Proxy → Intercept**
2. Activar:
	 **Intercept is ON**
3. Navegar desde el navegador.
    
4. Las peticiones al servidor pasarán a Burp y aparecerán en la ventana central.

Controles principales:

- **Forward** → Envía la petición al servidor.
- **Drop** → La descarta, el servidor nunca la recibe.
- **Intercept is OFF** → Desactiva la captura.
- **Intercept Client Responses** (opcional) → Captura también respuestas.