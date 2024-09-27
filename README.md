## Auditoria
Esta auditoria se ha realizado el `27 de sep 19:42` por Jose Daniel Moreno Lopez



### Fixed

- **PELIGRO** - Sudoers files

Este archivo tiene demasiados permisos `drwxr-xr-x`, tenemos que bajarle los permisos a `drwxr-x---`
```
- Sudoers file(s)                                           [ ENCONTRADO ]
    - Permissions for directory: /etc/sudoers.d                [ PELIGRO ]
```

Para bajar los permisos del archivo necesitamos ejecutar el siguiente comando.
```
 sudo chmod 750 /etc/sudoers.d
```
return `drwxr-x---`

![image](https://github.com/user-attachments/assets/1ef10c8d-2ccd-4ed8-8704-d1ff546e057e)

---

- **PELIGRO** - Promiscuous Interfaces

Indica que hay una o más interfaces de red configuradas en modo promiscuo.
```
- Checking promiscuous interfaces                           [ PELIGRO ]
```

Primero hemos verificado las interfaces que estan en modo promiscuo.
```
ip a
```
Ahora desactivamos la interfaces que esten en modo promiscuo.
```
sudo ip link set <interface> promisc off
```
return `Una aplicacion esta usando el modo promiscuo por lo q no puede ser deshabilitado`

En este caso es `VirtualBox` que esta usando ese modo debido a las maquinas virtuales con los modos de red de `Bridge` o `Internal Network`
