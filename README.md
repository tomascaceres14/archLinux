# Mi camino hacia el Software Libre

## Introducción

Hola, mi nombre es Tomás Alegría Cáceres y toda mi vida fui usuario de Windows (como el 99% de la población). Hoy, 5 de abril de 2022 se cumplen aproximadamente 9 meses desde que inicié mi camino en el mundo de la informática y programación. En los últimos meses, me picó el bichito de Linux y el código abierto. Tengo que admitir que siempre le tuve miedo a este tipo de cosas, simplemente por puro desconocimiento. Asi que en parte, esto es una forma de luchar contra ese miedo, salir de mi zona de confort y ponerme en "modo esponja", con el objetivo de aprender todo lo que mas pueda ya que mi objetivo es algun dia poder trabajar como DevOp. Esto lo hago para que quede un registro de todo lo que fui aprendiendo al adentrarme en el mundo de Linux y el código abierto.

## Por qué arch Linux?

Decidí arrancar con Arch Linux ya que de todas las distribuciones de Linux, esta es una de las mas difíciles de instalar y dejar como un sistema operativo "común y corriente". Esto se debe a que 1) no tenemos instalador (se debe instalar manualmente en el disco), 2) una vez instalado, solo tenemos los directorios necesarios para que el sistema funcione, pero no poseemos ni navegador web, ni interfaz, en algunos casos ni siquiera tenemos el comando sudo (vital en Linux), entre otros, para que se den una idea. 

Por lo general recomiendan empezar con distros mas fáciles, que vengan con funciones preparadas, pero como ya tengo algunos conocimientos en Bash Scripting y manejo de consola, decidí ir por el método mas complicado jajaja.

## Objetivo de este repositorio

Ademas de lo ya aclarado en el parrafo anterior, busco dos cosas principales mas: 

- Construir el sistema a mi gusto y placer, con las decoraciones que me gusten y las herramientas que yo considere que sean necesarias para mi uso.

- Que todo lo que vaya documentando en este archivo sea de fácil comprensión para cualquiera que haya pasado por lo mismo que yo. Esto ya que al comenzar con mi proceso de investigación, me encuentro con que la mayoria de guias o tutoriales eran un tanto complejos, explicados de maneras que me resultaba complicado entender lo que estaba pasando, además de que para un problema que se nos presente, existen infinidad de soluciones, lo que a veces puede llevar a uno a marearse y terminar en una gran frustración (lo digo por experiencia).

## Herramientas para trabajar

Primero que nada vamos a necesitar un software que nos permita crear máquinas virtuales. Esto con la intención de lograr la instalación y configuración básica de nuestro sistema en un entorno seguro, donde en caso de cometer algún error, podamos simplemente borrarla y crear otra. En mi caso, voy a estar utilizando [Virtual Box](https://www.virtualbox.org/wiki/Downloads).

La imágen iso de archLinux que vamos a instalar. Adjunto [pagina oficial de descargas](https://archlinux.org/download/).

Por último pero no menos importante, una buena [playlist](https://www.youtube.com/watch?v=5qap5aO4i9A&ab_channel=LofiGirl) para concentrarse y juntar paciencia porque esto lleva tiempo jeje.

### Por si no saben usar VirtualBox:
https://clomatica.com/como-arrancar-e-instalar-desde-iso-en-virtualbox/

**IMPORTANTE: SI VAMOS A INSTALAR EL SISTEMA, REALIZAR TODO EL PROCESO SIN APAGAR LA MÁQUINA HASTA QUE INSTALEMOS EL GRUB.** De otra forma, si apagamos la VM tendremos que hacer todo desde 0.

## Proceso de instalación

La configuración de ArchLinux no es tan complicada, simplemente seguir los pasos con mucho cuidado.
Ahora, estoy instalando Arch en VirtualBox, para asegurarme de conocer un poco el sistema antes de instalarlo en el SSD.

### Configuración VM

Lo primero que debemos hacer es crear una máquina virtual, asignarle cantidad de RAM (recomiendo 2048 Mb), almacenamiento, etc. (Básicamente, una vez asignada la memoria RAM, dar click a "siguiente" hasta que termine). Una vez creado, le damos click derecho > Configuracion > Almacenamiento > Controlador IDE y elegimos la imágen iso de Arch.

**IMPORTANTE** en los settings ir a Network o Redes y cambiar la primer opción a Adaptador Puente o Bridged Adapter para que la Maquina Virtual reconozca el wifi de nuestra computadora.

### Instalador

Elegimos la primera opción "Arch linux Install medium x86".

### Particionado de discos

Antes que nada, vamos a aclarar este concepto un poco. Una partición es una división o porción del disco duro que se crea con el fin de instalar un sistema operativo o almacenar información. Estas divisiones son diferentes secciones virtuales, de manera que físicamente tales particiones no existen. Los sistemas interpretan y manipulan las particiones como un disco independiente, a pesar de que dichas particiones se encuentren en un mismo disco fisico.

>Les dejo un [video](https://www.youtube.com/watch?v=CSbUiK2pytE) que explica muy bien lo basico sobre particiones y sistemas de archivos. 

Lo primero que hay que hacer es particionar el disco en tres partes. La primera particion será para la raiz del sistema(root), la segunda para datos personales (/home) y la tercera para el swap o espacio de intercambio. Estas dos últimas son subparticiones, y las vamos a alojar dentro de una partición extendida. [Mas informacion util sobre particiones en Linux.](https://www.compuhoy.com/como-uso-la-particion-extendida-en-linux/)

>Swap es un espacio en el almacenamiento reservado para, en términos simples, "ayudar" a la RAM a liberar carga y almacenar allí archivos temporales. [Mas sobre Swap](https://es.wikipedia.org/wiki/Espacio_de_intercambio).


**Luego de terminar con todo el proceso, nos deberia quedar algo así:**
![image](https://user-images.githubusercontent.com/92989104/162602672-9c489999-3bcf-4feb-9c24-d60b23369f69.png)

- Usamos el comando fdisk (permite ver y administrar los discos de nuestra maquina)  `fdisk -l` para listar los discos diponibles. Nos encontraremos con el disco loop0 (el cual se encuentra por defecto) y con el disco /dev/sda , que es con el que vamos a trabajar. Ahora, ejecutamos `fdisk /dev/sda` para abrir el menu de partición. Con la letra `m` observamos las distintas opciones que tenemos. 

>[Mas sobre fdisk](https://openwebinars.net/blog/9-comandos-basicos-fdisk-para-gestionar-el-disco-duro/)

- Para crear la tabla de particiones, pulsamos tecla "o". [Mas sobre tablas de particiones](https://es.wikipedia.org/wiki/Tabla_de_particiones#:~:text=Una%20tabla%20de%20particiones%20es,de%20solo%20lectura%2C...)

- Ahora, para crear la primer partición pulsamos `n`. Nos va a preguntar si dicha partición es primaria o extendida. Al ser la del root, o sea, donde se va a instalar la raíz del Sistema Operativo, vamos con `p` de primary. Nos va a preguntar que numero de partición (del 1 al 4). Al no haber particiones creadas, elegimos el numero 1.

>A continuación vamos a trabajar sobre los sectores del disco, que hace referencia a contenido mucho mas complejo sobre el hardware del propio HDD. Por ende, no voy a documentar de que se trata porque ni yo entendi, pero que sepan que siguiendo los pasos que les indico no van a tener problemas.

- El primer sector lo dejamos en blanco. El ultimo sector es el tamaño de la particion de sistema. El disco de la vm tiene 8GB, asi que voy a asignar 4. Esto se hace con +*cant.Gigas*G. En este caso, `+4G`.

- Ahora, debemos crear la partición extendida que luego alojará las otras dos subparticiones restantes (/home y swap). Volvemos a pulsar `n` y presionamos `e` para crear la partición extendida, numero de partición 2. Ahora, como esta va a ocupar el resto del disco, el primer y último sector lo dejamos en blanco.

>Recordemos que con "p" podemos listar los discos y sus particiones.

- Debemos repetir el mismo proceso para las dos restantes, con la diferencia de que a la primera que creemos le asignamos 3GB y a la segunda 1GB (sumando asi, el total de 4GB que tiene la partición extendida contenedora). Esta vez no nos preguntará si primaria o extendida ya que no hay mas espacio en el disco, por lo que lógicamente, empezará a crear particiones dentro de la partición extendida.

Si hicimos todo bien, listando todos los discos deberiamos poder ver esto:

![image](https://user-images.githubusercontent.com/92989104/162602909-b1ea2efc-2554-479a-b159-5cec2f102613.png)


Ahora, si listamos con `p`, vemos que tenemos 4 particiones. La root (de tipo Linux), la Extendida (que es el grupo de particiones), otra Linux que es la de los datos personales, y una linux mas que seria la swap. A esta última, tenemos que cambiarle el tipo de Linux a Swap.

- Para hacer esto, pulsamos `t`. Nos preguntará el numero de partición. Si nos guiamos por la descripción de la ruta de cada una, veremos que la última creada es la dev6, por lo tanto ingresamos 6. Ahora tenemos que decirle que queremos que sea Swap. Para ver todos los tipos que tenemos ingresar "L". Swap tiene el codigo 82, asi que insertamos 82. Si volvemos a mostrar, veremos que dice linux swap / solaris.

Nuestro siguiente trabajo, es decirle que la primera partición va a ser booteable. 

- Insertamos "a" y le decimos que la partición booteable sera la 1. Para corroborar que se realizó con exito, veremos que al listar particiones, habrá un asterisco en la columna de boot.

_**MUY IMPORTANTE**:_ Corroborar que todo esté configurado correctamente antes de aceptar los cambios ya que no hay vuelta atras. Para confirmar todos estos cambios, presionamos `w`.

Listo! Particiones creadas!

La partición de swap no funciona ahora mismo. Para que funcione debemos ingresar mkswap /dev/sda6 o la ruta de donde se haya creado. Una vez hecho esto, para encender el swap: swapon.

### Formateo de particiones:

Ahora debemos formatear ambos discos utilizando `mkfs.ext4 /dev/sda1`; luego ingresamos el comando anterior, sin embargo cambiamos la parte de `/sda1` por `/sda5`, haciendo referencia a la partición con datos personales. De esta manera, el disco ya esta listo, solo falta montar las particiones para poder utilizarlas.

### Montar particiones:

Para esto, utilizaremos comando `mount`. De nuevo, recordemos que con `fdisk -l` podemos listar las particiones. La primer partición a montar será la de sistema. Lo mas lógico sería instalarla en el root '/', pero esta está siendo utilizada por el instalador de Arch, asi que vamos a instalarla temporalmente en '/mnt'.

- Escribimos 'mount /dev/sda1 /mnt'.

Luego hacemos lo mismo con la partición 5 (datos), dentro de mnt en una carpeta que llamaremos /home. Como la carpeta home no existe, primero debemos crearla.

- `mkdir /mnt/home` para crear dicha carpeta. 

- Ahora si, ejecutamos `mount /dev/sda5 /mnt/home` para finalizar con el montado de particiones. 

### Descarga e Instalación del Sistema y Kernel:

Hasta ahora creamos y montamos las particiones. Lo que debemos hacer a continuación es llenarlas para "crear" nuestro linux. En este sentido es bastante simple, ya que podemos hacerlo con un sencillo comando.

El siguiente comando descarga todos los elementos necesarios para que el sistema funcione correctamente: 

`pacstrap /mnt base linux linux-firmware nano grub networkmanager dhcpcd netctl wpa_supplicant dialog`

pacstrap es el gestor de paquetes (herramienta que nos permite descargar archivos), /mnt es la ruta de instalación y luego indicamos separando con espacios todos los paquetes que queremos descargar e instalar:
- base: al ser un SO desde cero, esto nos instala la base del sistema.
- linux: aclaramos el kernel que instalaremos.
- linux-firmware: instala controladores
- nano: editor de texto
- grub: cargador de arranque
- networkmanager: administrador de redes para conectarse a internet
- dhcpcd: cliente DNS
- netctl: gestionar conexiones wifi
- wpa_supplicant: por si red wifi usa wpa como contraseña

Luego de este extenso comando, damos Enter para que empiece la descarga (consejo: prepararse un café porque puede tomar un rato).

### Generar fstab:

Finalizadas todas las descargas, veremos que el sistema ya esta instalado, pero este no puede arrancar. Para ello, utilizamos un script para automatizar su arranque. El mismo se llama _**genfstab**_. Si escribimos `genfstab /mnt` veremos la configuración que contiene el script. Ahora, debemos reescribir esa configuración en un fichero llamado fstab. 

- Ejecutamos el comando `genfstab /mnt >> /mnt/etc/fstab`. Para corroborar que se haya guardado la configuración correctamente, ejecutamos `cat /mnt/etc/fstab`.

### Acceso CHROOT al sistema recién instalado:

Luego de todo este proceso, aún nos encontramos en el instalador, asi que vamos a entrar en ese sistema recién instalado. Para ello, ingresamos el comando `arch-chroot /mnt`

### Configuracion de GRUB:

A pesar de ya tener asignado el fstab, tenemos que configurar GRUB para que el sistema pueda bootear, si no, este no nos dejará entrar (lo digo por experiencia). Grub es un cargador de arranque múltiple, lo que nos abre un menú para elegir el sistema que queremos iniciar.

>[Mas sobre GRUB](https://es.wikipedia.org/wiki/GNU_GRUB)

- Ejecutar `grub-install /dev/sda`

Grub instalado, ahora vamos a configurarlo:
- `grub-mkconfig -o /boot/grub/grub.cfg`

En este punto, solo falta regenerar el disco de imágen de arranque. Linux utiliza una imágen de arranque, la cual debemos actualizar para que funcione correctamente.

- Ejecutar `mkinitcpio -P`

Ahora si somos libres de poder apagar la maquina, total el sistema base ya esta creado. Cuando se vuelva a abrir el instalador, no tocar la primera opción y elegir la que dice "Boot existing OS".

### Creación de usuarios y contraseñas:

- Establecer contraseña para root: `passwd`

- Crear usuario: useradd -m(crea carpetas del usuario) _nombreusuario_(nombre, solo letras minusculas)
- Crear contraseña usuario: passwd _nombreusuario_

### Configuraciones básicas:

- Configurar hora local: `ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime` En mi caso puse horario de Buenos Aires ya que es el mas cercano a  mi ubicación, sin embargo esto depende de la ubicación geográfica en donde te encuentres.(_tip_: al escribir rutas, luego de la barra '/', si presionan dos veces el tab les lista todo lo que contiene dicha carpeta).
- Configurar reloj: `hwclock --systohc`
- Configurar hostname: `echo "nombremaquina" > /etc/hostname`
- Configurar localhost: `echo "127.0.0.1 localhost" > /etc/host`

### Instalación de Sudo

En Linux, el comando que mas vamos a utilizar es el `sudo`. El mismo nos permite elevar los derechos de usuario para ejecutar comandos que tendríamos que ejecutar como root. Cuando le asignamos contraseña al root, por defecto no se instala.

- Con el usuario root, ejecutar `pacman -S sudo`

Una vez finalizado, debemos editar un archivo donde se guarda la configuración del comando. En el, debemos indicarle que el usuario que previamente creamos, va a tener derecho para utilizar sudo.

- Como usuario root, utilizamos el comando `nano /etc/sudoers`. 

  Nota: Recordemos que previamente instalamos "nano", un editor de texto de consola.

- Desplazarse hasta el fondo del archivo. Deberiamos poder ver algo como esto:

![image](https://user-images.githubusercontent.com/92989104/162651139-7ba73eaf-b961-4bfe-9302-97cddc98ea91.png)

Si observamos donde señala la flecha blanca, podemos notar que dice "User privilege specification"; es allí donde debemos especificar que usuarios pueden utilizar el comando. En la flecha verde se señala la sentencia que le da derechos a mi usuario.

- Donde apunta la flecha verde, o debajo de la sentencia del root, ejecutar ` username ALL=(ALL:ALL) ALL` logicamente reemplazando _username_ por el nombre que ustedes le hayan dado a su usuario.

- `Ctrl + X` para guardar,  `Y` para confirmar los cambios y  `Enter` para guardarlo bajo el mismo nombre.

**FIN!!! Arch Linux instalado.**

Ahora, ejecutamos exit para salir del SO, despues en el instalador ejecutamos reboot now para reiniciar.

Recomiendo que una vez llegado a este paso, apagar la maquina, dirigirse a la configuración, almacenamiento y quitarle la iso a la maquina virtual para que no vuelva a abrir el instalador cada vez que la encendamos.

## Instalación de red:

### Verificar conexión:

El paquete Network manager esta instalado pero aun no está activado. Para iniciarlo, ejecutamos `sudo systemctl start NetworkManager.service`. Esto dará de alta el servicio de red. Para que se ejecute automaticamente cada vez que encendamos la pc, ejecutar el mismo comando, cambiando `start` por `enable`.

### Conectarse a internet:



Antes que nada, realizar una prueba mandando una señal a cualquier sitio (ej: google.com) con `ping google.com`. Si recibimos una respuesta, Felicidades! Ya estás conectado a internet. En caso contrario, ejecutar `ip link` para ver dispositivos de red. Prestar atención a la segunda tarjeta que aparezca, esa es la que nos interesa. En mi caso, esta es enp0s3.

![Screenshot 2022-04-10 225518_LI](https://user-images.githubusercontent.com/92989104/162652424-00bb6ed9-78a6-4b69-bc49-06704eae66e9.jpg)

La marca verde señala donde tenemos que mirar para encontrar nuestra placa de red. La marca roja me informa que la misma esta funcionando (UP). Esto debido a que ya la encendí. Si en lugar de "UP" nos muestra "DOWN", ejecutar `ip enp0s3 up`, reemplazando "enp0s3" por el tipo de placa que nos aparezca.

## Instalación de programas y software:

En Linux, la instalación de programas y/o aplicaciones es un tanto distina a la que usualmente usamos en Windows. En Windows, cuando queremos instalar un programa debemos entrar en internet, buscar una pagina web confiable que contenga un link de descarga del instalador, descargarlo, verificar que no se nos quiera instalar nada extra y darle a "siguiente" entre unas 5 o 6 veces.

Por otra parte, en Linux debemos usar el administrador de paquetes que tengamos a mano. Por lo general se usa `apt` en distros como Ubuntu, Debian, etc. Sin embargo, en nuestro caso utilizamos `pacman`. 
Para descargar e instalar un programa, a través de consola ingresamos `pacman -S nombreprograma`. Con ese simple comando ya estamos descargando e instalando el software, sin necesidad de perder mas tiempo (en algunas ocasiones debemos buscar el nombre del programa en internet, a pesar de esto, es un proceso mucho mas fácil que en Windows).

### Aplicaciones básicas:

- Primero y principal, los drivers de la placa gráfica o gráficos que tengamos en nuestra computadora.
  - para AMD: xf86-video-amdgpu
  - para NVIDIA: xf86-video-nouveau

- Tenemos que instalar un gestor de entorno gráfico para poder tener escritorio y ventanas, por mi parte, voy a instalar xorg-server. Además, también vamos a instalar un gestor de sesiones, esto para iniciar sesion con ventana y no desde consola.
  - `sudo pacman -S xorg-server lightdm lightdm-gtk-greeter`
  - Habilitamos lightdm con `systemctl enable lightdm.service`. Luego de la instalación, reiniciamos el sistema con `sudo reboot now`. Una vez inicie,       tendremos disponible lightdm para iniciar sesion y qtile funcionando. 
