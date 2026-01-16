
---
## ¿Qué es?

Es una utilidad gratuita, que permite analizar la seguridad de un sitio creado con WordPress de forma externa. Los resultados y reportes que obtenemos nos servirán para analizar la seguridad de nuestro sitio creado con **WordPress**.

---
## Flags

La opción —**enumerate** pone a nuestra disposición el tipo de vulnerabilidades que queremos analizar:

- ``vp`` Plugins vulnerables.
- ``ap`` Todos los plugins.
- ``p`` Plugins populares.
- ``vt`` Temas vulnerables.
- ``at`` Todos los temas.
- ``t`` Temas populares.
- ``tt`` Timthumbs.
- ``cb`` Config backups.
- ``dbe`` Db exports.
- ``u`` Cuentas de usuario.

---

## Configuración

Para incluir tu token de usuario y que pueda hacer las búsquedas pertinentes debemos seguir la siguiente configuración:

```
nano ~/.wpscan/scan.yml
```

Y en la parte inferior del fichero incluimos el siguiente bloque de código:

```
cli_options:
    api_token: TU_TOKEN
```

---
## Comandos

Escaneo básico:

```
wpscan --url https://<URL> --api-token <mi_token>
```

Ejemplo escaneo con `--enumerate`:

```
wpscan --url http://<> --enumerate u
```

Si queremos enumerar cualquier otra cosa, solo tendremos que cambiar la flag.

Y si queremos incluir una lista de usuarios y claves a comprobar vamos a realizar lo siguiente:

```
wpscan --url http://www.domain.com -U usernames.list -P passwords.list
```

Podemos especificar el tipo de ataque `(wp-login, xmlrpc, xmlrpc-multicall)`, la url del login si esta ha sido cambiada, etc…

```
wpscan --url http://www.domain.com -U usernames.list -P passwords.list --login-uri http://www.domain.com/wp-login.php
```

Algunos sitios tienen configuraciones defensivas.

Evitar TLS verify:

```
wpscan --url https://target.tld --disable-tls-checks`
```

Cambiar user-agent:

```
wpscan --url https://target.tld --random-user-agent`
```

Ajustar timeout:

```
wpscan --url https://target.tld --connect-timeout 15
```
