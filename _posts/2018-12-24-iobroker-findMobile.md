---
layout: post
title: "Расчёт расстояния до мобильного телефона "
date:   2018-12-24
description: "ioBroker Find my Mobile"
categories: ioBroker
main-class: 'iobroker'
color: '#EB7728'
tags:
- iobroker
- iob
- иоброкер
- иоб
- script 
excerpt:
introduction: "📞Скрипт, который получает координаты с мобилы"
---

# ioBroker Find my Mobile
**Скрипт расчёта расстояния до мобильного телефона**
>Сделано на основании информации  [отсюда][1] и уважаемого автора Александра @cahek2202 Скрипт, который получает координаты с мобилы и на основании координат считает расстояния до заданных точек. И потом реагирует как нужно.

Расстояние с гугл/яндекс карт не совпадает с расчётным. хз как на картах считается расстояние. Имейте ввиду.



{% highlight javascript %}
//данный блок лучше убрать в global секцию --- начало блока
//Координаты дома и важных мест
var long_дом   =xx.xxxxxxxxxxxxxx;
var alt_дом    =xx.xxxxxxxxxxxxxx;
var long_офис  =xx.xxxxxxxxxxxxxx;
var alt_офис   =xx.xxxxxxxxxxxxxx;
var long_школа =xx.xxxxxxxxxxxxxx;
var alt_школа  =xx.xxxxxxxxxxxxxx;
//Мобила один - расстояния до объектов – ID 43a43eff узнаётся после получения первой посылки с мобилы
createState('javascript.0.GPSLogger.43a43eff.доДома');
createState('javascript.0.GPSLogger.43a43eff.доОфиса');
createState('javascript.0.GPSLogger.43a43eff.доШколы');
//Мобила два - расстояния до объектов – ID 47fff2aa узнаётся после получения первой посылки с мобилы
createState('javascript.0.GPSLogger.47fff2aa.доДома');
createState('javascript.0.GPSLogger.47fff2aa.доОфиса');
createState('javascript.0.GPSLogger.47fff2aa.доШколы');
//Переменные в MQTT для работы скриптов автовключения света при приближении к дому
createState('javascript.0.MyHome.ЛюдиДома'); //флаг присутствия людей дома
createState('javascript.0.MyHome.ProjON');   //флаг включения прожекторов
//данный блок лучше убрать в global секцию --- конец блока



//Сам скрипт
//Скрипт получения координат с мобильнх телефонов

//слушаем порт 8090 и получаем с него по POST запросу данные
var http = require('http');
var server = http.createServer().listen(8090);
var userA;

server.on('request', function (req, res) {
    res.writeHead(200);
    if (req.method == 'POST') {
        var body = '';
    }

    req.on('data', function (data) {
        body += data;
        console.log(body);
    });

    req.on('end', function () {
        temp = body.split('&');
        var obj = {};
        res.write('hi');
        res.end();
        for (var key in temp) {
            temp2 = temp[key].split('=');
            var objkey = temp2[0];
            var objval = temp2[1];
            obj[objkey] = objval;
        }

        if (getState('javascript.0.GPSLogger.' + obj.deviceid + '.deviceid').val) {
            for (var param in obj) {
            setState('javascript.0.GPSLogger.' + obj.deviceid + '.' + param, obj[param], true);
            }
        } else {
        for (param in obj) {
            createState('GPSLogger.' + obj.deviceid + '.' + param, obj[param]);
            }
        }
    });
});

//Реагируем на изменение координат всех мобил, которые шлют данные
on({id: /^javascript\.0\.GPSLogger.*\.latitude$/, change: "any"}, function (obj) {
  var value = obj.state.val;
  var oldValue = obj.oldState.val;

var numberA = obj.id.substring(31, 23);

//Блок необходимый для places. Если places не используется, блок можно удалить.
//получаем ID мобилы
//соотносим имя из plces.0 и ID мобилы из скрипта
if      (numberA == '43a43eff') { userA = 'ИмяЧеловека1'; }
else if (numberA == '47fff2aa') { userA = 'ИмяЧеловека2'; }
//Записываем данные в объекты places        
sendTo("places.0",  {
    user:       userA, 
    latitude:   value,
    longitude:  getState('javascript.0.GPSLogger.' + numberA + '.longitude').val,
    timestamp:  getDateObject(getState('javascript.0.GPSLogger.' + numberA + '.latitude').ts).getTime()
});
//Конец блока для places

Сергей Фролов, [24.12.18 00:35]
//Берём записанные в mqtt свежие координаты
var long_target = getState('javascript.0.GPSLogger.' + numberA + '.latitude').val;
var alt_target  = getState('javascript.0.GPSLogger.' + numberA + '.longitude').val;
//Cчитаем расстояние в метрах от мобилы до каждого объекта. Записываем в mqtt
setState('javascript.0.GPSLogger.' + numberA + '.доДома',  (6371*1000*Math.acos(Math.sin(Math.PI*alt_дом/180)*Math.sin(Math.PI*alt_target/180)+Math.cos(Math.PI*alt_дом/180)*Math.cos(Math.PI*alt_target/180)*Math.cos(Math.PI*(long_дом-long_target)/180))).toFixed());
setState('javascript.0.GPSLogger.' + numberA + '.доОфиса', (6371*1000*Math.acos(Math.sin(Math.PI*alt_офис/180)*Math.sin(Math.PI*alt_target/180)+Math.cos(Math.PI*alt_офис/180)*Math.cos(Math.PI*alt_target/180)*Math.cos(Math.PI*(long_офис-long_target)/180))).toFixed());
setState('javascript.0.GPSLogger.' + numberA + '.доШколы', (6371*1000*Math.acos(Math.sin(Math.PI*alt_школа/180)*Math.sin(Math.PI*alt_target/180)+Math.cos(Math.PI*alt_школа/180)*Math.cos(Math.PI*alt_target/180)*Math.cos(Math.PI*(long_школа-long_target)/180))).toFixed());
//Отправить в телеграмм расстояния до объектов в метрах. Нужно для отладки.
//SendToTelegram(userA +' до дома '+ (6371*1000*Math.acos(Math.sin(Math.PI*alt_дом/180)*Math.sin(Math.PI*alt_target/180)+Math.cos(Math.PI*alt_дом/180)*Math.cos(Math.PI*alt_target/180)*Math.cos(Math.PI*(long_дом-long_target)/180))).toFixed(), getState('telegram.0.communicate.requestChatId').val);
//SendToTelegram(userA +' до офиса '+ (6371*1000*Math.acos(Math.sin(Math.PI*alt_офис/180)*Math.sin(Math.PI*alt_target/180)+Math.cos(Math.PI*alt_офис/180)*Math.cos(Math.PI*alt_target/180)*Math.cos(Math.PI*(long_офис-long_target)/180))).toFixed(), getState('telegram.0.communicate.requestChatId').val);
//SendToTelegram(userA +' до школы ' + (6371*1000*Math.acos(Math.sin(Math.PI*alt_школа/180)*Math.sin(Math.PI*alt_target/180)+Math.cos(Math.PI*alt_школа/180)*Math.cos(Math.PI*alt_target/180)*Math.cos(Math.PI*(long_школа-long_target)/180))).toFixed(), getState('telegram.0.communicate.requestChatId').val);

//Блок проверки
//Выключить весь свет, когда расстояния до мобил больше 400 метров и дома кто то был
if ((getState('javascript.0.MyHome.ЛюдиДома').val) && (getState('javascript.0.GPSLogger.43a70ef7.доДома').val > 400) && (getState('javascript.0.GPSLogger.477182ff.доДома').val > 400)) {
setState('javascript.0.MyHome.ЛюдиДома', false);
setState('javascript.0.MyHome.ProjON', false);
Управление_Светом('мы уехали'); }
//Включить весь свет, когда расстояния до мобил меньше 400 метров и дома никого нет
if (!(getState('javascript.0.MyHome.ProjON').val) && ((getState('javascript.0.GPSLogger.43a70ef7.доДома').val < 400) || (getState('javascript.0.GPSLogger.477182ff.доДома').val < 400))) {
setState('javascript.0.MyHome.ЛюдиДома', true);
setState('javascript.0.MyHome.ProjON',   true);
Управление_Светом('мы приехали'); }
//Включить прожекторы при приближении к дому ближе 1000 метров
if (!(getState('javascript.0.MyHome.ProjON').val) && ((getState('javascript.0.GPSLogger.43a70ef7.доДома').val < 1000) || (getState('javascript.0.GPSLogger.477182ff.доДома').val < 1000))) {
setState('javascript.0.MyHome.ProjON',   true);
Управление_Светом('включи все прожекторы'); }

});

{% endhighlight %}

[1]: https://forum.iobroker.net/viewtopic.php?t=17435

