
---
## ¿Qué es?

Es la carga de ficheros en el servidor a través de un punto de acceso, ej: subir una imagen con código corrupto o subir cualquier archivo si no hay saneamiento.

Que exista esta vulnerabilidad facilita la ejecución remota de comandos por parte del atacante, ataques en el lado del cliente.

---

## Formas de ataques

**Nivel Bajo**:

Este nivel no tiene defensas se puede subir cualquier fichero en este caso podríamos subir un `.php` que liste el contenido de la carpeta en la que está. 

![[Pasted image 20260131114350.png]]

También podríamos ver el contenido de ficheros sensibles o hacer una shell inversa.

**Nivel Medio**:

En este nivel ya se hace un filtrado, solo permite una imagen *jpg/png* 

Para este caso utilizaremos la herramienta [[Burp Suite]] para interceptar la petición que vamos a mandar al servidor de la máquina víctima y modificar el **Content-Type**.

![[Pasted image 20260131115651.png]]

**Nivel Alto**

Esta opción tiene múltiple validación, el Content-Type exige una imagen en un formato específico, la firma del archivo y el tamaño.

Para saltarnos las barreras impuestas por el código modificaremos una imagen y le pondremos al final de la imagen un código de php.

![[Pasted image 20260131120110.png]]

También podríamos añadir principio `GIF89a;` para que entienda que es una imagen .jpeg sin necesidad de que haya contenido.

Para este caso vamos a realizar una shell inversa, el codigo php es el siguiente:

![[Pasted image 20260131120730.png]]

Esto al subirlo nos permitirá hacer una ejecución de comandos a través de la variable **cmd** mezclando los conceptos aprendidos en el apartado de **LFI** en el [[File Inclusion]]:

![[Pasted image 20260131120853.png]]
