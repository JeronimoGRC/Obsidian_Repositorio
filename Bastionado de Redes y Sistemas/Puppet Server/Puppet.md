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

