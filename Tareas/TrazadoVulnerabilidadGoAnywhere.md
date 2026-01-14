
---
## Trazado

Según el artículo de INCIBE dispuesto, la vulnerabilidad que presenta el recurso Fortra GoAnywhere MFT. 

Esta vulnerabilidad permite al ciberdelincuente dispuesto a a explotarla, crear una cuenta de administrador a través del portal de administración.

En el mismo portal de INCIBE actualizado el *25/01/2024* informa que la vulnerabilidad **CVE-2024-0204**.  

![[Pasted image 20260108175018.png]]

---
## Información sobre Vulnerabilidades

En la página de INCIBE nos da una dirección a la página de [Fortra](https://www.fortra.com/security/advisories/product-security/fi-2024-001) con la información de la vulnerabilidad.

![[Pasted image 20260108175827.png]]
- La severidad de la vulnerabilidad -> `Crítica`
- Fecha de publicación -> `22-Jan-2024`
- CVE -> `CVE-2024-0204`

![[Pasted image 20260108180018.png]]

En esta tabla podemos observar el **CWE** de la vulnerabilidad: `CWE-425`.

---
## Información sobre el riesgo o criticidad de una vulnerabilidad

En la página principal del NIST para comprobar el nivel de criticidad de la vulnerabilidad y su CVSS correspondiente.

![[Pasted image 20260109112329.png]]

En esta página para visualizar todavía mas datos sobre el CVSS:

![[Pasted image 20260109112640.png]]

Al visualizar esa información podemos observar que el riesgo que corre la tríada CIA es alto en todas las partes, Integridad, Disponibilidad, Confidencialidad.

En esta misma página podemos confirmar la ID de la debilidad:

![[Pasted image 20260109112158.png]]

---
## Información sobre las debilidades explotadas.

Basándonos en lo encontrado en la página del **NIST** sobre la debilidad CWE-425 buscaremos más información en la página de [cwe.mitre](https://cwe.mitre.org/index.html)

![[Pasted image 20260109114614.png]]

Gracias a la información podemos llegar a la conclusión de que la debilidad ocurre cuando una aplicación no aplica controles de autorización de forma adecuada en todos los recursos restringidos, permitiendo que un usuario acceda directamente a ellos si conoce o adivina la ruta.

De la información mostrada en pantalla podemos ver que la debilidad es hija o de la misma naturaleza a las siguientes:

![[Pasted image 20260109114919.png]]

También podemos observar métodos para mitigar la debilidad, basándonos en el diseño y la arquitectura de la aplicación:

![[Pasted image 20260109120821.png]]


---
## Información sobre patrones de ataque

En la página anterior teníamos una referencia de los posibles patrones de ataque que tiene esta debilidad.

![[Pasted image 20260109121604.png]]

- `CAPEC-127`
	![[Pasted image 20260109122154.png]]
	Descripción: Un adversario **solicita un recurso terminando en el nombre de un directorio** (en lugar de un archivo) para provocar que el servidor **liste el contenido del directorio**. Esto permite al atacante descubrir nombres de archivos, directorios ocultos, copias de seguridad y otros recursos no expuestos mediante enlaces habituales, facilitando ataques posteriores.
- `CAPEC-143`
	![[Pasted image 20260109122215.png]]
	Descripción: Un atacante **explora un sitio web en busca de páginas que no están enlazadas públicamente** (por ejemplo con herramientas de búsqueda de directorios) para hallar contenido no divulgado que pueda contener datos o funcionalidades sensibles.
- `CAPEC-144`
	![[Pasted image 20260109122240.png]]
	Descripción: Similar a CAPEC-143, pero dirigido a **servicios web no publicitados**. El adversario **busca servicios REST/SOAP que el sitio no ha enlazado públicamente**, los cuales pueden carecer de controles de seguridad adecuados si el administrador cree que no son accesibles.
- `CAPEC-668`
	![[Pasted image 20260109122306.png]]
	Descripción: El atacante **intercepta y manipula la negociación de claves de Bluetooth (entropía)** entre dos dispositivos durante el emparejamiento, provocando que acuerden una clave de baja entropía (débil). Esto permite descifrar o manipular fácilmente la comunicación cifrada Bluetooth.
- `CAPEC-87`
	![[Pasted image 20260109122325.png]]
	Descripción: El adversario emplea **navegación forzada** (ingreso directo de URLs o rutas no expuestas) para **acceder a secciones de una aplicación web que deberían estar restringidas**. Este patrón aprovecha la ausencia de controles de acceso efectivos para ver o interactuar con recursos que no están protegidos correctamente.

---

## Descarga del JSON

Con el registro CVE podremos observar que tiene comprendida toda la información en formato JSON, para que nosotros la podamos adquirir y acceder a su información.

![[Pasted image 20260109123840.png]]

Esto es una breve parte de la información adquirida del JSON.

![[Pasted image 20260109123906.png]]
