![Samba_logo_2010 svg](https://user-images.githubusercontent.com/28895475/221364397-12def956-ee2b-4b79-91d8-203a1c077c4a.png)

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

Puedes verificar la ruta del fichero de configuración ejecutando el comando `smbd -b | grep smb.conf`.

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

## Verificar archivo smb.config
Es importante validar el contenido del fichero `smb.conf` usando el programa testparm. Si testparm se ejecuta correctamente se listarán los servicios cargados, en caso contrario devolverá un mensaje de error. Introduzca el siguiente comando:

```
$ testparm /etc/samba/smb.conf
```

<img src="https://user-images.githubusercontent.com/28895475/221375077-23ab7062-c6db-4d22-a25f-3c4e0e77e6ea.png" data-canonical-src="https://user-images.githubusercontent.com/28895475/221375077-23ab7062-c6db-4d22-a25f-3c4e0e77e6ea.png" width="480" height="325"  />

La utilidad testparm también se puede utilizar para generar un archivo smb.conf totalmente optimizado a partir de un archivo maestro de configuración y documentación ejecutando el siguiente comando:

```
$ testparm -s /etc/samba/smb.conf.master > smb.conf
```
La idea es tener un fichero de configuración maestro con toda la documentación de las instrucciones para el funcionamiento de nuestro servidor Samba a modo de desarrollo y otro optimizado sin comentarios que se utilice en producción.


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
para que funcione correctamente de la siguiente manera:
```
path = /media/[username]/cine
force user = [username]
```

# Enlaces
* Wiki: https://wiki.samba.org/index.php/User_Documentation
* Sitio oficial: https://www.samba.org/samba/
