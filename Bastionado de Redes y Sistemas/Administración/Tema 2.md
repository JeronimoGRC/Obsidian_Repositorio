
---

## Fundamentos: Criptografía y el Modelo AAA.

La criptografía se describe como el arte o ciencia de cifrar y descifrar información para asegurar que sólo los destinatarios legítimos, poseedores de los medios de descifrado, puedan leer los mensajes. 

Las fuentes advierten que la combinación tradicional de usuario y contraseña ya no es suficiente ante las amenazas actuales, lo que hace necesario adoptar modelos más robustos como el AAA. Este modelo gestiona la seguridad a través de tres pilares: Autenticación (probar la identidad), Autorización (conceder privilegios específicos tras la autenticación) y Contabilidad (rastrear el consumo de recursos para administración o facturación). Además, se suele añadir una cuarta "A" para la Auditoría, que consiste en evaluar evidencias para asegurar que el sistema mantiene la integridad de los datos y cumple con las regulaciones.
## Debilidades y Fortalecimiento de las contraseñas.

La seguridad depende críticamente de la elección y confidencialidad de las claves, las cuales suelen ser blanco de ataques de fuerza bruta (probar todas las combinaciones), de diccionario (usar palabras comunes) o de análisis estadístico (patrones típicos).

Para mitigar esto, las fuentes dictan normas estrictas:

- No usar palabras de diccionario, números exclusivos o información personal.

- Evitar la repetición de caracteres y no permitir que las aplicaciones "recuerden" las claves.  
  
- Implementar OTP (One-Time Password) para que la clave sea de un solo uso y SSO (Single Sign-On) para acceder a múltiples sistemas con una única validación. 
  
- Criptografía Simétrica, Asimétrica y Firma Digital.

- Simétrica: Utiliza la misma clave para cifrar y descifrar, lo que presenta retos en la distribución segura de la clave y en su escalabilidad para grandes grupos. Ejemplos incluyen AES, DES y Blowfish.
  
- Asimétrica (PKI): Utiliza un par de claves: una pública (conocida por todos) y una privada (custodiada por el dueño). Se apoya en funciones hash, que son resúmenes de un solo sentido que sirven para almacenar claves de forma segura o validar la integridad de los archivos, ya que cualquier cambio en el dato original altera el hash resultante.

- Firma Digital: Basada en estos sistemas, garantiza la autenticidad del origen, la integridad de la información y el no repudio, impidiendo que el emisor niegue haber enviado el mensaje.

---
## Autenticación Biométrica y Multifactor (MFA).

La biometría utiliza rasgos únicos como la huella dactilar, el iris o el reconocimiento facial. Sin embargo, las fuentes advierten sobre debilidades como la calidad de los dispositivos de captura o enfermedades que puedan alterar los rasgos (ej. laringitis para la voz). Para maximizar la seguridad, se recomienda la autenticación multi-factor, que combina al menos dos de estos elementos:

  

1. Conocimiento: Algo que el usuario sabe (contraseña).

2. Posesión: Algo que el usuario tiene (token, tarjeta, Yubikey).

3. Inherencia: Algo que el usuario es (rasgos biométricos). Ejemplos avanzados incluyen Google Authenticator (códigos basados en tiempo), Latch (un "pestillo" digital que bloquea cuentas y alerta de intentos de acceso) y certificados digitales de usuario.

4. Control de Acceso y Bastionado de Sistemas.
  

El robustecimiento de los sistemas se divide en dos ámbitos según las fuentes:
  

• **Administración Remota (SSH)**: Se debe configurar el servicio para usar solo la versión 2, limitar los usuarios permitidos, deshabilitar el acceso al usuario root y prohibir contraseñas vacías. Es preferible el acceso por certificado, donde el servidor solo responde si se presenta una clave pública previamente autorizada.

  

• **Administración Local**: Incluye revisar las cuentas de sistema en /etc/passwd para que no tengan acceso a la shell y usar herramientas como John the Ripper para detectar claves débiles. El uso de PAM (Pluggable Authentication Module) es vital para forzar políticas de complejidad (mediante pam_cracklib), bloquear cuentas tras varios intentos fallidos (pam_tally2) y obligar al uso de algoritmos de hash fuertes como SHA-512.

---

## Protocolos de Red: TACACS+, RADIUS y KERBEROS


**TACACS+**: Protocolo privativo de Cisco que utiliza TCP (puerto 49) y encripta la totalidad del paquete de datos para las funciones de AAA.

**RADIUS**: Estándar para acceso a redes (como WiFi) que usa UDP (puerto 1812). Funciona enviando las credenciales desde un dispositivo de acceso a un servidor central para su validación.

**KERBEROS**: Protocolo de autenticación mutua basado en un tercero de confianza llamado KDC. El proceso utiliza "tickets": el usuario obtiene un TGT (Ticket de concesión de tickets) y luego solicita vales específicos para servicios, permitiendo un inicio de sesión único (SSO) rápido y seguro. Su principal limitación es que requiere una sincronización de tiempo muy estricta y la disponibilidad constante del KDC

