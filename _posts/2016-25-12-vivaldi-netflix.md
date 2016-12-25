---
layout: post
title: "Ver Netflix en Vivaldi (Elementary OS)"
date: 2016-25-12 14:30:00/
description:
tags: minitutorial, spanish, netflix, vivaldi 
categories: tutorial
comments: true
twitter_text:
---
Un apunte personal de cómo habilitar Netflix en Vivaldi. Encontré un script que saca de Chrome el plugin necesario para poder renderizar video con DRM H264, requisito de **Netflix**. 

Como primer paso instalamos algunos codecs ffmpeg para chromium (la versión open source de Chrome)

```shell
    sudo apt-get install chromium-codecs-ffmpeg-extra
```

Luego, bajamos y ejecutamos el script (hay que revisar el código antes, por lo que hacer el pipe a sudo directamente no es una buena idea, quedan avisados). 

```
    curl https://gist.githubusercontent.com/ruario/3c873d43eb20553d5014bd4d29fe37f1/raw/963e3e932307e20820680b97ef35817243a36a45/latest-widevine.sh | sudo su -
```

Y podremos disfrutar de Netflix en Vivaldi.

![](assets/netflix_vivaldi.png)


