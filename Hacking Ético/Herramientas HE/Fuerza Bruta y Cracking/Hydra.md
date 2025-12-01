
Es una herramienta de [[Fuerza Bruta y Cracking de Contraseñas]] y ataques de diccionario.

---

### Flags

- `-l` -> Un solo usuario.
- `-L` -> Lista de usuarios.
- `-p` -> Una sola contraseña.
- `-P` -> Diccionario de contraseñas.
- `-V` -> verbose.
- `-f` -> Parar al encontrar un login válido.
- `-s` -> Indicar el puerto.

---
### Ejemplos:

Comando básico:

```
hydra -l <usuario> -P <fichero de contraseñas> <protocolo>://<IP> 
```

SSH:

```
hydra -l <usuario> -P <fichero de contraseñas> ssh://<IP>
```

FTP:

```
hydra -l <usuario> -P <fichero de contraseñas> ftp://<IP>
```

HTTP POST form:

```
hydra -l <usuario> -P  <fichero de contraseñas> http-post-form://[IP]/login.php:username=^USER^&password=^PASS^:F=Incorrecto
```

MySQL:

```
hydra -l <usuario> -P <fichero de contraseñas> mysql://<IP>
```