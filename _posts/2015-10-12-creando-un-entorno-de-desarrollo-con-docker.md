---
layout: post
title: "Creando un entorno de desarrollo con docker, ejemplo con Jekyll"
date: 2015-10-12 11:50:08
image: '/assets/img/dockerlogo.png'
description:
tags: minitutorial, spanish, docker, volume
categories: tutorial
twitter_text:
---
La utilización de contenedores dentro de un sistema operativo es una práctica muy utilizada en el mundo de IT, debido a que por un costo marginal casi nulo nos permite de disponer de un entorno controlado en el que los procesos pueden operar sin la necesidad de contar con todo un entorno real, pero creyendo que están en uno. 
Soluciones como Solaris Containers (zonas) o BSD Jails vienen acompañado a los administradores Unix hace mucho tiempo, y en el mundo de los sistemas GNU/Linux hay una tecnología análoga a estas que no está pasando desapercibida, se trata de **docker**.  

La misma nace como un proyecto interno dentro de dotCloud, se libera como Open Source y rápidamente consigue popularidad dentro de github.com, llegando a contar luego con el apoyo de gigantes de la magnitud de **Red Hat**.

Entre sus ventajas tenemos el menor costo computacional a la hora de disponer de un entorno completo contra el costo de una máquina virtual, al aprovechar las tecnologías de aislamiento de recursos y utilizar un solo kernel compartido con su anfitrión; así como también la facilidad de uso frente a lo que supone el construir un sistema completo. 

Como esto es solamente un artículo de un blog, de querer profundizar su estudio se puede leer la ya mucha bibliografía que hay al respecto, y presentaré en este artículo simplemente una receta que me ha sido de utilidad a la hora de disponer de un entorno de pruebas para armar este blog sobre jekyll. 

## Navegar el hub 

La comunidad conformas en torno a docker ha dispuesto de una cantidad de entornos pre configurados de los cuales se puede hacer uso.  Los mismos se pueden encontrar en el hub.

Para el caso que nos interesa, queremos disponer de un entorno jekyll similar al que ofrece github pages, a fin de poder disponer de un entorno de desarrollo análogo al que tendremos en nuestro blog de producción. Podemos realizar la búsqueda en el hub con el comando **docker search**, o bien navegando desde el navegador la página del [hub](https://hub.docker.com/)

    $ docker search jekyll/jekyll
    NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
    grahamc/jekyll                    A basic container containing just Jekyll. ...   41                   [OK]
    jekyll/jekyll                     Official Jekyll Docker Image                    25                   [OK]
    rentabiliweb/jekyll               Docker image of Jekyll.                         4                    [OK]
    whileone/jekyll                                                                   1                    [OK]
    mchelen/jekyll                                                                    1                    [OK]
    troyswanson/jekyll                                                                1                    [OK]
    jouana/guard-jekyll               This docker's image purpose is to use jeky...   1                    [OK]
    froehlichundfrei/jekyll-website   This is the Fröhlich und Frei Docker Imag...    1                    [OK]
    frvi/jekyll                       https://github.com/frvi/dockerfile-jekyll       1                    [OK]
    jfromaniello/jekyll                                                               0                    [OK]
    b7alt/jekyll                      Jekyll & jekyll-import: how to import tumb...   0                    [OK]
    mckoss/jekyll                                                                     0                    [OK]
    thomo/jekyll                      Provides jekyll (http://jekyllrb.com/)          0                    [OK]
    kisenka/centos6-jekyll            CentOS 6.5 + Ruby 2.1.2 + Bundler + Jekyll...   0                    [OK]
    minn/jekyll-gen                   GitLab runner image for building and deplo...   0                    [OK]
    flyingpie/jekyll                                                                  0                    [OK]
    jameswthorne/jekyll               Docker container for one-time runs of jekyll.   0                    [OK]
    schickling/jekyll                 Lightweight jekyll working environment (32...   0                    [OK]
    greencape/jekyll                  This image provides Jekyll together with t...   0                    [OK]
    sthysel/jekyll                                                                    0                    [OK]
    jdecool/jekyll                    Jekyll (static blog engine) in a container      0                    [OK]
    latrokles/jekyll-base             base docker image for jekyll site               0                    [OK]
    fstab/jekyll                                                                      0                    [OK]
    louisjc/jekyll-s3                 Jekyll + S3_website                             0                    [OK]
    ctwise/jekyll                     Jekyll 3.0 beta image                           0                    [OK]


Una vez ubicada la imagen que nos interesa, podemos bajar la misma utilizando el comando **docker pull**

    $ docker pull jekyll/jekyll:pages



## Usando un directorio local en el contenedor

Siendo que el ciclo de vida de los datos de un contenedor es igual que el del contenedor en si mismo, las modificaciones que hacemos en el mismo se perderán cuando detengamos su ejecución. Para poder realizar modificaciones de forma persistente, uno de los mecanismos con los que contamos es el de exponer una carpeta local como si fuera un volumen específico en el docker. 
Para ello utilizaremos el parámetro **volumen**, especificando un directorio local, y el directorias destino con el fue lo veremos en el contenedor, esto nos posibilita por ejemplo exponer la carpeta **/home/usuario/jekyll** con el nombre **/srv/jekyll** en el contenedor.

Para ello utiliamos el comando **docker run**, al que le añadimos el parámetro **--volume=**

    $ docker run --rm --volume=/home/pulpo/srv/prueba:/srv/jekyll -it -p 127.0.0.1:4000:4000 jekyll/jekyll:pages sh
    /srv/jekyll #

Desglosando el mismo un poco, tenemos:

- **run**: orden para que se ejecute el docker especificado
- **rm**: elimina la instancia particular del docker luego que termina su ciclo de vida
- **volume**: como ya se explicó se indica la carpeta a exponer, y luego la carpeta con la que estará disponible en la máquina virtual
- **it**: modo interactivo
- **p**: especifica el socket a utilizar, en este caso machearía el puerto 4000 de loopback con el 4000 del contenedor


De esta forma por ejemplo podemos tener un entorno en el que podemos realizar modificaciones y verlas en el momento en nuestra máquina, y cuando ya tenemos las modificaciones controladas realizar el paso a producción correspondiente. 

Gracias a poder contar con un entorno interactivo, podemos invocar una shell (sh), y crear un nuevo sitio con los comandos de jekyll, por lo tanto, del prompt que devuelve el comando anteriormente ejecutado, podemos utilizar 'jekyll new .' para crear un blog en la locación donde nos deja (que será la ~/srv/prueba de nuestra máquina real, y que se traducirá como el /srv/jekyll en el entorno del contenedor). 

    /srv/jekyll # jekyll new .
    New jekyll site installed in /srv/jekyll. 
    /srv/jekyll # 

Luego arrancamos el servidor, para que sirva la página creada

    /srv/jekyll # jekyll s
    Configuration file: /srv/jekyll/_config.yml
                Source: /srv/jekyll
           Destination: /srv/jekyll/_site
          Generating... 
                        done.
     Auto-regeneration: enabled for '/srv/jekyll'
    Configuration file: /srv/jekyll/_config.yml
        Server address: http://0.0.0.0:4000/
      Server running... press ctrl-c to stop.

Y desde el navegador, ya podemos probar acceder al sitio creado:

![](/assets/img/prueba_jekyll.png) 


Podremos por lo tanto crear los post que queramos en **~/srv/prueba/_posts**

    $ cd ~/srv/prueba  
    $ vim vim 2015-10-17-prueba.markdown

Y en el archivo creamos un post de prueba:


    ---
    layout: post
    title:  "Prueba"
    date:   2015-10-17 16:00:00
    categories: jekyll update
    ---
    #Prueba


    Esto es un post de prueba.

    Saludos!


Que veremos en el listado de posts:

![](/assets/img/prueba_jekyll2.png) 


Y al cual podremos acceder y visualizarlo:

![](/assets/img/prueba_jekyll3.png) 


Espero que le sea de utilidad a alguien.
