---
layout: post
title: "Install ioBroker  "
date:   2018-05-22
description: "Консольные команды для установки ioBroker"
categories: ioBroker
main-class: 'iobroker'
color: '#EB7728'
tags:
- iobroker
- iob
- иоброкер
- иоб
- Linux 
excerpt:
introduction: "💲 Консольные команды для установки ioBroker"
---
# Установка ioBroker на OC Linux 
### Подготовим систему, установим необходимые пакеты.

>❗️Все команды от **root**❗️  
>Строки **2-4** удалят **Nodejs** ,выполнить если ранее были установленны другие версии **Nodejs**
>В **5** строке перезагрузка
>**6** строка выполнить если не установлен **curl**


{% highlight bash %}
root@pc2i:#apt-get update && apt-get upgrade
root@pc2i:#apt-get --purge remove node
root@pc2i:#apt-get --purge remove nodejs
root@pc2i:#apt-get autoremove
root@pc2i:#reboot
root@pc2i:#apt-get install curl
root@pc2i:#curl -sL https://deb.nodesource.com/setup_8.x | bash -
root@pc2i:#apt-get install -y nodejs
root@pc2i:#apt-get install git-core libnss-mdns libavahi-compat-libdnssd-dev -y
root@pc2i:#apt-get install build-essential libpcap-dev
root@pc2i:#npm install -g node-gyp
root@pc2i:#npm install -g npm@latest

{% endhighlight %}

### Установка ioBroker

{% highlight bash %}
cd /opt
mkdir iobroker
cd iobroker
npm install iobroker --unsafe-perm
init 6
{% endhighlight %}
