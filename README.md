# RaspberryPi3bplus-3.5inch-displayA-ILI9486-MPI3501-XPT2046
Error pantalla con _ parpadeante. Solución. SO 64bits y 32bits.

Esto fue testeado en la versión Bookworm y Bullseye tanto de 32 como de 64 bits.
El error se repite en otros tipos de pantalla, pero el procedimiento seria el mismo.

<div align="center">

<h3>Si te gustaría ver un video sobre como lo solucione da click acá:</h3>

<a href="https://youtu.be/3FgEISmO2sI"><img alt="Tutorial" src="Raspberry.png" width="500" /></a>
<br>
***No te olvides de suscribirte y dar me gusta.***

</div>

Si desean clonar este repositorio que solo viene preparado para la versión de pantalla que se encuentra en el titulo pueden reemplazar el primer comando por:

```bash
sudo rm -rf LCD-show
git clone https://github.com/Shinigamy19/RaspberryPi3bplus-3.5inch-displayA-ILI9486-MPI3501-XPT2046
chmod -R 755 LCD-show
cd RaspberryPi3bplus-3.5inch-displayA-ILI9486-MPI3501-XPT2046/
```

Aunque este sea más ligero ya que solo tiene este controlador, recomiendo usar el oficial.

> Voy a dejar más información respecto de todo el proceso al final del post.

# Requisitos

- Montar la imagen de Raspberry pi os (ex raspbian) con Pinn Multiboot, montando una imagen o usando la herramienta de <a href="https://www.raspberrypi.com/software/">Raspberry Software</a> 

- Conexión por hdmi, un teclado y mouse para primer inicio del OS. (Después lo pueden usar remoto o vía ssh)

- Contar con conexión a internet en su Raspberry Pi.

- Tener habilitado SSH y SPI.

Yo utilizo <a href="https://www.putty.org/">Putty</a> y <a href="https://www.realvnc.com/es/connect/download/viewer/">VNC Viewer</a> para mayor comodidad.
También les dejo un teclado en pantalla por si necesitan.

```bash
sudo apt-get install update
sudo apt-get install matchbox-keyboard
```

# Comandos

```bash
sudo rm -rf LCD-show
git clone https://github.com/goodtft/LCD-show.git
chmod -R 755 LCD-show
```

Antes de continuar no se olviden de tener activa la conexión por ssh ya que al hacer esto la interface de nuestro OS no se volverá a mostrar hasta que hayamos configurado todo lo posterior.
En caso de no ser posible una conexión mediante ssh, después de hacer el siguiente paso podrían poner la memoria en su sistema operativo y des comentar la línea `dtoverlay=vc4-kms-v3d` para volver a ver la pantalla HDMI. (Hay que recordar volver a comentarlo una vez se solucione el problema de la pantalla de 3.5)

```bash
cd LCD-show/
sudo ./LCD35-show
```

Ahora si nos conectamos por ssh y ejecutamos el siguiente comando:

```bash
sudo systemctl disable glamor-test.service
sudo rm /usr/share/X11/xorg.conf.d/20-noglamor.conf
sudo systemctl restart lightdm
```

Con esto detenemos el proceso de Glamor que genera que en dispositivos de hasta  1gb de ram se detengan procesos, concretamente uno de los afectados es el que corresponde con el controlador de pantalla. Por eso al desactivarlo la pantalla inicia normalmente.
> Mas información al final del post.

Recorda que `dtoverlay=vc4-kms-v3d` debe estar comentado en /boot/config.txt.

Para cambiar la resolución ejecutamos:

```bash
sudo nano /boot/config.txt
```

al final de todo este documento agregamos cualquiera de estas resoluciones y la des comentamos.

```bash
#framebuffer_width=720
#framebuffer_height=480

#framebuffer_width=960
#framebuffer_height=640

#framebuffer_width=1440
#framebuffer_height=960

#framebuffer_width=1920
#framebuffer_height=1280
```

Yo particularmente uso:

```bash
framebuffer_width=960
framebuffer_height=640
```

Para guardar el documento apretamos Control (cmd) + O y luego Control + X

Deje varias resoluciones para que elijan la que necesiten.
Recuerden que para que los cambios hagan efecto tiene que hacer un:

```bash
sudo reboot
```

Para calibrar la pantalla tactil:

```bash
sudo apt-get install xinput-calibrator 

sudo nano /etc/X11/xorg.conf.d/99-calibration.conf
```

Si necesitan rotar la pantalla:

```bash
cd LCD-show/
sudo ./rotate.sh 90
```

con mi repo:

```bash
cd RaspberryPi3bplus-3.5inch-displayA-ILI9486-MPI3501-XPT2046/
sudo ./rotate.sh 90
```

Y eso sería todo lo que hay que hacer.

Si por alguna razón ya no quieren usar su pantalla de 3.5 pulgadas pueden ejecutar el siguiente comando y funcionara con el hdmi.

```bash
chmod -R 755 LCD-show
cd LCD-show/
sudo ./LCD-hdmi
```

Si estas usando este repo:
```bash
chmod -R 755 LCD-show
cd RaspberryPi3bplus-3.5inch-displayA-ILI9486-MPI3501-XPT2046/
sudo ./LCD-hdmi
```

# Queres apoyarme:

<h3 align="center">Mis redes sociales:</h3>
<p align="center">
<a href="https://www.youtube.com/c/shinigamy19" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/youtube.svg" alt="shinigamy19" height="35" width="35" title="Mi canal de Youtube" /></a>
<a href="https://twitch.tv/shinigamy_19" target="blank"><img align="center" src="https://img.icons8.com/external-justicon-flat-justicon/64/external-twitch-social-media-justicon-flat-justicon.png" alt="px9fcpbp3T" height="30" width="30" title="Mi canal de Twitch"/></a>
<a href="https://kick.com/shinigamy19" target="blank"><img align="center" src="https://img.freepik.com/premium-vector/kick-logo-vector-download-kick-streaming-icon-logo-vector-eps_691560-10814.jpg" alt="px9fcpbp3T" height="30" width="30" title="Mi canal de Kick"/></a>
<a href="https://discord.gg/px9fcpbp3T" target="blank"><img align="center" src="https://images-eds-ssl.xboxlive.com/image?url=Q_rwcVSTCIytJ0KOzcjWTYl.n38D8jlKWXJx7NRJmQKBAEDCgtTAQ0JS02UoaiwRCHTTX1RAopljdoYpOaNfVf5nBNvbwGfyR5n4DAs0DsOwxSO9puiT_GgKqinHT8HsW8VYeiiuU1IG3jY69EhnsQ--&format=source&w=120" alt="px9fcpbp3T" title="Mi Server de Discord" height="30" width="30" style="border-radius: 4px 4px 4px 4px"  /></a>
<a href="https://instagram.com/shinigamy19_art" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Instagram_logo_2016.svg/2048px-Instagram_logo_2016.svg.png" alt="shinigamy19_art" title="Mi instagram de Artista" height="30" width="30" /></a>
<a href="https://instagram.com/shinigamy19" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Instagram_logo_2016.svg/2048px-Instagram_logo_2016.svg.png" title="Mi Intagram Personal" alt="shinigamy19" height="30" width="30" /></a>
<a href="https://www.tiktok.com/@shinigamy_19" target="blank"><img align="center" src="https://cdn.pixabay.com/photo/2021/01/30/06/42/tiktok-5962992_1280.png" alt="shinigamy19" title="Mi Tiktok" height="30" width="30" style="border-radius: 4px 4px 4px 4px" /></a>
<a href="https://linkedin.com/in/eros-benitez" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/LinkedIn_logo_initials.png/640px-LinkedIn_logo_initials.png" alt="eros-benitez" title="Mi LinkedIn" height="30" width="30" style="border-radius: 4px 4px 4px 4px" /></a>
<a href="https://www.behance.net/shinigamy19" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/behance.svg" alt="shinigamy19" title="Mi Behance" height="30" width="30" /></a>
<a href="https://shinigamy19.itch.io/" target="blank"><img align="center" src="https://static.itch.io/images/app-icon.svg" alt="shinigamy19" title="Mi perfil de Itch" height="30" width="30" style="border-radius: 4px 4px 4px 4px" /></a>
<a href="https://fb.com/shinigamy19" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/facebook.svg" alt="shinigamy19" title="Mi facebook" height="30" width="30" /></a>
<a href="mailto:erosbenitezd@gmail.com" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Gmail_Icon_%282013-2020%29.svg/2560px-Gmail_Icon_%282013-2020%29.svg.png" alt="shinigamy19" title="Mi Mail" height="30" width="35" /></a>
</p>

<div>
<h3 align="center">Donaciones:</h3>
<p align="center">
<a href="https://ceneka.net/mp/d/shinigamy19" target="blank"><img align="center" src="https://seeklogo.com/images/M/mercado-pago-logo-52B7182205-seeklogo.com.png?v=638388567080000000" alt="shinigamy19" height="35" width="35" title="Donaciones Por Mercado Pago" /></a>
<a href="https://www.paypal.me/shinigamy19" target="blank"><img align="center" src="https://upload.wikimedia.org/wikipedia/commons/a/a4/Paypal_2014_logo.png" alt="px9fcpbp3T" height="30" width="30" title="Donaciones Por PayPal"/></a>
<a href="https://www.patreon.com/shinigamy19" target="blank"><img align="center" src="https://cdn.icon-icons.com/icons2/2429/PNG/512/patreon_logo_icon_147253.png" alt="px9fcpbp3T" height="30" width="30" title="Donaciones Por Patreon"/></a>

</p>
<p align="center">
<a href='https://cafecito.app/shinigamy19' rel='noopener' target='_blank'><img srcset='https://cdn.cafecito.app/imgs/buttons/button_6.png 1x, https://cdn.cafecito.app/imgs/buttons/button_6_2x.png 2x, https://cdn.cafecito.app/imgs/buttons/button_6_3.75x.png 3.75x' src='https://cdn.cafecito.app/imgs/buttons/button_6.png' alt='Invitame un café en cafecito.app' title="Donaciones Por Cafecito"/></a></p>
</div>

# Creditos

Toda la documentación relacionada la pueden encontrar en:
https://github.com/goodtft/LCD-show/

Los overlays de este repositorio fueron obtenidos de dicho directorio.

Si tenes una pantalla Waveshare:
https://github.com/waveshare/LCD-show.git

También les dejo este pequeño foro de donde saque la información del proceso Glamor.
https://github.com/goodtft/LCD-show/issues/369


