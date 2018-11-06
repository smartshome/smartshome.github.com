---
layout: post
title: "Установка ioBroker на ОС Linux "
date:   2018-11-06
description: "Установка ioBroker на ОС Linux"
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
introduction: "❗️ Установка ioBroker на ОС Linux"
---
![][3]
# Установка ioBroker на  OC Linux
**Всем привет!**
>У многих возникает проблема с первой установкой ioBroker на ПК/Одноплатник (она-же Raspberry/Малина к ним-же относятся оранджы,
 бананы и т.д.))) под операционной системой Linux. В этой статье пройдём по основным пунктам которые будут актуальны практически
 в любой сборке Линукс
 
>Установку из под Windows в этой статье рассматривать не буду так-как Windows операционка сама по себе очень прожорливая,
а нам дороги каждый МБ оперативнной памяти, конечно ioBroker на Windows имеет место быть если у вас позволяет железо,
а с Linux ну прям совсем туго.

>И так, устанавливаем на ПК/Лептоп/Малинку понравившуюся сборку Линукс или используем уже имеющуюся.
 Для ПК/Лептоп я бы посоветовал (и дальше в статье её буду использовать) сборку [Linux Mint 19][1]

Для Малинки (или другого одноплатника)  [DietPi][2]  но это не принципиально ?
Саму установку ОС опустим, она расписанна уже сотни раз

## Начнём :)

**У НАС УСТАНОВЛЕН ЛИНУКС И ИМЕЕТСЯ ДОСТУП К СИСТЕМЕ ПО SSH**

Первым делом приведём нашу ОС в актуальное состояние

{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

ожидаем завершения, на одноплатниках это может занять определённое колличество времени.
Удаляем если уже была в системе установленна ранее или прилогалась в сборке ОС ноду.
Просто строку за строкой отрабатываем следущие команды
Входим с правами root (или перед всеми командами добавляем команду sudo)

{% highlight bash %}
sudo su
apt-get --purge remove node
apt-get --purge remove nodejs
apt-get autoremove
reboot
{% endhighlight %}

Устанавливаем ноду 8 и несколько необходимых пакетов

{% highlight bash %}
sudo su
apt-get install curl
curl -sL https://deb.nodesource.com/setup_8.x | bash -
apt-get install -y nodejs
apt-get install git-core libnss-mdns libavahi-compat-libdnssd-dev -y
apt-get install -y libudev-dev libpam0g-dev
apt-get install build-essential libpcap-dev -y
npm install -g node-gyp
npm install -g npm@latest
{% endhighlight %}

Проверяем всё ли правильно установилось командами

{% highlight bash %}
node -v
npm -v
{% endhighlight %}

должны увидеть вывод примерно как на скрине ниже (версии актуальны на 1.11.2018)

![][4]

Выходим из root командой exit

Установим serialport, он может нам пригодится в будущем кто использует стики zigbee, z-wave и т.д.
{% highlight bash %}
sudo npm install -g serialport --unsafe-perm
{% endhighlight %}

Командой serialport-list выводим имеющиеся сериальные порты и что в них установлено

{% highlight bash %}
serialport-list
{% endhighlight %}

![][5]


**Подготовка к установке ioBroker закончена, переходим непосредственно к самой установке ioBroker**
 (все команды отрабатываем построчно)
 
 {% highlight bash %}
sudo su
cd /opt
mkdir iobroker
cd iobroker
npm install iobroker --unsafe-perm
{% endhighlight %}



**Готово❗️**

Запоминаем что всё что делаем с ioBroker делаем из директории /opt/iobroker

Соответственно переход у нас такой

{% highlight bash %}
cd  /opt/iobroker
{% endhighlight %}

Базовые команды

{% highlight bash %}
sudo iobroker start
sudo iobroker stop
sudo iobroker restart
{% endhighlight %}

Больше узнать о доступных командах можем командой 

{% highlight bash %}
iobroker -h
{% endhighlight %}

Командой iobroker start  запускаем iobroker и он должен будет доступен по адресу из браузера
{% highlight html %}
http://ип_вашей_системы:8081
{% endhighlight %}

![][6]

Небольшой обзор админки мы проведём в слеующей статье.

Удачи!

[1]: https://linuxmint.com/
[2]: https://dietpi.com/
[3]: /assets/image/intro_iob.png
[4]: /assets/image/node_v.png
[5]: /assets/image/serialport.png
[6]: /assets/image/admin_st.png

