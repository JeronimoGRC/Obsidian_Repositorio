
---

## ¿Qué es?

Es una herramienta que se utiliza para el escaneo de puertos y servicios de un dispositivo. Identifica las versiones y si es vulnerable a ciertos *CVE*. 

---

## Flags

- `-p-` -> Escaneo todos los puertos.
- `-p` -> Especifica el puerto.
- `-sS` -> Escaneo TCP/SYN, es relativamente sigiloso y rápido ya que nunca completa las conexiones.
- `-sU` -> Escaneo UDP
- `-sV` -> Detecta las versiones de los servicios escaneados.
- `-O` -> Detección del OS.
- `-A` -> Escaneo agresivo incluye: 
	- Detección de versiones
	- Sistema operativo
	- Escaneo de scripts
	- Traceroute

- `-sn` -> Sirve para conocer los host de una red.
- `-n` -> No hace la resolución de DNS.
- `-Pn` ->Evita que los firewalls bloqueen las solicitudes de ping.
- `-sC` -> Ejecución de scripts predeterminadas.
- `--scripts=[nombre del script]` -> Ejecución de un script concreto.
	- Para ver los scripts -> `/usr/share/nmap/scripts`

---

## Ejemplos

Escaneo 

```
nmap -sV -p- [IP objetivo]
```

Ejecución de todos los *scripts*

```
nmap --script * [IP objetivo]
```

Ejecución muy silenciosa

```
nmap -sS -n -Pn -sV -p- [IP objetivo]
```

Ejecución de escaneo de red

```
nmap -sP <red>/<interfaz de red>
```
