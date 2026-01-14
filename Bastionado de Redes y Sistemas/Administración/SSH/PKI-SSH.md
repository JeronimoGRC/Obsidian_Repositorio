### **Crear claves pública/privada**

Comandos para generar claves:

```
ssh-keygen -t rsa -b 4096
```

Significado:
-  `-t rsa` El tipo de clave
- `-b 4096` El tamaño en bits de la clave, en este caso es muy seguro

U otra opción

```
ssh-keygen -t ecdsa -b 521
```

Significado:
-  `-t ecdsa` El tipo de clave
- `-b 521` El tamaño en bits de la clave
---

Al ejecutar el comando, **Open SSH** nos pedirá una frase de paso para revestir la clave.

```
Enter passphrase (empty for no passphrase):
```

Durante la creación se genera:
- La clave privada: `~/.ssh/id_<tipo de clave>`
- La clave pública: `~/.ssh/id_<tipo de clave>`.pub`

---
### **Transferir claves  a otras máquinas**

La forma más común:

```
ssh-copy-id -i <ruta clave pública usuario> <usuario>@<ip>
```

La ip y el usuario serán los de la máquina a la que nos queremos conectar.

- `-i` -> Para indicar la clave pública 
