
---
## ¿Qué es?

**SCP** es una herramienta que pertenece al protocolo [[SSH]], es utilizado en la transferencia de archivos y por consiguiente va cifrado todo lo que haga.

---

### Comandos

Local <--> Remoto:

```
scp <dirección de archivo local> <usuario>@<IP>:<dirección destino>
```

Remoto <--> Local:

```
scp <usuario>@<IP>:<dirección archivo remoto> .
```

- `.` -> Directorio actual.

