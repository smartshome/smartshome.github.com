---
layout: post
title: "Парсим прогноз погоды "
date:   2018-12-26
description: "ioBroker скрипт часть.1"
categories: ioBroker
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
introduction: "☔️ Скрипт, который получает значения прогноза погоды."
---

# ioBroker скрипт часть.1
**Преамбула.**
Началось все с того что данные драйвера **yr.no погода** перестали показывать актуальную погоду, что собственно и стало
причиной написания данного скрипта. 
[cheerio][1]
[iconv-lite][2]
Добавим библиотеки.
![][6] 
>**Лайфхак** [jscript][3]

**meteovesti.ru/pogoda_5/34214**
![][4] 



{% highlight javascript %}
createState('javascript.0.cast.temp', 0);//создаем обьекты в папке javascript.0=>cast с значениями 0
createState('javascript.0.cast.date', 0);
createState('javascript.0.cast.temp1', 0);
createState('javascript.0.cast.date1', 0);
schedule('*/30 * * * *', function () {//крон, парсим каждые 30 минут
var request = require("request");
var cheerio = require('cheerio');
var iconv  = require('iconv-lite');
var Dat,Temp,Dat1,Temp1
 
var requestOptions  = { method: "post"
                ,uri: "http://www.meteovesti.ru/pogoda_5/34214"//сайт погоды откуда парсим температуру и дату
                ,headers: { "User-Agent": "Mozilla/5.0" }
                ,encoding: null
            };

// request 
request(requestOptions, function(error, response, body) {
  // по рузки
  var strContents = new Buffer(body);
  var $ = cheerio.load(iconv.decode(strContents, 'windows-1251').toString());//переводим кракозябры в читаемый вид
  Dat = $('td .wpdate').eq(0).text();
  Temp = $('td .wptemp').eq(0).text();
  Dat1 = $('td .wpdate').eq(1).text();
  Temp1 = $('td .wptemp').eq(1).text();
  setState('javascript.0.cast.temp', Temp);//записываем спарсеные значения в объект
  setState('javascript.0.cast.date', Dat);
  setState('javascript.0.cast.temp1', Temp1);
  setState('javascript.0.cast.date1', Dat1);
  //console.log(Dat); 
  //console.log(Temp);
 });
});
{% endhighlight %}

скрин 
![][5] 

[1]: https://github.com/cheeriojs/cheerio
[2]: https://github.com/ashtuchkin/iconv-lite
[3]: https://gist.github.com/developer-sdk/a70bbf570d36e119c4853bedcfdd29f3
[4]: /assets/image/salam/weather.png
[5]: /assets/image/salam/weather_obj.png
[6]: /assets/image/salam/parser_lib.png




