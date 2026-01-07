---

---

---
[[Ficheros y directorios]]
## Navegación por el sistema de archivos


- Lista lo archivos de un directorio.

```
ls
```

- Lista con detalles.

```
ls -l
```

- Lista archivos, incluidos los ocultos.

```
ls -a
```

- Cambia a directorio indicado.

```
cd <ruta>
```

- Sube un nivel.

```
cd ..
```

- Muestra la ruta actual.

```
pwd
```

---

## Gestión de archivos y directorios

- Copia archivos.

```
cp <origen> <destino>
```

- Mueve o renombra archivos.

```
mv <origen> <destino>
```

- Elimina archivos.

```
rm <archivo>
```

- Elimina directorio recursivamente.

```
rm -r <directorio>
```

- Crear archivo vacío.

```
touch <archivo>
```

- Crear un directorio.

```
mkdir <nombre>
```

---
## Visualización y edición

- Mostrar contenido.

```
cat <archivo>
```

- Muestra las primeras 10 líneas.

```
head <archivo>
```

- Muestra las primeras $x$ líneas.

```
head -n <x> <archivo>
```

- Muestra las últimas 10 líneas.

```
tail <archivo>
```

- Abre el editor simple.

```
nano <archivo>
```

- Eliminar saltos de línea de un fichero

```
sed -i 's/[ \t]//g' <fichero>
```


---
## Búsqueda

- Busca coincidencias dentro de un archivo.

```
grep "texto" <archivo>
```

- Búsqueda recursiva.

```
grep -r "texto" <directorio>
```

- Buscar archivos por nombre.

```
find <ruta> -name <patron>
```

---

## Gestión de procesos

- Lista procesos.

```
ps
```

- Lista completa de procesos.

```
ps aux
```

- Monitor en tiempo real

```
top
```

- Finaliza un proceso

```
kill <PID>
```

---

## Permisos y usuarios

- Cambiar permisos.

```
chmod <permisos> <archivo>
```

- Cambiar propietario.

```
chown <usuario>:<grupo> <archivo>
```

- Cambiar a superusuario.

```
sudo su
```

- Ver que usuario soy

```
whoami
```

- Crear un nuevo usuario con su directorio  y añadirle contraseña

```
sudo useradd -m <nombre_usuario>
sudo passwd <nombre_usuario>
```

---
## Red

- Ver interfaces de red.

```
ifconfig
```

o

```
ip a
```

- Probar conectividad.

```
ping <host>
```

- Ver puertos abiertos.

```
netstat -tulnp
```

- Realizar peticiones HTTP.

```
curl <URL>
```

---

## Sistema

- Uso de disco.

```
df -h
```

- Memoria RAM.

```
free -h
```

- Info del sistema.

```
uname -a
```

- Reiniciar.

```
shutdown -r now
```

- Apagar.

```
shutdown -h now
```

---

## Compresión

- Crear `.tar`.

 ```
 tar -cvf archivo.tar <directorio>
 ```

- Extraer `.tar`.

```
tar -xvf archivo.tar
```

- Crear zip

```
zip -r archivo.zip <directorio>
```

- Extraer zip

```
unzip archivo.zip
```

---

## Conexiones

- Conectarme al servicio **MySQL**.

```
mysql -u <user> -p
```


---

## Control de accesos

- Lista de los usuarios conectados por última vez.

```
lastlog
```

- Última vez que se han conectado los usuarios del sistema.

```
last
```