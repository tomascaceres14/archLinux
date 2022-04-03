# archLinux

La configuracion de ArchLinux no es tan complicada, simplemente seguir los pasos con mucho cuidado.
Ahora, estoy instalando Arch en VirtualBox, para asegurarme de conocer un poco el sistema antes de instalarlo en el SSD.

En VM, lo primero que hay que hacer es particionar el disco. Una particion para booteable para el root, otra para datos personales (/home) y otra (opcional) para el swap 
o espacio de intercambio. Esta ultima se usa para almacenar archivos temporales y asi liberar espacio en ram.

para esto, primero usamos fdisk -l para listar los discos diponibles. El disco loop0 esta por defecto (en VM y on bare-metal tmb), usaremos el Disk /dev/sda.

Para particionarlo, utilizamos fdisk *disco* /dev/sda. Ahora aparece un menu de ayuda. Si pulsamos la letra m nos mostrara las distintas opciones que tenemos. 

Para crear la tabla de particiones, pulsamos tecla "o". Esto no aplica en uefi.

Ahora, para crear la particion pulsamos "n". Nos va a preguntar si dicha particion es primaria o extendida. Al ser la del root, vamos con la letra "p" de primary.

Nos va a preguntar que numero de particion (del 1 al 4). Al no haber particiones creadas, elegimos la 1.

Primer sector lo dejamos en blanco. El ultimo sector es el tamaño de la particion de sistema. El disco de la vm tiene 8, asi q voy a asignar 4. 
Esto se hace con +*cant.Gigas*G. En este caso, +4G.

Ahora toca crear las otras 2 particiones. Estas particiones van a ser sub-particiones, quiere decir que se van a alojar en una particion.
Volvemos a pulsar "n", vamos con "e" para crear particion extendida, numero de particion 2. Ahora, como esta particion va a ocupar el resto del disco, primer y ultimo 
sector lo dejamos en blanco.

Una vez finalizado esto y de vuelta al menu de Command(m for help): , recordemos que con "p" podemos listar los discos y sus particiones.

Volvemos a utilizar "n", solo que esta vez no nos preguntara si primaria o extendida ya que no hay mas espacio en disco, por lo que logicamente empezara a crear particiones 
dentro de la particion extendida.

Ahora un poco mas de lo mismo. Primer sector libre, asignamos los gb que queremos, y luego la segunda particion.

Ahora si listamos con "p", vemos que tenemos 4 particiones. La root, de tipo Linux, la Extendida que es el grupo de particiones, otra Linux que es la de los datos personales,
y una linux mas que seria la swap. A esta ultima, tenemos que cambiarle el tipo de Linux a Swap.

Para hacer esto, pulsamos la "t". Nos preguntara el numero de particion, si nos guiamos por la descripcion de la ruta de cada una, veremos que la ultima creada es la dev6,
por lo tanto pulsamos 6. Ahora tenemos que decirle que queremos que sea Swap. Para ver todos los tipos que tenemos, "L". Swap es codigo 82, asi que insertamos 82.
Si volvemos a mostrar, veremos que dice linux swap / solaris.

Nuestro siguiente trabajo, es decirle que la primera particion va a ser booteable. Para ello, insertamos "a" y le decimos que la particion booteable sera la 1.
Para corroborar que se realizo con exito, veremos que al listar particiones, habra un asterisco en la columna de boot.

Ahora paso MUY IMPORTANTE. Corroborar que todo este configurado correctamente. Para confirmar todos estos cambios, tocamos la "w". No hay vuelta atras.

Listo! Particiones creadas. En VM no pasa nada, pero cuando haya que mandar mano sobre un disco real, precaucion en este paso pq se borra todo archivo existente.

La particion de swap no funciona ahora mismo. Para que funcione hay que escribir mkswap /dev/sda6 o la ruta de donde se haya creado.
Una vez hecho esto, para encender el swap, swapon.


Formateo de particiones:
Ahora debemos formatear ambos discos utilizando mkfs.ext4 /dev/sda1 y luego lo mismo pero /sda5, haciendo referencia a la particion con datos personales.
El disco ya esta listo, solo falta montar las particiones para poder utilizarlas.

Montar particiones:
Para esto, utilizaremos comando mount. De nuevo, recordemos que con fdisk -l listamos particiones. La primer particion a montar sera la de sistema. Lo logico seria instalarla 
en el root '/', pero esta siendo utilizada en el instalador de arch, asi que vamos a instalarla temporalmente en '/mnt'. , asi que escribimos 'mount /dev/sda1 /mnt'.

Luego hacemos lo mismo con la particion 5 (datos), dentro de mnt, en la carpeta /home. Como la carpeta home no existe, primero la creamos utilizando mkdir /mnt/home.

Descarga e Instalacion del Sistema y Kernel:
Hasta ahora creamos y montamos las particiones. Lo que debemos a hacer continuacion es llenarlas, crear nuestro linux. En este sentido es bastante simple, ya que podemos 
hacerlo con un simple comando.
pacstrap *ruta particion 1* (/mnt) *paquetes necesarios* base linux linux-firmware nano grub networkmanager dhcpcd netctl wpasupplicant dialog

base: al ser un SO desde cero, necesitaremos la base, por eso ponemos paquete base.
linux: aclaramos el kernel que instalaremos.
linux-firmware: instala controladores
nano: editor de texto
grub: cargador de arranque
networkmanager: administrador de redes para conectarse a internet
dhcpcd: cliente DNS
netctl: gestionar conexiones wifi
wpa_supplicant: por si red wifi usa wpa como contraseña

Luego de este extenso comando, damos Enter para que empiece la descarga.

Generar fstab:
Finalizadas todas las descargas, veremos que el sistema ya esta instalado, pero este sistema no puede arrancar. Para ello, crearemos un script para automatizarlo. El mismo
se llama genfstab. Si escribimos genfstab /mnt veremos la configuracion que contiene el script. Ahora, debemos reescribir esa configuracion en un fichero llamado fstab. Para
ello, ejecutamos el comando genfstab /mnt >> /mnt/etc/fstab. Para corroborar que se haya guardado la configuracion, hacemos un cat a fstab.

Acceso CHROOT al sistema recien instalado:
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
