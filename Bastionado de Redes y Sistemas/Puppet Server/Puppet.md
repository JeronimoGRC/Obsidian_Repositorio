---

---

---

## ¿Qué es?

Es una herramienta de gestión de sistemas para centralizar y automatizar la configuración de los equipos. Funciona con un servidor que provee y ordena a los agentes puppet en el resto de equipos que se instalen dependencias, creen 

Ejemplo práctico de configuración:

![[Pasted image 20260219101033.png]]

La mejor forma de usar Puppet es mezclar el uso con [[PSSH]] y ejecutar el comando:

```
puppet agent -t
```

Para que así se ejecute a la vez la actualización de dependencias, borrados y creaciones en los equipos.  

Para ser exactos la carpeta en la que trabajamos es `/etc/puppetlabs/code/enviroments/production/modules`, en esa carpeta crearemos los módulos o tareas que importaremos.

También para tener más orden ejecutaremos el siguiente comando:

```
mkdir -p tarea_ejemplo/{manifests,files}
```

En la carpeta *manifests* estará el archivo `init.pp`  en el que configuraremos la tarea que tendrán que realizar nuestros equipos.

Esta es la ubicación del archivo donde adjuntaremos las tareas creadas:

`/etc/puppetlabs/code/environments/production/manifests/site.pp`

---
## Recursos:

### `group`:

Gestiona la existencia y configuración de grupos en el sistema operativo. En la mayoría de las plataformas, su función principal es crear o eliminar grupos.

Ejemplos:

Asegurarse de que esté presente el grupo *prueba*:

```
group { 'prueba':
	ensure => present,
}
```

Asegurarse de que NO esté el grupo *prueba*:

```
group { 'prueba':
	ensure => absent,
}
```

Asegurarse que varios grupos estén presentes:

```
group { ['prueba1','prueba2','prueba3']:
	ensure => present
}
```

Asegurarse que varios grupos NO estén presentes:

```
group { ['prueba1','prueba2','prueba3']:
	ensure => absent,
}
```

### `user`:

Creación de usuarios:

```
user { 'usuario1':
	ensure => present,
	comment => 'Este es el usuario usuario1',
	home => '/home/usuario1',
	shell => '/bin/bash',
	groups => ['grupo1','grupo2'],
	passwords => '##Contraseña generalmente hasheada##',
	managehome => true,
}
```

Borrar usuarios:

```
user { 'usuario1':
	ensure => absent,
	managehome => true,
}
```

### `file`:

Creación de fichero:

```
file { '/home/usuario1/fichero.txt':
	ensure => file,
	owner => 'usuario1',
	group => 'prueba1',
	mode => '664', # Permisos
	content => "Este fichero se ha generado mediante una tarea Puppet.",
	require => User['usuario1'], #Obliga crear el usuario antes que el fichero
}
```

Asegurarse que NO existe un fichero:

```
file { '/home/usuario1/fichero.txt':
	ensure => absent,
}
```

### `package`:

Instalación de paquetes:

```
package { 'iptables':
	ensure => installed,
}
```

Asegurarse de que no existe el programa: 

```
package { 'iptables':
	ensure => absent,
}
```

### `file`:

- **ensure**: `file`, `directory` o `present`.
- **source**: Indica la ruta del archivo original en el servidor (ej: `puppet:///modules/mi_modulo/archivo.conf`).
- **content**: Permite escribir texto directamente en el archivo.

```
file { '/var/www/html/index.html':
  ensure => file,
  source => 'puppet:///modules/apache_config/index.html',
}
```

### `service`:

Controla si un servicio está corriendo o parado.

- **ensure**: `running` o `stopped`.
- **enable**: `true` o `false` (si arranca al iniciar el sistema).

```
service { 'apache2':
  ensure => running,
  enable => true,
}
```

### `exec`:

Se usa cuando no existe un recurso específico de Puppet para lo que queremos hacer. **Cuidado:** Siempre debe tener una condición para no ejecutarse en cada ciclo.

- **command**: El comando exacto a ejecutar.
- **path**: Las rutas donde buscar el ejecutable (ej: `['/usr/bin', '/usr/sbin']`).
- **creates**: No ejecuta el comando si el archivo especificado ya existe (ideal para escaneos).
- **onlyif / unless**: Ejecuta el comando solo si una condición se cumple (o no).

```
exec { 'ejecutar_escaneo':
  command => 'nmap -sP 192.168.1.0/24 > /root/escaneo_de_red',
  path    => ['/usr/bin'],
  creates => '/root/escaneo_de_red',
}
```

#### **Metaparámetros de Relación**:

1. `require` (Dependencia)

Dice: "Yo necesito que **X** esté listo antes de empezar". Se pone en el recurso que **depende** de otro.

- **Uso:** `require => Package['apache2']` (Nota: El tipo de recurso va en mayúscula cuando se referencia). 

2. `before` (Ordenación)

Dice: "Yo voy primero, **X** va después". Se pone en el recurso que debe ejecutarse **antes**.

- **Uso:** `before => Service['apache2']`

