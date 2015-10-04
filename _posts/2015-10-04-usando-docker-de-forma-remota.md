---
layout: post
title: "Usando docker de forma remota"
date: 2015-10-04 15:06:32
image: '/assets/img/'
description:
tags: tips, docker, spanish
categories: tips
twitter_text:
---

## Conectando a un docker remoto (http)

Bajamos el servicio de docker que esté en ejecución en el **servidor**
        
        sudo service docker.io stop

Iniciamos el servicio en el **servidor** , exponiendo el sock

        docker -H tcp://0.0.0.0.0:5000 -H unix:///var/run/docker.sock -d &

Ahora desde el **cliente**, ejecutamos, y suponiendo que el servidor tenga la IP 192.168.0.30

        sudo docker -H 192.168.0.30:5000 ps

