---
layout: post
title: "Интегрируем Google Asistant в IoBroker "
date:   2018-11-13
description: "IoBroker + Google Asistant + IFTTT"
categories: ioBroker
main-class: 'iobroker'
color: '#EB7728'
tags:
- iobroker
- iob
- иоброкер
- иоб
- Google Asistant 
excerpt:
introduction: "🎤 Голосовое управление через IFTTT"
---
![][4]
# Преамбула.
Привет Всем!

Появился в моем "без"Умном Доме новый гаджет, [колонка][8] со встроеным Гугл Асистентом.

Ну и сразу вопрос: "Как интегрировать голосового помошника в УД ?"
У меня стоит IoBroker, как установить, настроить можно посмотреть в [статье][1], или в доках на [офсайте][2].
Будем считать что у вас уже есть настроеный ИоБрокер.

# Шаг за шагом.

1. Первое что нужно сделать это добавить колонку к вашему Гугл аккаунту.
   Для этого в приложении [Google Home][3] для андроида, добавим наше устройство.
   
2. Установим одноименный драйвер для ИоБ - **Chromecast** .
Жмем на плюсик.
 ![][5]
 > ❗️Примечание: Если тапнуть на знак вопроса , рядом с плюсиком то попадаем на файл Readme. Описание драйвера на GitHub.
 
3. Настройки драйвера.
После успешной установки мы попадаем на вкладку настройки драйвера.
Устанавливаем значения и жмем сохранить.

 ![][13]
 
4.Установим драйвер **Sayit**
 ![][6]

После успешной установки мы попадаем на вкладку настройки драйвера.
Должно появится наше устройство. Устанавливаем значения и жмем сохранить. 

 ![][9]

Теперь проверим будет ли разговаривать наша колонка. Для этого в обьектах драйвера Sayit заполнить значение,
например **Привет мир** и нажать галочку. Колонка должна сказать - **Привет мир**

![][14]

5.Установим драйвер **Cloud**. Если еще не установлен.

 ![][7]
 
После успешной установки мы попадаем на вкладку настройки драйвера.
В настройках драйвера на вкладке **Настройки** необходимо указать **APP-KEY**, если его нет тапнуть на адресс указаный стрелкой
пройти регистрацию и получить.

 ![][10]
 
На вкладке **СЕРВИСЫ И IFTTT**  указать **IFTTT key** и УРЛ для IFTTT


 ![][11]

**IFTTT key** находится по ссылке https://ifttt.com/maker_webhooks , тапнув на **Документация** .

 ![][12]


to be continued...

[1]: https://sprut.ai/client/article/274
[2]: http://www.iobroker.net/docu/?page_id=2630&lang=ru
[3]: https://play.google.com/store/apps/details?id=com.google.android.apps.chromecast.app
[4]: /assets/image/salam/zolo.png
[5]: /assets/image/salam/cast.png
[6]: /assets/image/salam/sayit.png
[7]: /assets/image/salam/cloud.png
[8]: https://zoloaudio.com/pages/mojo
[9]: /assets/image/salam/sayit_i.png
[10]: /assets/image/salam/cloud_i.png
[11]: /assets/image/salam/cloud_ifttt.png
[12]: /assets/image/salam/ifttt_wh.png
[13]: /assets/image/salam/chrom_i.png
[14]: /assets/image/salam/sayit_obj.png
