###INSTALACION , CONFIGURACION DE LINUX Y VIRTUALIZACION###
**CONTENIDO**
1. Instalación de un servidor con LVM.
2. Configuración de la red.
3. Creación y administración de usuarios por línea de comando.
4. Modificación de archivos.
5. Administración de archivos y permisos.
6. Virtualización de servidores con KVM.
7. Solución de problemas.

###1.-INSTALACION DEBIAN 8 JEESIE###
Iniciar  la instalación.
- Elegir el país.
- Escoger el teclado latino.
- nombre: Debian.
- Dominio: nada.
- Contraseña root:
- Nombre de usuario:
- Método de particionamiento:"manual".
- Elegir : "Espacio libre".
- Crear una nueva partición de 2 gb para "SWAP"
- Primaria.
- Utilizar como área de intercambio.
- Terminar de definir la partición.+
***NOTA:***Solo puede tener 3 particiones primarias.
- Crear ahora una nueva partición lógica
- Tipo ext4 que es equivalente a NTFS para : raiz ***/*** para ***/home*** (***/var,/tmp/,/usr,/boot***)
- En punto de montaje elegir la opción :***manualmente*** y poner ***/datos*** (darle más espacio para utilizar las máquinas virtuales.)
- Luego de instalar ver si reconoce la tarjeta de red con el comando:
***$lspci ***(Muestra la información de la tarjeta de red)
***lspci | grep -i network ***(En este caso para verel modelo de la tarjeta WIFI)
***lspci | grep -i ethernet ***
Buscar el firmware con el modelo de la tarjeta con el siguiente comando:
***aptitude search ~modelo_de_la_tarjeta ***
Instalar el firmware:
***$apt-get install firmware-brcm80211*** (Instala el modelo de la tarjeta WIFI)

* * *
###2.- CONFIGURACION DE LA RED###
***CONFIGURACION EN MODO GRAFICO:*** 
Para ello debemos ejecutar el comando:
***$gnome-control-center network*** (Se abre la ventana de configuración)
- Click en cableado (añadir perfil)
- IPV4 - manual
	Poner las direccones IP
    	dirección:192.168.55.34
        Máscara:255.255.255.0
        Puerta de enlace:192.168.55.1
        DNS: 200.87.100.10
- Hacer ping al gateway para ver si hay conexión:
***$ping 192.168.55.1***
***CONFIGURACION DESDE CONSOLA:***
Ejecutar el siguiente comando:
***$nano /etc/network/interfaces*** (Ingresamos los IPs)
***$/etc/networking restart*** (para reiniciar el servicio)
####SISTEMA DE PAQUETES APT###
***INSTALACION DESDE LOS REPOSITORIOS:***

Modificar /etc/apt/sources.list
***$su*** (pide la contraseña del administrador)
***$nano /etc/apt/sources.list***
- Comentar la línea donde está ***CD room***
- Adicionar los repositorios de la EGPP:
deb http://192.168.55.2/ftp.us.debian.org/debian/whezzy main contrib non-free
***NOTA:*** Para repositorios externos quitar el IP de la EGPP y poner ***non-free***
***$apt-get update*** (actualiza los paquetes)
***$apt-get install sudo*** (Instala sudo)
Para ver si mi usuario está dentro del grupo ***sudo***:
***$groups***
Adicionar mi usuario al grupo ***sudo***:
***$adduser ***usuario ***sudo***

***INSTALACION DESDE DVD:***

Como root ejecutar el siguiente comando:
***$sudo mkdri /mnt/dvd1 ***(Creo el directorio donde voy a montar mi ISO)
***$sudo mount /mnt/dirección_donde_esta_mi_iso/iso ***(Montamos las imágenes ISO)
Editamos el archivo ***sources.list***
***$nano /etc/apt/sources.list***
Coloco la ruta donde monté mis imágenes ISO)
deb file://mnt/dvd1/debian/wheezy manin contrib
* * *
###SISTEMA DE VERSIONAMIENTO GIT
(Creado por Linus Torvalds)
Instalamos ***GIT***
***$apt-get install git***
Crear la carpeta ***proyecto***
***$cd proyecto***
***$git init*** (Creamos un repositorio vacío)
***$nano README.md*** (Creamos un archivo con la extensión ***.md***)
- Ingresar al navegador y colocar: ***github.com***
- Crearse una cuenta.
- Hacer click en ***nuevo proyecto***
- Hacer click en HTTPS y copiar la URL.
- Ir a la terminal y escribir el siguiente comando:
***$git remote add origin*** (pegamos la URL)
***$git add README.md*** (adicionamos el archivo)
***$git config--global user.name "nombre usuario"***
***$git config-global user.email "correo"***
***$git commit -m "Primer versionamiento"*** (Ponemos el comentario)
Ahora subimos al servidor:
***$git push origin master***
- Pide contraseña y usuario que hemos creado.
- Ir al navegador y actualizar el proyecto ( nos muestra el archivo README.md)
* * *
###3.- VIRTUALIZACION DE SERVIDORES###
###KVM###
KVM Es una tecnología para virtualizar sistemas operativos.
- Para procesador INTEL es vmx
- Para procesador AMD ES SVM

Ver si se puede virtualizar hardware en el equipo:
***$grep svm /proc/cpuinfo***
***$grep --color svm /proc/cpuinfo ***(Muestra el ***svm*** en color rojo)
- Instalar los paquetes necesarios:
***$apt-get install qemu-kvm libvirt-bin virtinst virt-viewer***
_ Para administrar las máquinas virtuales en modo gráfico GUI instalar el siguiente paquete:
***$apt-get install virt-manager***
- Una vez instalado adicionar el usuario al grupo:
-***$adduser ***usuario*** kvm***
-***$adduser ***usuario*** libvirt***
* * *
###CREAR UNA MAQUINA VIRTUAL###
- Abrimos Virtual Manager.
- Configurar.
- Botón derecho en Local Host.
- Detalles.
- Almacenamiento.
- Click en el ícono ***"+"***
- Poner nombre: "maquinas_virtuales"
- Adelante.
- Explorar.
- Seleccionar la carpeta ***/datos ***.
- Abrir.
- Finalizar.
- Click en la parte izquierda en máquinas virtuales.
- Click en nuevo volúmen.
- Nombre: ***vm1***
- Formato:***qcow2***
- Máxima capacidad 3 GB (en servidores recomendado 20 GB)
- Finalizar.
- Ver ***gestor***
- Luego archivo.
- Nueva máquina virtual (medio de la instalación ISO o CDROOM)
- Adelante.
- Buscar imágen ISO de debian 7.0.2
- En decargas: elija volúmen.
- Buscamos el S.O.
- Adelante.
- RAM 1 GB
- 1 procesador
- Adelante.
  	* Elija administrado o algún .....
  	* Explorar y buscar su disco duro.
  	* Buscar en máquinas virtuales y seleccionar : vm1.qcow2 3gb
  	* Elija volúmen.
  	* Adelante,adelante.
  	* Nombre: mv1
  	* Finalizar.
* * * 
- En opciones avanzadas:
- Red virtual NAT: inactivo.
- Pregunta: la red virtual está inactiva: ***Si***
- Finalizar.
- Inicializa el instalador de Debian.
###Tipos de particiones:###
Deben ser 4 , 3 primarias y una extendida
- Primaria para bootear el S.O
- Extendida y lógica para almacenar datos, puede haber otras particiones para LVM
	* Particiones:
	* /boot
	* /swap
	* LVM:
	* /var
	* /home
- Elegir ***disco virtual***
- Crear uan nueva tabla de particiones? ***Si***
- Elegir ***Espacio libre***
- Crear una partición nueva.
- Partición de booteo 256 MB
- Primaria-principio-utilizar como: ext2 (para archivos pequeños porque es más rápido al momento de iniciar)
- Punto de montaje: ***/boot***
- Finalizar la partición
- Crear partición para la ***memoria virtual SWPAP***
- Regla : 4gb de RAM o menos mínimo 2 GB.
- Como nuestro disco es de 3 GB le pondremos 512 MB
- Partición lógica.
- Al principio.
- Utilizar como: area de intercambio.
- Finalizar.
- Crear una partición más.
- 1 GB
- Lógica.
- Al principio
- tipo ***ext4***
- Punto de montaje ***raíz /***
- ***Ahora ir a configurar como LVM*** (Gestor de volúmenes lógicos)
- Guardar los cambios ? ***Si***
- Crear ***grupo de volúmenes***
- Nombre: ***sistemas***
- Seleccionamos la opción LIBRE [*]/dev/vda/free
- Guardar los cambios a los discos y config. LVM? ***Si***
- Crear volúmen lógico
- Seleccionamos el grupo: ***sistemas***
- Le damos un nombre: ***home***.
- Le damos 500 MB
- Creamos ***tmp*** (200MB) - terminar
- Seleccionamos ***LV home*** enter.
- Utilizar como ext4.
- Punto de montaje /home
- Elegir se ha terminado de...
- Seleccionamos ***LV /tmp***
- ext4
- Punto de montaje /tmp.
- Elegir se ha terminado de...
- Finalizar la partición y escribir los cambios? ***Si***
- No, no, no
- GROUB? ***Si***
- Seleccionar [*] utilidades estándar del sistema.
* * * 
***Para crear más máquinas virtuales click en el ícono "foco"***
Ver las particiones con el comando :
***$df -h***
Ver la información del grupo LVM:
***$vgdisplay***
Ver la información de los volúmenes lógicos:
***$lvdisplay***
###Para iniciar la máquina virtual previamente se debe iniciar la red:###
- Local host.
- Detalles.
- Play.
- Iniciar máquina virtual.
* * * 
###REDIMENSIONAR EL DISCO DURO###
***Ingresar al directorio donde hemos creado el disco duro***
***$cd /datos***
***$ls -l***
Sacamos una copia de seguridad del disco duro con el siguiente comando:
***$sudo cp vm1.qcow2 vm1.qcow2.back***
- Iniciar la máquina virtual.
- Ver cuántas particiones tenemos:
***$df -h***
Me muestra 2 particiones ***lvm***:
 * dev/mapper/sistemas-home.
 * dev/mapper/sistemas-tmp.
- Verificar si tenemos espacio para redimensionar con el siguiente comando:
***$vgdisplay*** (muestra todas las particiones)
***$vgs*** (ver el tamaño libre en la partición de LVM)
***$lvs*** (nos muestra información del volúmen lógico)
###Extender el disco:###
Con el siguiente comando:
***$lvextend -L+100M /dev/mapper/sistemas-tmp***
***$lvs*** (nos muestra el nuevo tamaño de las particiones LVM)
###Redimensionar el sistema de archivos:###
- Desmontamos la partición:
***$umount /tmp***
- Verificar que esté desmontado.
***$df -h***
- verificar si el sistema de archivos está correcto con el siguiente comando:
***$e2fsck -f /dev/mapper/sistemas-tmp***
- Montar la partición:
***$mount /tmp***
***$df -h***
###Reducir el tamaño del disco:
- Ejecutar el siguiente comando:
***$umount /dev/mapper/sistemas-tmp***
***$df -h *** (verificamos que esté desmontado)
- verificar si el sistema de archivos está correcto con el siguiente comando:
***$e2fsck -f /dev/mapper/sistemas-tmp***
- Reducimos el sistema de archivos:
***$resize2fs /dev/mapper/sistemas-tmp 200M***
- Montamos a partición
***$mount /tmp***
- Reducimos el espacio de la partición:
***$lvreduce -L 200M /dev/sistemas/tmp***
***$lvs***(nos muestra información del volúmen lógico)
* * *
###CREARM DISCO DURO###
- Click izquierdo en ***Local host***
- Detalles - almacenamiento.
- Maquinas_virtuales.
- Nombre:***hdaux***
- Formato:***qcow2***
- Máxima capacidad: 2GB
- Finalizar.
- ***En la máquina virtual*** click derecho.
- Abrir.
- Click en el ícono ***"foco"***
- Agregar hardware.
- Storage.
	* Elija administrado...
	* Explorar máquinas virtuales.
	* Seleccionar el disco ***hdaux.qcow2***
	* Elija volúmen.
	* Tipo de bus: ***VIRTIO***
- En opciones avanzadas:
	* Formato de almacenaje ***raw***
	* Finalizar
- Iniciar la máquina virtual.
- Dentro la máquina virtual:
***$df -h***
- Particionar el disco que hemos creado y darle formato:
***$fdisk -l | more***
***$cfdisk /dev/vdb***
***quit***
***$vgdisplay*** (nos muestra el grupo:***sistemas***, el tipo:***lvm2***)
***$pvdisplay*** (nos muestra que el grupo ***sistemas*** está dentro de /dev/vda7)
* * * 
###CREAR UN NUEVO GRUPO EN LA MAQUINA VIRTUAL###
###Crear un nuevo grupo dentro de una partición###
***$cfdisk /dev/vdb***
- New
- Primary.
- Sise (en MB) en 1024
- Beginning.
- Write.
- Sobre la partición creada: ***write,yes,quit***
***$fdisk -l /dev/vdb*** (para ver el disco)
***$reboot***
- Creamos la partición:
***$pvcreate /dev/vdb1*** 
***$pvdisplay***
- Creamos un grupo:
***$vgcreate grupoA /dev/vdb1***
***$pvdisplay*** ( nos muestra el grupo creado)
***$lvs***
- Crear un n uevo volúmen:
***$lvcreate -L 500 -n volumen01 grupoA***
***$df -h*** (vemos las particiones montadas)
- Tenemos que darle un sistema de archivos al volumen que hemos creado:
***$mkfs.ext3 -m 0 /dev/mapper/grupoA-volumen01***
- Creamos la carpeta sobre la cual queremos montar:
***$mkdir /mnt/volumen01***
***$mount /dev/mapper/grupoA-volumen01 /mnt/volumen-01***
***$df -h***
***mkdir -p /mnt/volumen-01/datos/{a1,a2,...}*** (creamos subcarpetas dentro del volúmen)
- Tenemos que montar automáticamente el volúmen:
***$nano /etc/fstab***
- Adicionamos nuestro volúmen creado.
- Guardar y salir.
***$reboot***
* * *
###EXTENDER EL TAMAÑO DEL DISCO DURO###
_Apagar la máquina virtual.
- Desde root de la terminal madre ingresar a la carpeta ***/datos***
***$cd /datos***
***$sudo qemu-img resize vm1.qcow2 +7G***
- Reiniciar la computadora.
- Click en el foco.
- Ver en ***Virtio DISK 1*** cambia el tamaño a 10 GB
- Iniciar la máquina virtual, dentro de esta ejecutar el comando:
***$cfdisk /dev/vda*** (en este caso muestra 7 gb de espacio libre)
***$ fdisk -l /dev/vda***
###Redimensión de la partición extendida en modo gráfico###
- Descargamos el archivo ***gparted.iso***
- Copiamos:
***$sudo cp /home/usuario/Descargas/gparted.iso /datos/***
	* Apagar la máquina virtual.
	* Click en el ***foco*** , agregar hardware.
	* Elija administrado...
	* Elegir gparted.iso
	* Tipo de dispositivo: CDROOM
	* Finalizar.
	* Boot options: seleccionar ID CDROOM para que bootee primero.
- Iniciar la máquina ivrtual, aparecen las opciones de ***gparted***
- Seleccionar la primera opción y enter.
- Colocar la segunda opción.
- Enter en ***dont touch keymap"***
- Elgir idioma español ***25*** y enter.
- Poner ***0*** y enter para iniciar el entorno gráfico.
- Seleccionar la ***partición extendida /dev/vda2 ***
- Click en redimensionar.
- Extender la flecha superior hasta el tope.
- Luego redimensionar.
- Apply,Apply
- Close.
- Seleccionar /dev/vda7
- Redimensionar.
- Aply, Aply
- Cambiar la opción de booteo y reiniciar la máquina virtual.
Ver la nueva partición extendida:
***$fdisk -l /dev/vda***
###Redes:###
***Desde entorno gráfico:***
- Network.
- Network manager
*** Desde línea de comando:***
***$nm-conection-editor*** (muestra las conecciones de red en forma gráfica)
***$ls /sys/class/net*** (muestra las interfases de red físicas y virtuales)
- Apagar la máquina virtual.
- Click en el foco.
- Agregar hardware.
- Network
- Red virtual default: NAT
- Finalizar.
- Iniciar la máquina.
###Configuración básica DHCP###
- En la máquina virtual:
***$nano /etc/network/interfaces***
- Comentar allow-hotplug eth0 y poner auto eth0
- Reiniciar el servicio con el siguiente comando:
***$service networking restart***
- Verificar el IP que me ha asignado:
***$ip addr show***
-o
***$ifconfig***
###ADICIONAR OTRA RED###
- Click deracho en ***Local host***
- Redes virtuales.
- Default.
- Click en el ícono ***"+"*** (adicionar)
- Cambiar el rango de ips
- Desactivar IPV6
- Reenvío a la red física.
- Finalizar.
- Click en el ***foco***
- Elegir la fuente de red: en este caso ***red virtual egpp***
***$reboot***
- Configurar el DNS
***$nano /etc/resolv.conf***
- name server 192.168.6.1
-Configurar repositorios:
***$nano /etc/apt/sources.list***
deb http://192.168.55.2/ftp.us.debian.org/debian/wheezy main contrib

***Para accedeer remotamente a nuestro servidor:***
debmos instalar ***SSH***
***$apt-get install ssh***
- ***SSH*** trabaja sobre el puerto 22, para ver los puertos abiertos:
***$netstat -natp***
***$netstat -natp | grep 22***
*** nano -c /etc/ssh/sshd-config ***(colocar ***no*** el la línea 31 pubkeyAuthen...)
- Reiniciar el servicio:
***$service ssh restart***
- Desde la máquina ***madre***:
***$ssh root@192.168.100.80*** (nos conectamos via ssh)
- creamos un archivo para copiarlo.
***$echo "hola mundo" > hola.txt"***
***scp hola.txt root@192.168.100.80:/tmp***
- Crear una carpeta y copiarla al servidor remoto:
***$mkdir -p /tmp/datos_egpp/{aaa,bbb,ccc}***
- Copiamos la carpeta creada y subcarpetas en forma recursiva.
***$scp -r /tmp/datos_egpp/ root@192.168.100.80:/srv***
- En la máquina virtual deshabilitamos el acceso a root en la línea 27.
***$nano -c /etc/ssh/sshd_config***
***$service ssh restart***
###AUTENTICACION POR LLAVES PUBLICAS PARA INGRESAR SIN PONER CONTRASEÑAS:CREAR LA LLAVE###
***$who*** (para ver que terminales están conectadas)
- En la máquina madre crear carpeta que se llame ***.ssh*** ( si es que no existe)
***$mkdir ~/.ssh***
- Generar la llave:
***$ssh-keygen -t rsa -b 2048***
- La llave se almacena en el directorio ~/.ssh
- Copiar la llave al servidor:
***$ssh-copy-id -i /home/usuario/ .ssh/id_rsa.pub miguel@192.168.100.80***
***$nano -c /etc/ssh/sshd_config***
- Ir a la líneA 31 Y CAMBIAR PublicAutentication a yes)
***$service ssh restart***
###ADMINISTRACION DE ARCHIVOS Y PERMISOS###
***$ls -l***
drwxr-xr-x
- Permisos: r = lectura, w=escritura,x=ejecución
***$chmod O+W prueba/***
***$o-rwx prueba*** (Quita todos los permisos del grupo ***otros***)
***$sudo chown root:miguel prueba/*** (da los permisos de root al usuario miguel)
###CREAR USUARIOS###
***$sudo useradd usuario1 ***(crear usuario)
***$sudo passwd usuario1*** (asignar password)
***$getent passwd***(muestra todos los usuarios que hay)
***$sudo su -usuario1***(para cambiar de usuario)











    




  	








