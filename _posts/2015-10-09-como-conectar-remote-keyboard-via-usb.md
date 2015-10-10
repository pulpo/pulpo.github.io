---
layout: post
title: "Como conectar Remote Keyboard via USB"
date: 2015-10-09 08:07:22
image: '/assets/img/remotekeyboard.png'
description:
tags: adb android spanish remotekeyboard
categories: tips
twitter_text:
---

Desde que descubrí RemoteKeyboard, tanto mi tablet como mi teléfono han cobrado una nueva vida. Ahora son compañeros en mi día a día, dándome una pantalla adicional para por ejemplo seguir mis conversaciones de IM, poder seguir lo que pasa en Slack, responder correos, mientras en mi monitor principal estoy con la terminal centrado en el trabajo propiamente dicho. 

Para ello, encontré un teclado en android que se comporta muy bien, [Remote Keyboard](https://play.google.com/store/apps/details?id=de.onyxbits.remotekeyboard), el mismo permite que nos podamos conectar a un socket (escucha vía red en el puerto 2323), y en el caso que no tengamos red wifi disponible, también podemos hacer la conexión via **adb** con el usb. 

## Para realizar la conexión via Wifi

1. Instalamos la aplicación en el teléfono
2. La abrimos, habilitamos el teclado, cuando nos muestra que levantó el socket nos dice la IP y el puerto en el que se encuentra
 ![](https://lh3.ggpht.com/tEW1thjWXlgHw5cine_DHsPVfyP-nZNJaPIXEapQjvQDj-RLAHJmEFeSaKZMaOrHCTQ=h900) 
3. Luego nos conectamos desde la terminal de nuestro sistema operativo favorito mediante telnet
 ![](https://lh3.ggpht.com/7pSqMTjZYMH-uVn4-jkWYwd55YDTFVKFzQCBm-req0bMdPXjMyStF8mr0ElN8PiPHg=h900) 
 **Tip** Utilizando por ejemplo guake, o yakuake, podemos dejar una terminal abierta y conectada a remotekeyboard, a la que podemos invocar con F12 cada vez que necesitemos escribir algo en nuestro dispositivo android. 


## Para realizar la conexión via ADB

Si nos encontramos en un ambiente donde no tenemos WiFi, o en el cual no podemos conectar todos nuestros dispositivos a la misma red WiFi (por ejemplo en la oficina), también tenemos la posiblidad de conectarnos a Remote Keyboard mediante USB. 

Para ello necesitamos:

- Tener instalado adb en nuestro sistema operativo, en Ubuntu y derivados sería un simple ```sudo apt-get install android-tools-adb```
- Tener habilitado el modo depuración USB de Android (desde las opciones para desarrolladores). 

Luego conectamos mediante USB el dispositivo con Android, e iniciamos el servicio de adb:

    ╭─pulpo@wendy-reloaded  ~  
    ╰─$ sudo adb start-server
    * daemon not running. starting it now on port 5037 *
    * daemon started successfully *

Hacemos **forward** del puerto en el que esté escuchando RemoteKeyboard, en este caso, **2323** (el default)

    ╭─pulpo@wendy-reloaded  ~  
    ╰─$ adb forward tcp:2323 tcp:2323

Por último seremos capaces de conectarnos vía telnet a través de la placa de loopback de nuestra máquina, que redireccionará por adb la conexión mediante el usb al terminal android. 

    ╭─pulpo@wendy-reloaded  ~  
    ╰─$ telnet localhost 2323        
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    Android Remote Keyboard                                                                                                                                                           
        
                 Welcome to your Android keyboard. Just type
        (\_/)     in this window to have key presses forwarded 
        (^_^)      to your device (there will be no local echo).
        (___)O      If characters turn into garbage, check if your 
                     client is configured to send them UTF-8 encoded.


        CTRL-C      Copy
        CTRL-V      Paste
        CTRL-X      Cut
        CTRL-A      Select All
        CTRL-L      Send action  
        CTRL-S      Search key
        CTRL-Q      Back key
        CTRL-D      Disconnect from device
        CTRL-R      (Un-)Scramble text using ROT13
        INSERT      Substitute current word with a replacement.



