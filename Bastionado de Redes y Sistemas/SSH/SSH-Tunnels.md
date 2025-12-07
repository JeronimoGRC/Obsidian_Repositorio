Sirve para crear una **conexión segura** entre un ordenador y otro de manera cifrada

---
## Comandos 
---
### Local Port Forwarding 

Local Port Forwarding hace que **un puerto de TU máquina local** actúe como un túnel hacia **un puerto de la máquina remota** a través de SSH..

```
ssh -L localhost:5000:localhost:80 root@<IP> -fN
```

Explicación:
- `localhost:5000` -> puerto local donde escucha mi máquina.
- `localhost:80` -> puerto de la máquina remota al que se conectará el servicio SSH remoto.
- `root@<IP>` -> máquina remota a la que te conectas.

Todo el tráfico que envíes a `localhost:5000` en tu máquina **viaja por el túnel SSH** y llega al **puerto 80 de la máquina remota**

---
### Remote  Port Forwarding

Abre un puerto en la **máquina remota**, y cualquier conexión a ese puerto se redirige hacia **un puerto en tu máquina local** a través del túnel SSH.


```
ssh -R 0.0.0.0:7777:localhost:80 <usuario>@<IP> -fN
```

Explicación:
- En la máquina remota, se abre el puerto `7777`.
- Cualquiera que se conecta a `<maquina_remota>:7777` vieja por el túnel, llega al puerto `80` de tu máquina local.
- `0.0.0.0` el puerto remoto queda accesible desde cualquier IP

En el servidor remoto habilitar estas opciones en el fichero `/etc/ssh/sshd_config`:

```
GatewayPorts yes
AllowTcpForwarding yes
```

### Entendimiento final:

- **Local Port Forwarding**: “Yo (cliente) abro un puerto local que apunta a un puerto remoto.”
- **Remote Port Forwarding**: “La máquina remota abre un puerto remoto que apunta a un puerto local mío.”

---
### Ver túneles activos

```
ps aux | grep ssh
```

---
### Borrar túneles

Borrar un túnel:

```
kill id
```

Para borrar todos los túneles:

```
pkill ssh
```


