# Raspberry3bplus-3.5inch-displayA-ILI9486-MPI3501-XPT2046
Error pantalla con _ parpadeante. Solucion. SO 64bits.

Esto fue testeado en la version Bookworm y Bullseye tanto de 32 como de 64 bits.
El error se repite en otros tipos de pantalla pero el procedimiento seria el mismo.

Si te gustaria ver un video sobre como lo solucione da click aca:

De clonar este repositorio recordar que solo viene preparado para la version de pantalla que se encuentra en el titulo.

Voy a dejar mas informacion respecto de todo el proceso al final del post.


# Requisitos

Montar la imagen de Raspberry pi os (ex raspbian) con Pinn Multiboot, montando una imagen o usando la herramienta de <a href="https://www.raspberrypi.com/software/">Raspberry Software<a> 

Conexion por hdmi, un teclado y mouse para primer inicio del OS. (Despues lo pueden usar remoto o via ssh)

Contar con conexion a internet en su Raspberry Pi.

Tener habilitado SSH y .

Yo utilizo <a href="https://www.putty.org/">Putty</a> y <a href="https://www.realvnc.com/es/connect/download/viewer/">VNC Viewer</a> para mayor comododidad.
Tambien les dejo un teclado en pantalla por si necesitan.

sudo apt-get install update
sudo apt-get install matchbox-keyboard
sudo nano /usr/bin/toggle-matchbox-keyboard.sh


# Comandos

sudo rm -rf LCD-show
git clone https://github.com/goodtft/LCD-show.git
chmod -R 755 LCD-show

Antes de continuar no se olviden de tener activa la coneccion por ssh ya que al hacer esto la interface de nuestro OS no se volvera a mostrar hasta que hayamos configurado todo lo posterior.
En caso de no ser posible una conexion mediante ssh, despues de hacer el siguiente paso podrian poner la memoria en su sistema operativo y descomentar la linea dtoverlay=vc4-kms-v3d para volver a ver la pantalla HDMI. (Hay que recordar volver a comentarlo una vez se solucione el problema de la pantalla de 3.5)

cd LCD-show/
sudo ./LCD35-show

Ahora si nos conectamos por ssh y ejecutamos el siguiente comando:

sudo systemctl disable glamor-test.service
sudo rm /usr/share/X11/xorg.conf.d/20-noglamor.conf
sudo systemctl restart lightdm

Con esto detenemos el proceso de Glamor que genera que en dispositivos de hasta un 1gb de ram se detengan procesos, concretamente uno de los processos afectados es el que corresponde con el controlador de pantalla. Por eso al desactivarlo la pantalla inicia normalmente.
Recorda que dtoverlay=vc4-kms-v3d debe estar comentado en /boot/config.txt.

Para cambiar la resolucion ejecutamos:

sudo nano /boot/config.txt

al final de todo este documento agregamos cualquiera de estas resoluciones y la descomentamos.

#framebuffer_width=720
#framebuffer_height=480

#framebuffer_width=960
#framebuffer_height=640

#framebuffer_width=1440
#framebuffer_height=960

#framebuffer_width=1920
#framebuffer_height=1280

Yo particularmente uso:

framebuffer_width=960
framebuffer_height=640

Para guaradar el documento apretamos Control (cmd) + O y luego Control + X

Deje varias para que elijan la que necesiten.
Recuerden que para que los cambios hagan efecto tiene que hacer un:

sudo reboot

Para calibrar la pantalla tactil:

sudo apt-get install xinput-calibrator 

sudo nano /etc/X11/xorg.conf.d/99-calibration.conf

# Creditos

Toda la documentacion relacionada la pueden encontrar en:
https://github.com/goodtft/LCD-show/

Los overlays de este repositorio fueron obtenidos de dicho directorio.

Si tenes una pantalla Waveshare:
https://github.com/waveshare/LCD-show.git

