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
introduction: "⚙️ Консольные команды для установки ioBroker"
---
# Установка ioBroker на OC Linux 
### Подготовим систему, установим необходимые пакеты.

>❗️Все команды от **root**❗️  
>Строки **2-4** удалят **Nodejs** ,выполнить если ранее были установленны другие версии **Nodejs**<br>
>Проверить можно командами<br> 
{% highlight bash %}
npm -v
node -v
nodejs -v

{% endhighlight %}

>В **5** строке перезагрузка<br>
>**6** строка выполнить если не установлен **curl**


{% highlight bash %}
1 root@pc2i:#apt-get update && apt-get upgrade
2 root@pc2i:#apt-get --purge remove node
3 root@pc2i:#apt-get --purge remove nodejs
4 root@pc2i:#apt-get autoremove
5 root@pc2i:#reboot
6 root@pc2i:#apt-get install curl
7 root@pc2i:#curl -sL https://deb.nodesource.com/setup_8.x | bash -
8 root@pc2i:#apt-get install -y nodejs
9 root@pc2i:#npm install -g node-gyp
10 root@pc2i:#npm install -g npm@latest

{% endhighlight %}

### Установка ioBroker

{% highlight bash %}
curl -sL https://iobroker.net/install.sh | bash -
{% endhighlight %}
