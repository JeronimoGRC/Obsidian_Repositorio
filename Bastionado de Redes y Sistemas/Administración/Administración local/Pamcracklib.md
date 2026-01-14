
---

Es la política de contraseñas que estableceremos a los usuarios.


Instalación

```
apt install libpam-cracklib
```

---
### Archivos implicados

- `/etc/pam.d/common-password` -> Donde se configura las reglas a aplicar sobre las contraseñas.
- `/etc/login.defs` -> Donde se define el periodo de expiración de las contraseñas.

---

### Configuración 

Editamos el archivo `/etc/pam.d/common-password` para configurar la complejidad de las contraseñas.

Agregamos los siguientes valores a la línea que contiene **_pam_cracklib.so_** con el siguiente ejemplo:

```
password        requisite   pam_cracklib.so retry=3 minlen=10 difok=3 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1
```
#### Flags

- `retry=` -> Número de intentos.
- `minlen=` -> Longitud mínima.
- `difok=` -> Caracteres distintos a la anterior contraseña.
- `dcredit=` -> Dígitos numéricos.
- `ucredit=` -> Letra mayúscula.
- `lcredit=` -> Letra minúscula.
- `ocredit=` -> Símbolo especial.

El siguiente paso es editar el archivo `/etc/login.defs` para configurar los parámetros temporales de caducidad y advertencia.

```
PASS_MAX_DAYS 90 
PASS_MIN_DAYS 0 
PASS_WARN_AGE 15
```

- PASS_MAX_DAYS -> Número máximo de días  que se puede usar una contraseña.
- PASS_MIN_DAYS -> Número mínimo de días  que se puede usar una contraseña.
- PASS_WARN_AGE -> Número de días de advertencia antes de que caduque una contraseña.

---
## PWGen

### ¿Qué es?

Es un generador de contraseñas aleatorias desde la terminal.

### Instalación

```
sudo apt-get install pwgen
```

### Sintaxis de los comandos

```
pwgen [opciones] longitud cantidad
```

### Flags

- `-s` → Modo seguro (totalmente aleatorio, recomendado)
- `-y` → Incluye símbolos
- `-n` → Incluye números
- `-c` → Incluye mayúsculas
- `-A` → Evita caracteres ambiguos (0, O, l, 1…)

### Contraseña fuerte compatible con PAM

```
pwgen -s -y -n -c 12 1
```

### Evitar símbolos problemáticos

```
pwgen -s -y -r "'\"\\`" 12 5
```