Almacena las claves privadas en memoria, permitiendo que se usen sin pedir la contraseña cada vez. Sirve como servidor local de autenticación.

Gracias a esta herramienta no pedirá la frase de paso y ayudará al hecho de que podamos entrar varias veces al día sin problema a la máquina remota.
### Iniciar SSH Agent

```
eval $(ssh-agent)
```

### Añadir una clave al agente

```
ssh-add ~/.ssh/<clave privada a utilizar>
```

### Ver las claves cargadas

```
ssh-add -l
```
