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
Есть инструкция на [GitHub][15]

 ![][12]

6. Переходим к настройке сценария на  IFTTT. Естественно Вы уже зарегестрированы на IFTTT, по картинкам настраиваем сценарий.

![][16]

![][17]

![][18]

![][19]

![][20]

![][21]

![][22]

![][23]

![][24]


7. Осталось отреагировать на событие. Создадим скрипт который будет следить за изменением в обьекте  cloud.0.services.ifttt
 
![][25]

{% highlight javascript %}
on({id: 'cloud.0.services.ifttt', change: "ne"}, function (obj) { //Подписуемся на изменение обьекта cloud.0.services.ifttt
  var value = obj.state.val;
  var oldValue = obj.oldState.val;
  if (getState("cloud.0.services.ifttt").val === 'monkey_white') { //и если оно равно monkey_white
    setState("sayit.0.tts.text", 'Получилось');//то наш скрипт говорит в колонку "Получилось"
  }
});
{% endhighlight %}

на блокли это выглядит так 

![][26]


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
[15]: https://github.com/ioBroker/ioBroker.cloud/blob/master/doc/ifttt.md
[16]: /assets/image/salam/ifttt_ap.png
[17]: /assets/image/salam/ifttt_this.png
[18]: /assets/image/salam/ifttt_as.png
[19]: /assets/image/salam/ifttt_tr.png
[20]: /assets/image/salam/ifttt_cpl.png
[21]: /assets/image/salam/ifttt_that.png
[22]: /assets/image/salam/ifttt_that_wh.png
[23]: /assets/image/salam/ifttt_wh_rq.png
[24]: /assets/image/salam/ifttt_wh_cpl.png
[25]: /assets/image/salam/ifttt_obj.png
[26]: /assets/image/salam/ifttt_script.png








