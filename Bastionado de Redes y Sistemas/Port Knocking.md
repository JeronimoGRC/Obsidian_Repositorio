
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

Cambiar las command por el que nos ha puesto.

![[Pasted image 20251209131704.png]]

La secuencia de puertos serán los puertos que utilizaremos para abrir o cerrar el puerto y poder acceder.

---

## Apertura y cierre del puerto desde cliente


```
$ knock -v <IP> <combinación de puertos>
```

Cuando abramos el puerto y hagamos el escaneo aparecerá `open`, pero si todavía no lo hemos abierto o en su defecto está cerrado aparecerá `filtered`.
