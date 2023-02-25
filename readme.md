# Introducción
Samba es el conjunto estándar de programas de interoperabilidad de Windows para Linux y Unix.

Samba es software libre licenciado bajo la Licencia Pública General de GNU, el proyecto Samba es miembro de Software Freedom Conservancy.

Desde 1992, Samba ha proporcionado servicios de archivo e impresión seguros, estables y rápidos para todos los clientes que utilizan el protocolo SMB/CIFS, como todas las versiones de DOS y Windows, OS/2, Linux y muchas otras.

Samba es un componente importante para integrar sin problemas servidores y escritorios Linux/Unix en entornos de Active Directory. Puede funcionar tanto como controlador de dominio como como miembro de dominio regular.

# Instalación

Para realizar la instalación ejecutar el siguiente comando:
```
$ sudo apt-get update
$ sudo apt-get install samba -y
```

Para comprobar la versión ejecutar el siguiente comando:
```
$ smbd --version
```


# Configuración
El archivo de configuración es único y está ubicado en `/etc/samba/smb.conf`.

Algunas configuraciones globales que se pueden añadir son las siguientes:
```
[global]
workgroup = WORKGROUP
server string = Samba Server
netbios name = ubuntu
security = user
map to guest = bad user
dns proxy = no
```

## Compartir directorio local

Para configurar una carpeta compartida en modo escritura sin seguridad permitiendo el acceso invitado añadir la siguiente configuración:
```
[Public]
path = /var/samba/shares/compartida
browsable =yes
writable = yes
guest ok = yes
read only = no
create mask = 644
```

Para aplicar los cambios hay que reiniciar el servicio con el siguiente comando:
```
$ sudo service smbd restart
```

Para cambiar los permisos del directorio y hacerlo de escritura ejecutar el siguiente comando:
```
$ sudo chmod 777 /var/samba/shares/compartida
```

Para comprobar los permisos se puede utilizar el comando `ls` aplicando el parámetro `-l` de la siguiente forma:
```
$ ls -l
```

## Compartir directorio local `/media`

Para configurar una carpeta compartida en modo escritura sin seguridad permitiendo el acceso invitado añadir la misma configuración 
que el apartado anterior de (Compartir directorio local), pero con la `path` correspondiente y utilizando la propiedad `force user`
para que funcione correctamnente de la siguiente manera:
```
path = /media/[username]/cine
force user = [username]
```

# Enlaces
* Wiki: https://wiki.samba.org/index.php/User_Documentation
* Sitio oficial: https://www.samba.org/samba/
