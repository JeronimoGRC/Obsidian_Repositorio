### **Crear claves pública/privada**

Comando para generar claves:

```
ssh-keygen -t rsa -b 4096
```

Significado:
-  `-t rsa` El tipo de clave
- `-b 4096` El tamaño en bits de la clave, en este caso es muy seguro
---

Al ejecutar el comando, **Open SSH** nos pedirá una frase de paso para revestir la clave.

```
Enter passphrase (empty for no passphrase):
```

Durante la creación se genera:
- La clave privada: `~/.ssh/id_rsa`
- La clave pública: `~/.ssh/id_rsa.pub`

---
### **Transferir claves  a otras máquinas**

La forma más común:

```
ssh-copy-id <usuario>@<ip>
```

La ip y el usuario serán los de la máquina a la que nos queremos conectar.
