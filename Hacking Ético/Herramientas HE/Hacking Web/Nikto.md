
---

**Nikto** es un escáner web de código abierto que identifica vulnerabilidades de configuración y seguridad en servidores web. 

Puede detectar configuraciones erróneas comunes, versiones desactualizadas de software, y archivos, directorios o scripts potencialmente peligrosos.

---
## Flags

- `-h` -> Especifica el host objetivo
- `-p` -> Define el puerto (por defecto, 80 para HTTP).
- `-ssl` -> Fuerza la conexión HTTPS
- `-o` -> Guarda los resultados en un archivo.
- `-Tuning` -> Permite especificar el tipo de pruebas (por ejemplo, inyecciones SQL, directorios, etc.)
- `-Format` -> Define el formato del resultado (txt, html, xml, etc.).
- `-update` -> Actualiza la base de datos de Nikto.

---
## Comandos

Escaneo básico

```
nikto -h http://<Ejemplo> o <IP> 
```

Escaneo de un puerto específico

```
nikto -h http://example.com -p <puerto>
```

Forzar conexión HTTPS

```
nikto -h http://example.com -ssl
```

Actualizar la base de datos de Nikto

```
nikto -update
```

