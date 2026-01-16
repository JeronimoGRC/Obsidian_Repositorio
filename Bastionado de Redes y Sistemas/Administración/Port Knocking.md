
---
## ¿Qué es?

Es una técnica de control de acceso a un puerto permitiendo únicamente a usuarios legítimos el acceso al servicio que se ejecuta en un servidor.

---
## Instalación de la herramienta.

```
sudo apt install knockd
```

---
## Ubicación y manejo de ficheros relevantes.

- Fichero de configuración -> ``/etc/knockd.conf`
- Línea de trabajo del knockd -> `/etc/default/knokd` , en este fichero encontraremos dos líneas de código importantes que si no están bien configuradas **no funcionará el servicio**.

1. `START_KNOCKD=0`. Descoméntalo y establece el valor en `1`.
2. `KNOCKD_OPTS=”-i eth1”` Descoméntalo y reemplaza el valor predeterminado `eth1` valor con la interfaz de red activa de tu sistema. (Hacer un `ip a` para ver la interfaz de red correcta)

---
## Activación y uso del servicio.

```
systemctl start knockd #Iniciar servicio

systemctl enable knockd #Habilitar servicio

systemctl status knockd #Conocer el estado del servicio

systemctl restart knockd #Reiniciar servicio
```

--- 
## Configuración

Ejemplo visual:

Apertura:

```
/sbin/iptables -D INPUT -p tcp --dport número_puerto -j DROP
```

Cierre:

```
/sbin/iptables -A INPUT -p tcp --dport número_puerto -j DROP
```

La secuencia de puertos serán los puertos que utilizaremos para abrir o cerrar el puerto y poder acceder.

Ejemplo:

```
sequence = 9000,8000,7000
```

**Ejemplo:**

*`[open...]`*:

```
[openSSH]
		sequence = 7000,8000,9000
		seq_timeout = 5
		command = iptables -D INPUT -p tcp --dport <numero_puerto> -j DROP
		tcpflags = syn
```

*`[close...]`*:

```
[closeSSH]
		sequence = 8100,8200,8300
		seq_timeout = 5
		command = iptables -A INPUT -p tcp --dport <numero_puerto> -j DROP
		tcpflags = syn
```

Con estos ejemplos se sobreentiende que el servicio tiene el puerto `filtered`:

```
/sbin/iptables -A INPUT -p tcp --dport <puerto> -j DROP
```

---

## Apertura y cierre del puerto desde cliente


```
$ knock -v <IP> <combinación de puertos>
```

Cuando abramos el puerto y hagamos el escaneo aparecerá `open`, pero si todavía no lo hemos abierto o en su defecto está cerrado aparecerá `filtered`.

---

## Configuración para la interfaz de red

- Fichero de configuración -> `/etc/default/knockd`.

En el caso de que se tenga que cambiar la configuración será la siguiente fila:

```
# command line options
#KNOCKD_OPTS="-i eth01"
```

Y cambiar la interfaz de red por la que funciona en el equipo.