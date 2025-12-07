
---

## ¿Qué es?

**Netcat** es una herramienta que conecta dos máquinas a través de puertos remotos, también puede escuchar en un puerto y esperar a que alguien se conecte.

Además puedes transferir archivos sin necesidad de [[FTP]] o la herramienta de SSH [[SCP]].

---

## Comandos

### Conectarse a un puerto (modo cliente):

```
nc <IP> <puerto>
```

### Activar el modo de escucha en un puerto

```
nc -l <puerto>
```

### Enviar mensaje simple

Escucha:

```
nc -l <puerto>
```

Mensaje:

```
echo "Hola" | nc <IP> <puerto>
```

### Transferencia de archivos

Recibir archivos:

```
nc -l <puerto> > <archivo>
```

Enviar archivo:

```
nc <IP> <puerto> < <archivo>
```