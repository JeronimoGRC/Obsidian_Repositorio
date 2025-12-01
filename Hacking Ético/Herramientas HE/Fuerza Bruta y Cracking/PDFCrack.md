
---

En el caso de que necesitamos hallar las contraseñas de un documento PDF haremos el siguiente comando:

```
pdfcrack -f <ruta/del/ficheroPDF o el fichero> -w <diccionario o ruta/diccionario>
```

Una vez ejecutado el comando obtendremos una salida parecida a esta:

```
PDF version 1.5
Security Handler: Standard
V: 2
R: 3
P: -1028
Length: 128
Encrypted Metadata: True
FileID: 52ba29cdb31fe1d0b382709378a087fb
U: 244ce867cf004cb27afd2e8ad2e998e300000000000000000000000000000000
O: 89f34f437d07a63257444471b5c02e60c0986ca326b4f3d77c5865f8b6701dfc
found user-password: '<password>'

```

Donde viene `<password>` es donde aparece la contraseña del fichero PDF.


