---
layout: post
title: "Android приложение -MQTT Broadcast Receiver"
date:   2018-06-15
description: "Управляем Андроид устройством посредством MQTT. "
categories: mobile
main-class: 'mobile'
color: '#EB7728'
tags: MQTT mobile Android
excerpt:
introduction: "🎮 - Андроид приложение  MQTT Broadcast Receiver . "
---

## Приложение для android, которое может передавать данные о состоянии батареи, входящих звонках и сообщениях и т.д. 
### Скачиваем приложение.
[Скачать][2]
[Видео][1]
### Внешний вид.
Имена категории и под категории можно создавать любые, названия топика (topic) должно содержать только латинский буквы и не иметь пробелов. Все категории будут создаваться в топике *GT-I9500/info/item/*.  Например для категории Room и ее под категория Light будут создан такой топик  *GT-I9500/info/item/room/light*
![][3]![][4]![][5]
### Настройки.
![][6]![][7]![][8]
С большей частью настроек вкладки *MQTT* не должно возникнуть вопросов, разве что два последних пункта. Device name — это имя вашего android устройства, например у Samsung S4 имя будет GT-I9500, если у вас 2 одинаковых android устройства, то имя устройства нужно поменять, так как если этого не сделать все сообщения будут приходить на один топик. *First topic* — это первоначальный топик, данная опция нужна если хотите разместить не в корне брокера, как тут *GT-I9500/info/battery/charging*, а на пример в топике mytopic, тогда все топики будут размещаться так mytopic/GT-I9500/info/battery/charging

*General* — это вкладка с общими настройками, тут можно выбрать какие данные будут отправляться на ваш сервер (MQTT брокер), а так же другие параметры. Пункт Full battery данная настройка устанавливает при каком уровне зарядке батареи, будет отправлен в топик *GT-I9500/info/battery/status* значение ok
[Источник...][9]



[1]: https://www.youtube.com/watch?v=VtKbiyyVZks
[2]: https://github.com/bondrogeen/CLAW_light/raw/master/app/release/app-release.apk
[3]: https://codedevice.ru/wp-content/uploads/2018/06/Screenshot_app_menu.jpg
[4]: https://codedevice.ru/wp-content/uploads/2018/06/Screenshot_app.jpg
[5]: https://codedevice.ru/wp-content/uploads/2018/06/Screenshot_app_item.jpg
[6]: https://codedevice.ru/wp-content/uploads/2018/06/Settings.jpg
[7]: https://codedevice.ru/wp-content/uploads/2018/06/MQTT_settings.jpg
[8]: https://codedevice.ru/wp-content/uploads/2018/06/General_settings.jpg
[8]: https://codedevice.ru/archives/913



