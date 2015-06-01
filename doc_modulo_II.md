~~~
DIPLOMADO DE SOFTWARE LIBRE 2015
'EGPP'

Nombre: Alvaro fabian Canqui callisaya
C.I.: 6724811 LP.


~~~

###INSTALACION Y CONFIGURACION  DE SERVIDORES LINUX Y VIRTUALIZACION
***

####Listado de archivos


* _Ordenar archivos por fecha de modificacion_

``` ls -ahtr ```

* _Almacenar salida de consola en un archivo_

``` ls -l > nombre_archivo```

* _Almacenar y concatenar la salida de consola en un archivo_

``` ls -l >> nombre_archivo```

* _Logs de archivos_

``` tail -F archivo.txt```




***
####Directorios


* _Crear directorio_

	```mkdir nombre_carpeta```

* _Crear carpeta y subcarpetas_

	```mkdir -p /nombre_carpeta/subcarpeta```

* _Crear archivos vacios_
	
	```touch nombre_archivo```
***

####Busqueda de palabras

* _Busqueda de palabras _
	
	``` grep palabra_a_abuscar ```

* _mostrar numero de lineas _
	
	``` grep -n "aa" archivo.txt ```

* _ busqueda en un grupo de archivos_
	
	```grep -nR "aa" archivo.txt  ```

* _ para resaltar con colores_
	
	``` grep -nR --color "aa" archivo.txt ```

***
####Busqueda de archivos

* _busqueda de archivo_
	
	``` find directorio -name ```

* _muestra manual de palabra reservada_
	
	``` man find ```

***
####Otros Comandos

* _muestra la ruta de comando_
	
	``` whereis nano ```

* _elimina archivo_
	
	``` rm nombre_archivo```

* _eliminar directorio_
	
	```rm -r nombre_carpeta ```

* _entrar en una carpeta o directorio_
	
	```cd ~ ```

* _instalar paquete o aplicaciones_
	
	```apt-get install nombre_paquete ```

	```aptitude install nombre_paquete ```

***
***
###INSTALACION DE DEBIAN 7.2

El equipo solo debe tener tres particiones primarias, no se pueden genrar mas particiones

####_AREA DE INTERCAMBIO_

* Area de Intercambio
* primaria
* principio
* tamaño

####_RAIZ_

* /
* logica
* principio
* tamaño

####_DATOS_
Particion seleccionada manualmente para almacenar las maquinas virtuales en este caso la particion se llama /DATOS

* /DATOS
* logica
* principio
* tamaño

Una vez instalado la distribucion de Linux verificamos si se reconoce la tarjeta de red


```$ lspci ```


```$ lspci | grep -i network ```

```$ lspci | grep -i ethernet ```

```$ lspci | grep -i wireles ```


***
####Configuracion de tarjeta de red

Para ver la interfaz grafica de la tarjeta de red :

```$ gnome-control-center network ```


***
####Agregar lineas al archivo source.list

```$ nano /etc/apt/source.list ```

adicion de la siguiente linea

deb.http://192.168.55.2/ftp.us.debian.org./debian/wheezy main contrib non-free

una vez añadida las lineas en el archivo source.list, escribimos la siguiente sentencia para actualizar los reporsitorios del equipo

```
```$ apt-get update ```
```

***
#### Añadimos el usuario al grupo de administradores

```$ adduser usuario sudo```

```$  gropus alvaro```

***
#### VERSIONAMIENTO (GIT)
```$ apt-get install git```

verificamos la version que tenemos instalada

```$ aptitude search git```

#### Creando un nuevo proyecto

```$  mkdir proyecto_git```

```$ cd proyecto git```

Ingresamos a la pagina  www.github.com

```crear repositorio ```


añadimos la url del repositorio en nuestro proyecto

```$  git remote add origin "url" ```

```$ git add README.md ```

``` $ git config --global user.name "nombre"```

``` $ git config --global user.email "usuario@usuario.com"```

``` $ git commit -m "promer versionamiento"```

``` $ git push origin master```


#### VIRTUALIZACION

Verificamos si nuestra PC cuenta con la opcion de virtualizacion

``` $ grep "vmx|svm" /proc/cpu info```

Instalamos paquete

``` $ apt-get install qemu-kvm libvirt-bin virtinst virt viewer```

Instalamos paquete de adimistracion

``` $ apt-get install virt-manager```

agragmos el usuario del equipo a la administracion

``` $ sudo adduser usuario kvm && sudo adduser usuario libvirt```

__Gestor de maquinas virtualues__

Creamos un nuevo equipo virtual  e imagen lvm para instalar la distribucion linux desde unba imagen.iso

__Particiones__

Creamos 4 particiones  tres particiones LVM, utilizaremos  particion manual creando una tabla de particiones


* boot
* swap
* / (raiz)

__Configurar gestor de volumenes logicos__

crear grupo de volumenes

* nombre: sistemas
* seleccionamos espacio libre
* creamos dos volumenes logicos
* home de 500MB
* tmp 200MB
* terminar

Selecionamos  LVM Home y LVMtmp y le agrgamos las caracteristcas de Home


__Comando para ver particiones creadas__


``` $ df -h```

__volumen de grupos__

``` $ vgdisplay```

__Redimensionando particiones__

__Aumentar tamaño__

``` $  vgdisplay```

``` $ vgs```

``` $ lvs```

Añadimos 100M a la particion

``` $ lvextend - L+100M /dev/mapper/sistemas-tmp```

desmonatamos el sistema de archivos

``` $ umount /tmp```

verificando la ingtegridad de los datos

``` $ e2fdisk -f /dev/mapper/sistemas-tmp```

``` $ resize2fs /dev/mapper/sistemas-tmp```

montamos la particion

``` $ mount /tmp```

__Disminuir tamaño__

desmontamos la particion

``` $ umount /dev/mapper/sistemas-tmp```

redimensionamos particion

``` $ e2fsck /dev/mapper/sistemas-tmp```

``` $ resize2fs /dev/mapper/sistemas_tmp 100M```

montamos la particion

``` $ mount /tmp```

***


####AGREGANDO HARDWARE

Crear una nueva imagen en el grupo de maquinas virtuales

nombre_hdaux
tamaño 2048 GB

__Creando grupo de volumenes logicos__

creando particiones fisicas

``` $ cfdisk /dev/sda```

* new
* primario
* size (MB) 1024
* Beginning
* write

``` $ fdisk -l /dev/sda1```

``` $ pvcreate /dec/sda1```

``` $ pvdisplay```

``` $ vgcreate grupoA /dev/sda1```

``` $ pvdisplay```

``` $ lvs```

Creamos volumen logico

``` $ lvcreate -L 500M -n volumen01 grupoA```

``` $ lvs```

CReamos un sistema de archivos al volumen creado

``` $ mkfs.ext3 -m 0 /dev/mapper/grupoA-volumen01```

montamos la particion

``` $ mkdir /mnt/volumen01```

``` $ mount /dev/mapper/grupoA-volumen01```

``` $ df -h```

Creamos directorios sobre la particion montada

``` $ mkdir -p /mnt/volumen01/datod/{a1,a2,a3,b1,b2,b3}```

Editamos el rchivos para que la particion creada se monte al inicio del sistema

``` $ nano /etc/fstab```

y agregamos 

``` /dev/mapper/grupoA-volumen01  /mnt/volumen01```


####Añadir espacio al disco duro 

``` $ sudo qemu-img resize lvm1.qcow2 +7G```

verificamos la existencia del nuevo espacio creado

``` $ cfdisk /dev/sda```

``` $ cfdisk -l /dev/sda```


Configuramos el equipo virtual para iniciar el sistema a partir de la aplicacion GPARTED.ISO
una vez en la aplicacion extendemos el tamaño de la particion con formato ext2 y de la particion lvm

####Añadiendo tarjeta de red al equipo virtual

sentencia para mostrar interfaz grafica

``` $ nm-connection-editor```

obteniendo las interfaces de red

``` $ ls /sys/class/net```

Agregamos el hardware correspondiente 

***
####Configuracion de la tarjeta de red

abrimos el archivo que contiene la interfaz grafica del equipo virtual

``` $ nano etc/network/interfaces```


> allow-hotplug eth0

> auto eth0


reiniciamos el servicio de red

``` $ service networking restart```

``` $ /etc/init.d/netwrking restart```

ver el detalle de ip asignadas

``` $ ip addr show```

``` $ ip addr show eth0```

``` $ ifconfig```

modificamos el archivo de interfaces


> auto eth0

> iface eth0 inet static

> address 192.168.22.100

> netmask 255.255.255.0

> gateway 192.168.3.1


Creamos un nuevo rango de red, todo esto se configura en el administrador de maquinas virtuales y podriamos crear el rango siguiente

>192.168.5.0/24

Configuracomo el DNS

``` $ nano /etc/resolv.conf```
 
y añadimos la siguiente linea
 
 > name server 192.168.122.1

Cambiamos el fdetalle de los repositorios

``` $ nano /etc/apt/source.list```

añadimos la siguiente linea

> deb http://192.168.55.2/ftp.us.debian.org/debian whezzy main contrib


__Acceso remoto al servidor o equipo virtual__

Primero necesitamos instalar un servidor __ssh__

``` $ apt-get install ssh```

el protocolo ssh trabajo sobre el puerto 22
para ver el detalle de puertos abiertos digitamos lo siguiente

``` $ netstat -natp```

``` $ netstat -natp | grep 22```

configuracion del ssh

``` $ nano -c /etc/ssh/sshd_config```

> PubKeyAuthentication yes

reiniciar el servicio para que los cambios surtan efecto

``` $ service ssh restart```

para ingresar al servidor remotamente por ssh realizamos lo siguiente:

``` $ ssh usuario@192.168.5.65```

copiar archivos al servidor

``` $ scp hola.txt root@192.168.122.80:/tmp```

copiar directorios als ervidor

``` $ scp -r /tmp/carpeta/ root@192.168.122.80:/srv```

deshabilitamos la opcion de inicio de sesion con el usuario root

``` $ nano /etc/ssh/sshd_config```

> permitRootLogin no

reiniciamos el servicio __ssh__

Ingresamos a la maquina virtual

creamos varios directorios

``` $ mkdir -p /tmp/test/{aaa,bbb,ccc,ddd}```

ahora realizamos la tarea de copiar todos los archivos desde el servidor al equipo cliente

creamos la carpeta __.ssh__

``` $ mkdir ~/.ssh```

creamos una llave ssh 

``` $ ssh-keygen -t rsa -b 2048 ```

ahora copiamos nuestra llave creada al servidor

``` $ ssh-copy-id -i /home/usuario/.ssh/id-rsa.pub root@192.168.122.80```

nuestra llave es copiada y guardada en el siguiente direccion:

> root/home/.ssh/authorized_keys

en caso de tener varias llaves publicas con esta sentencia seleccionamos con cual ingresar al servidor

``` $ ssh -i ~/.ssh/id_rsa usuario@192.168.122.80```


Permisos de archivos y directorios


> drwxr-x

> r=lectura 4

> w= escitura 2

> x ejecucion 1

para cambiar de permisos

``` $ chmmod o+w prueba/```


>Donde:

>o= grupo otros

>w= brindamos permisos de escritura al archivo 

``` $ chmod o-rwx prueba```

Para quitar todos los permisos a un archivo

``` $ chmod -rwx prueba/```

``` $ chmod 437 prueba```

cambiar de usuario propietario

``` $ chownroot:usuario prueba```

####Crear nuevos usuarios

``` $ sudo useradd usuario1```

``` $ sudo passwd usuario1```

para verificar cuantos usuarios existen en el equipo

``` $ getent passwd```

cambiar de usuario

``` $ sudo su -usuario1```







































