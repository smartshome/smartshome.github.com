---
layout: post
title: "Оповещения для Умного Дома на ioBroker через SMS"
date:   2018-12-26
description: "ioBroker — отправляем SMS скриптом. "
categories: iobroker
main-class: 'iobroker'
color: '#EB7728'
tags:
- iobroker
- iob
- иоброкер
- иоб
- script
- jscript 
excerpt:
introduction: "📝-  инструкция отправки SMS скриптом. "
---

## ioBroker — отправляем SMS скриптом через SMS.RU. 
### Регистрируемся на SMS.RU.


Отправлять будем, используя сервис [SMS.RU][1]. Данный сервис позволяет отправлять до 5 сообщений в день, а на свой номер бесплатно, а на номера других операторов тарифы от ~20 коп. до ~2 руб., что весьма бюджетно.

Итак, регистрируемся по [ссылке][1] (эта ссылка партнерская, получаем по ней 10 руб. на счет и 20% скидки), идем в раздел "Программистам", копируем свой api_id:

![][2]

И вставляем вместо **"ВАШ_ID_ПОЛУЧЕННЫЙ_НА_САЙТЕ"** скрипта IOBROKER:

### Сам скрипт:


{% highlight javascript %}
var request = require('request');
var my_API_ID = 'ВАШ_ID_ПОЛУЧЕННЫЙ_НА_САЙТЕ';

createState('SMS.status',0);
createState('SMS.status_code',0);
createState('SMS.numbers',0);
createState('SMS.message',0);
createState('SMS.balance',0);


function sendSMS() {
    // log('Link: ' + link);
    request('https://sms.ru/sms/send?partner_id=235959&api_id=' + my_API_ID + '&to=' + getState("javascript.0.SMS.numbers").val + '&msg=' + encodeURIComponent(getState("javascript.0.SMS.message").val) + '&json=1', cb(function(error, response, body) {
        if(error) log('Problem with request: ' + error, 'error');
        else {
            var jsonContent = JSON.parse(body);
            // log('Body: ' + body);
            // log('Status: ' + jsonContent.status);
            // log('Status_code: ' + jsonContent.status_code);
            // log('Balance: ' + jsonContent.balance);
            setState("javascript.0.SMS.status", jsonContent.status, true);
            setState("javascript.0.SMS.status_code", jsonContent.status_code, true);
            setState("javascript.0.SMS.balance", jsonContent.balance, true);
            }
    }));
}

// schedule("* * * * *", sendSMS);
{% endhighlight %}

[1]: http://smart2promo.sms.ru/
[2]: /assets/image/pooh063/smsru.png
[3]: /assets/image/pooh063/
[4]: /assets/image/pooh063/
[5]: /assets/image/pooh063/
