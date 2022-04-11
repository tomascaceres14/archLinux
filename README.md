# Mi camino hacia el Software Libre

## Introducción

Hola, mi nombre es Tomas Alegria Caceres y toda mi vida fui usuario de Windows (como el 99%) de la poblacion. Hoy, 5 de abril de 2022 se cumplen aprox. 9 meses de que 
inicie mi camino en el mundo de la informatica y programacion. En los ultimos meses, me pico el bichito de Linux y el codigo abierto. Tengo que admitir que siempre le 
tuve miedo a este tipo de cosas, simplemente por puro desconocimiento. Asi que en parte esto es una forma de luchar contra ese miedo, salir de mi zona de confort y 
ponerme en "modo esponja", con el objetivo de aprender todo lo que mas pueda ya que mi objetivo es algun dia poder trabajar como DevOp. Esto lo hago para que quede un 
registro de todo lo que fui aprendiendo al adentrarme en el mundo de Linux y el codigo abierto.

## Por que arch Linux?

Decidi arrancar con Arch Linux ya que de todas las distribuciones de Linux, esta es una de las mas dificiles de instalar y dejar como un sistema operativo "comun y corriente". Esto se debe a que 1) no tenemos instalador, hay que instalarlo manualmente en el disco y 2) una vez instalado, solo tenemos los directorios necesarios para que el sistema funcione, pero no poseemos ni navegador web, ni interfaz, en algunos casos ni siquiera tenemos el comando sudo (vital en linux), entre otros, para que se den una idea. 

Por lo general recomiendan empezar con distros mas faciles, que vengan con funciones preparadas, pero como ya tengo algunos conocimientos en Bash Scripting y manejo de consola, decidi ir por el metodo mas complicado jajaja.

## Objetivo de este repositorio

Ademas de lo ya aclarado en el parrafo anterior, quiero dos cosas principales mas: 

- Construir el sistema a mi gusto y placer, con las decoraciones que me gusten y las herramientas que yo considere que sean necesarias para mi uso.

- Que todo lo que vaya documentando en este archivo sea de facil comprension para cualquiera que haya pasado por lo mismo que yo. Esto ya que al comenzar con mi proceso de investigacion, me encuentro con que la mayoria de guias o tutoriales eran un tanto complejos, explcados de maneras que me resultaba complicado entender lo que estaba pasando, ademas de que para un problema que se nos presente, existen infinidad de soluciones, lo que a veces puede llevar a uno a marearse y terminar en una gran frustracion (lo digo por exp).

## Herramientas para trabajar

Primero que nada vamos a necesitar un software que nos permita crear maquinas virtuales. Esto con la intencion de lograr la instalacion y configuracion basica de nuestro sistema en un entorno seguro, donde en caso de cometer algun error, podamos simplemente borrarla y crear otra. En mi caso, voy a estar utilizando [Virtual Box](https://www.virtualbox.org/wiki/Downloads).

La imagen iso de archLinux que vamos a instalar. Adjunto [pagina oficial de descargas](https://archlinux.org/download/).

Por ultimo pero no menos importante, una buena [playlist](https://www.youtube.com/watch?v=5qap5aO4i9A&ab_channel=LofiGirl) para concentrarse y juntar paciencia porque esto lleva tiempo jeje.

### Por si no saben usar VirtualBox:
https://clomatica.com/como-arrancar-e-instalar-desde-iso-en-virtualbox/

## Proceso de instalacion

La configuracion de ArchLinux no es tan complicada, simplemente seguir los pasos con mucho cuidado.
Ahora, estoy instalando Arch en VirtualBox, para asegurarme de conocer un poco el sistema antes de instalarlo en el SSD.

### Configuracion VM

Lo primero que debemos hacer es crear una maquina virtual, asignarle cantidad de RAM (recomiendo 2048 mb), almacenamiento, etc. (Basicamente, una vez asignada la ram darle siguiente siguiente siguiente hasta que termine). Una vez creado, le damos click derecho > Configuracion > Almacenamiento > Controlador IDE y elegimos la imagen iso de Arch.

**IMPORTANTE** en los settings ir a Network o Redes y cambiar la primer opcion a Adaptador Puente o Bridged adapter para que la Maquina Virtual reconozca el wifi de nuestra computadora.

### Instalador

Elegimos la primera opcion "Arch linux Install medium x86".

### Particionado de discos

Lo primero que hay que hacer es particionar el disco. Una particion la raiz del sistema(root), otra para datos personales (/home) y otra para el swap o espacio de intercambio. Estas dos ultimas son subparticiones, que las vamos a alojar dentro de una particion extendida.

>Swap es un espacio en el almacenamiento reservado para, en terminos simples, "ayudar" a la RAM a liberar carga y almacenar ahi archivos temporales. [Mas sobre Swap](https://es.wikipedia.org/wiki/Espacio_de_intercambio).


**Luego de terminar con todo el proceso, nos deberia quedar tal que asi:**
![image](https://user-images.githubusercontent.com/92989104/162602672-9c489999-3bcf-4feb-9c24-d60b23369f69.png)

- Usamos el comando fdisk (permite ver y administrar los discos de nuestra maquina)  `fdisk -l` para listar los discos diponibles. Nos encontraremos con el disco loop0, el cual se encuentra por defecto, y con el disco /dev/sda que es con el que vamos a trabajar. Ahora ejecutamos `fdisk /dev/sda` para abrir el menu de particion. Con la letra `m` observamos las distintas opciones que tenemos. 

>[Mas sobre fdisk](https://openwebinars.net/blog/9-comandos-basicos-fdisk-para-gestionar-el-disco-duro/)

- Para crear la tabla de particiones, pulsamos tecla "o". [Mas sobre tablas de particiones](https://es.wikipedia.org/wiki/Tabla_de_particiones#:~:text=Una%20tabla%20de%20particiones%20es,de%20solo%20lectura%2C...)

- Ahora, para crear la primer particion pulsamos `n`. Nos va a preguntar si dicha particion es primaria o extendida. Al ser la del root, osea donde se va a instalar la raiz del Sistema Operativo, vamos con `p` de primary. Nos va a preguntar que numero de particion (del 1 al 4). Al no haber particiones creadas, elegimos la 1.

>A continuacion vamos a trabajar sobre los sectores del disco, que hace referencia a contenido mucho mas complejo sobre el hardware del propio HDD. Por ende, no voy a documentar de que se trata porque ni yo entendi, pero que sepan que siguiendo los pasos que les indico no van a tener problemas

- Primer sector lo dejamos en blanco. El ultimo sector es el tamaño de la particion de sistema. El disco de la vm tiene 8GB, asi q voy a asignar 4. Esto se hace con +*cant.Gigas*G. En este caso, `+4G`.

- Ahora toca crear la particion extendida que alojara luego las otras dos subparticiones restantes (/home y swap). Volvemos a pulsar `n`, vamos con `e` para crear particion extendida, numero de particion 2. Ahora, como esta particion va a ocupar el resto del disco, primer y ultimo sector lo dejamos en blanco.

>Recordemos que con "p" podemos listar los discos y sus particiones.

- Toca repetir el mismo proceso para las dos restantes, con la diferencia de que a la primera que creemos le asignamos 3GB y a la segunda 1GB (sumando asi, el total de 4GB que tiene la particion extendida contenedora). Esta vez no nos preguntara si primaria o extendida ya que no hay mas espacio en disco, por lo que logicamente empezara a crear particiones dentro de la particion extendida.

Si hicimos todo bien, listando todos los discos deberiamos poder ver esto:

![image](https://user-images.githubusercontent.com/92989104/162602909-b1ea2efc-2554-479a-b159-5cec2f102613.png)


Ahora si listamos con `p`, vemos que tenemos 4 particiones. La root, de tipo Linux, la Extendida que es el grupo de particiones, otra Linux que es la de los datos personales, y una linux mas que seria la swap. A esta ultima, tenemos que cambiarle el tipo de Linux a Swap.

- Para hacer esto, pulsamos `t`. Nos preguntara el numero de particion, si nos guiamos por la descripcion de la ruta de cada una veremos que la ultima creada es la dev6, por lo tanto ingresamos 6. Ahora tenemos que decirle que queremos que sea Swap. Para ver todos los tipos que tenemos, "L". Swap tiene el codigo 82, asi que insertamos 82. Si volvemos a mostrar, veremos que dice linux swap / solaris.

Nuestro siguiente trabajo, es decirle que la primera particion va a ser booteable. 

- Insertamos "a" y le decimos que la particion booteable sera la 1. Para corroborar que se realizo con exito, veremos que al listar particiones, habra un asterisco en la columna de boot.

_**MUY IMPORTANTE**:_ Corroborar que todo este configurado correctamente antes de aceptar los cambios ya que no hay vuelta atras. Para confirmar todos estos cambios, tocamos la `w`

Listo! Particiones creadas!

La particion de swap no funciona ahora mismo. Para que funcione hay que escribir mkswap /dev/sda6 o la ruta de donde se haya creado. Una vez hecho esto, para encender el swap, swapon.

### Formateo de particiones:

Ahora debemos formatear ambos discos utilizando`mkfs.ext4 /dev/sda1` y luego lo mismo pero cambiamos la parte de `/sda1` por `/sda5`, haciendo referencia a la particion con datos personales. El disco ya esta listo, solo falta montar las particiones para poder utilizarlas.

### Montar particiones:

Para esto, utilizaremos comando `mount`. De nuevo, recordemos que con `fdisk -l` listamos particiones. La primer particion a montar sera la de sistema. Lo logico seria instalarla en el root '/', pero esta siendo utilizada por el instalador de Arch, asi que vamos a instalarla temporalmente en '/mnt'.

- escribimos 'mount /dev/sda1 /mnt'.

Luego hacemos lo mismo con la particion 5 (datos), dentro de mnt en una carpeta que llamaremos /home. Como la carpeta home no existe, primero debemos crearla.

- `mkdir /mnt/home` para crear dicha carpeta. 

- Ahora si, ejecutamos `mount /dev/sda5 /mnt/home` para finalizar con el montado de particiones. 

### Descarga e Instalacion del Sistema y Kernel:

Hasta ahora creamos y montamos las particiones. Lo que debemos a hacer continuacion es llenarlas para "crear" nuestro linux. En este sentido es bastante simple, ya que podemos hacerlo con un simple comando.

El siguiente comando descarga todos los elementos necesarios para que el sistema funcione correctamente. pacstrap es el gestor de paquetes (herramienta que nos permite descargar archivos), /mnt es la ruta de instalacion y luego indicamos separando con espacios todos los paquetes que queremos descargar e instalar.

`pacstrap /mnt base linux linux-firmware nano grub networkmanager dhcpcd netctl wpa_supplicant dialog`

- base: al ser un SO desde cero, esto nos instala la base del sistema.
- linux: aclaramos el kernel que instalaremos.
- linux-firmware: instala controladores
- nano: editor de texto
- grub: cargador de arranque
- networkmanager: administrador de redes para conectarse a internet
- dhcpcd: cliente DNS
- netctl: gestionar conexiones wifi
- wpa_supplicant: por si red wifi usa wpa como contraseña

Luego de este extenso comando, damos Enter para que empiece la descarga (consejo: prepararse un cafe porque puede tomar un rato).

### Generar fstab:

Finalizadas todas las descargas, veremos que el sistema ya esta instalado, pero este no puede arrancar. Para ello, utilizamos un script para automatizar su arranque. El mismo se llama _**genfstab**_. Si escribimos `genfstab /mnt` veremos la configuracion que contiene el script. Ahora, debemos reescribir esa configuracion en un fichero llamado fstab. 

- Ejecutamos el comando `genfstab /mnt >> /mnt/etc/fstab`. Para corroborar que se haya guardado la configuracion, hacemos `cat /mnt/etc/fstab`.

### Acceso CHROOT al sistema recien instalado:

Bueno, luego de toodo esto, aun seguimos en el instalador, asi que vamos a entrar en ese sistema recien instalado. Para ello, comando arch-chroot /mnt

Configuraciones basicas:
Configurar hora local: ln -sf /usr/chare/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
Configurar reloj: hwclock --systohc
Configurar hostname: echo "TomiPC" > /etc/hostname
Configurar localhost: echo "127.0.0.1 localhost" > /etc/host

Creacion de usuarios y contraseñas:
Establecer contraseña para root: passwd
Crear usuario: useradd -m(crea carpetas del usuario) tomi(nombre, solo letras minusculas)
Crear contraseña usuario: passwd tomi

Configuracion de GRUB:
A pesar de ya tener asignado el fstab, tenemos que configurar GRUB para que el sistema pueda bootear, sino no nos dejara entrar (digo por experiencia, esto estaba casi al
final del tutorial, tuve que apagar la VM y cuando volvi no me dejaba entrar, deberia haber sido la primer config pero bueno xd).

ejecutar grub-install /dev/sda

Grub instalado, ahora vamos a configurarlo:
grub-mkconfig -o(argumento para guardarlo en su archivo de config) /boot/grub/grub.cfg

Ahora solo falta regenerar el disco de imagen de arranque. Linux utiliza una imagen de arranque, la cual debemos actualizar para que funcione  correctamente.
ejecutar mkinitcpio -P

FIN!!! Arch Linux instalado.

Ahora, ejecutamos exit para salir del SO, despues en el instalador ejecutamos reboot now para reiniciar

Instalacion de red:
network manager esta instalado pero no esta activado. Para iniciarlo, nos pasamos al root

ejecutamos systemctl start NetworkManager.service Esto dara de alta el servicio de red.
Para que se ejecute automaticamente cada vez que encendamos la pc, ejecutar lo mismo cambiando start por enable

Conectarse a internet:
antes que nada, probar hacer un ping a cualquier sitio (ej: google.com). S=
ip link para ver dispositivos de red.
prestar atencion a la segunda tarjeta que aparezca, esa es la que nos interesa.
ip *nombre tarjeta* enp0s3 up (En el caso de la VM, ya estaba conectado a internet)

*PARA INSTALACION ON BARE METAL*
para conectar al wifi:
nmcli dev wifi connect *nombre red* password *contraseña de red*

Programas a instalar:
El primer problema con el que me encontre fue que no tenia sudo instalado por defecto. Esto se debe a que cuando le asignamos una contraseña al usuario root, este no nos instala
sudo. En caso de que no tenga contraseña, si sera necesario. Descargarlo como usuario root (recordemos que se utiliza el admin de paquetes pacman)
pacman -S sudo. Luego, editar el archivo /etc/sudoers y al fondo de el mismo agregar usuarionombre ALL=(ALL) ALL.

Drivers de placa de video:
para AMD: xf86-video-amdgpu
para NVIDIA: xf86-video-nouveau


Programas para gestor de ventanas:
sudo pacman -S sddm (Servicio grafico)
sudo pacman -S qtile  (Gestor de ventanas) 
sudo pacman -S alacritty  (es una consola mas rapida)
sudo pacman -S neofetch   (un paquete que nos muestra la distro de linux)
sudo pacman -S rofi   (un menu rapido para acceder y abrir aplicaciones instaladas)

Comandos utiles:
win+enter   = abre una terminal
win+w = cierra una terminal
win+shift+q  = cierra sesion
win+j  = navegas de una ventana a otra
ctrl+1 vas al primer escritorio 
win+m = vas al menu para abrir aplicaciones instaladas

Una vez instalados todos estos programas para gestionar interfaz y ventanas, debemos activar el servicio grafico. Para eso systemctl enable sddm.service y luego lo mismo pero
cambiando enable por start
