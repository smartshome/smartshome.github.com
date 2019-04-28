---
layout: post
title: " API Saures.ru "
date:   2019-04-29
description: "Интегрируем API Saures.ru в  ioBroker"
categories: ioBroker
main-class: 'iobroker'
color: '#EB7728'
tags:
- iobroker
- iob
- иоброкер
- иоб
- Saures
- script 
excerpt:
introduction: "⚙️ Интегрируем API Saures.ru в  ioBroker"
---
# Интеграции с системой SAURES.
### Что такое SAURES ?

**SAURES** - это система из многофункциональных контроллеров и облачного сервиса, автоматизирующая регулярные задачи потребителей коммунальных ресурсов: ежемесячная передача показаний счетчиков, контроль состояния датчиков, защита от протечек и оповещение об аварийных ситуациях. 

Что понадобится нам? Это документация [пользовательского API.pdf][1]

### Отправляем POST и GET запросы.

>Для отправки POST и GET запросов будем пользовать програму  [Postman][2].

В теле запроса пропишем емайл и пасворд, в  теле ответа должны увидеть ** sid **.

![][3]

>Программа может сгенерировать код для запроса для разных ЯП ...**NodeJS** **PHP** **bash** и т.д.



### Скрипт в IoBroker.

Ну и сам скрипт, который получает значения через API и сохраняет в обьектах IoBrоker.

{% highlight jscript %}
let request = require('request');//подключаем библиотеку request запросов

createState('Saures.hot_water',0);//Создаем обьекты куда запишем значения из базы Saures
createState('Saures.cold_water',0);// с значениями = 0
createState('Saures.temp_water',0);

let apiUrlLogin= 'https://lk.saures.ru/api/auth/login';
let apiUrlMeter= 'https://lk.saures.ru/api/meter/meters',
sid;
let optionsPost = { //опции для Post запроса 
 url:apiUrlLogin,
 headers: {"Content-Type": " application/x-www-form-urlencoded; charset=utf-8"},
 formData: { email: 'demo@saures.ru', password: 'demo' }
};

 
function getSid(error, response, body) {
  if (!error && response.statusCode == 200) {
    var info = JSON.parse(body);
    sid = info.data.sid
    console.log(" Sid is - "+sid );
    let optionsGet = {
    url: apiUrlMeter,
    qs: { sid: sid, flat_id: '385' },   //опции для Get запроса 
}
    setTimeout(() => {request.get(optionsGet, getMeter)}, 1000);//Get запрос данных
  }
};
function getMeter(error, response, body) {
    if (!error && response.statusCode == 200  ) {
      var info = JSON.parse(body);
     //console.log(body); //раскоментировать для получения JSON данных
      console.log(" Статус - "+info.status );
      let HotW = info.data.sensors[0].meters[0].value ;//записуем значения в переменые
      let ColdW = info.data.sensors[0].meters[1].value ;
      let tempW = info.data.sensors[0].meters[2].value ;
      setState("javascript.0.Saures.hot_water", HotW, true);//записуем значения в созданые обьекты
      setState("javascript.0.Saures.cold_water", ColdW, true);
      setState("javascript.0.Saures.temp_water", tempW, true);
    }
  };

// schedule("* */12 * * *",  () => {
//   request.post(optionsPost, getSid);
// }); // раскоментировать крон для отправки запросов каждые 12 часов

request.post(optionsPost, getSid); //отправляем запрос

{% endhighlight %}

### Редактируем скрипт под свои задачи.

Для удобства распарсивания даных которые приходят с сервера воспользуемся 
[JSONeditorOnline][2]

в функции **getMeter** для получения данных расскоментируем строку

{% highlight jscript %}
//console.log(body); //раскоментировать для получения JSON данных
{% endhighlight %}

Полученный код необходимо вставить в левую панель , а в правой смотрим путь для нужного значения данных.
{% highlight jscript %}
 let HotW = info.data.sensors[0].meters[0].value
{% endhighlight %}
 
 ![][6]

### Итог.

В итоге получаем значения в обьектах **IoBroker**, которые будут обновлятся раз в 12 часов.

![][7]

[1]: https://www.saures.ru/upload/iblock/efd/SAURES-API-polzovatelskoe-v.3.12.pdf
[2]: https://web.postman.co
[3]: /assets/image/Postman_code.png
[5]: https://jsoneditoronline.org/
[6]: /assets/image/JSON_Editor.jpg
[6]: /assets/image/ObjSaures.png
