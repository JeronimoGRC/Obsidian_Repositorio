
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
hydra -l <usuario> -P <diccionario> <protocolo>://<IP>
```

SSH:

```
hydra -l root -P rockyou.txt ssh://192.168.1.10
```

FTP:

```
hydra -l admin -P passwords.txt ftp://192.168.1.10
```

HTTP POST form:

```
hydra -L users.txt -P diccionario.txt http-post-form://192.168.1.10/login.php:username=^USER^&password=^PASS^:F=Incorrecto
```

