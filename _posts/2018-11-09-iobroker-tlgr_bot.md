---
layout: post
title: " Inline меню для Telegram бота"
date:   2018-11-09
description: "IoBroker. Inline меню для Telegram бота"
categories: ioBroker
main-class: 'iobroker'
color: '#EB7728'
tags:
- iobroker
- iob
- иоброкер
- иоб
- telegram
- jscript 
excerpt:
introduction: "🤖 Создаем Inline меню для Telegram бота "
---


### IoBroker. Inline меню для Telegram бота



![](1000x400x_image.png)



В этой статье мы с вами по шагам будем создавать меню для
 telegram бота Умного Дома. Рекомендую изучить минимальные азы по языку
программирования *JavaSсript*, это облегчит понимание того, что тут вообще происходит.

## Приготовления

 Сначала необходимо установить драйвера <b>Telegram</b> и <b>Script Engine</b>. Вспомнить как это делается, можно в статье [ioBroker - устанавливаем первый драйвер](https://sprut.ai/client/article/274). Не забываем подключить своего бота к драйверу Telegram, как описано начале статьи - [ioBroker - уведомления.](https://sprut.ai/client/article/287)

При увеличении количества написанных скриптов, будет нарастать
бардак в дереве, поэтому рекомендую сразу приучать себя разбивать
скрипты по группам. 
![](https://sprut.ai/static/media/cache/00/13/75/40/291/5107/3000x_image.png)

Добавим новую группу в папку <b>common</b>

![](https://sprut.ai/static/media/cache/00/13/75/40/291/5091/3000x_image.png) 

и назовем ее например <b>Telegram</b>. В этой группе в дальнейшем можно будет создавать все скрипты, которые будут относиться к работе с драйвером <b>Telegram</b>.

![](https://sprut.ai/static/media/cache/00/13/75/40/291/5092/1600x_image.png)

Вот теперь можно добавить наш будущий скрипт для меню. Для этого надо выделить созданную группу <b>Telegram</b> и нажать кнопку <b>Новый скрипт</b>

![](https://sprut.ai/static/media/cache/00/13/75/40/291/5099/3000x_image.png)

В появившемся окне выбираем нужный тип языка - <b>JavaScript</b>

![](https://sprut.ai/static/media/cache/00/13/75/40/291/5100/3000x_image.png)

Поменяем имя на <b>Телеграм бот</b> и сохраним изменения.

![](https://sprut.ai/static/media/cache/00/13/75/40/291/5109/1600x_image.png)

Все готово к созданию меню для Умного дома. 

## Создаем Меню

Предварительно необходимо на листике или в уме подготовить набросок древовидной структуры будущего меню

В этой статье попробуем реализовать подобную структуру меню. 

Не
 обязательно придерживаться разбиения по комнатам, рекомендую в основные
 ветки выносить управление тем, что чаще всего используется, т.к.
допустим для запуска <b>Сцены 3</b> в зале надо сделать целых 4 нажатия! 

<b>Вызвать Меню - Зал - Сценарии - Сцена 3</b>

Ежедневно запускать сценарий по такому длинному пути быстро надоест :)

Набросаем наше дерево в скрипте

```javascript
var button = [{name: 'Меню',button: ['Зал', 'Кухня', 'Спальня', 'Ванная', 'Закрыть', 'Меню']},
                {name: 'Зал',button: ['Люстра', 'Зал. Кондиционер', 'Зал. Сцены', 'Назад', 'Закрыть', 'Меню']},
                    {name: 'Зал. Сцены', button: ['Кино', 'Весь свет', 'Приглушенный свет', 'Назад', 'Закрыть', 'Зал']},
                {name: 'Кухня', button: ['Кухня. Свет', 'Вентиляция', 'Назад', 'Закрыть', 'Меню']},
                    {name: 'Верхний свет', button: ['Включить', 'Выключить', 'Назад', 'Закрыть', 'Кухня']},
                {name: 'Спальня', button: ['Спальня. Свет', 'Спальня. Кондиционер', 'Спальня. Сцены', 'Назад', 'Закрыть', 'Меню']},
                    {name: 'Спальня. Свет', button: ['Верхний', 'Бра', 'Назад', 'Закрыть', 'Спальня']},
                    {name: 'Спальня. Сцены', button: ['Сцена 1', 'Сцена 2', 'Назад', 'Закрыть', 'Спальня']},
                {name: 'Ванная', button: ['Ванная. Свет', 'Бойлер', 'Вытяжка', 'Назад', 'Закрыть', 'Меню']},
    ];
```

javascript [Копировать]()

Разберем подробнее

В первой строке в квадратных скобках перечисляются основные кнопки (ветки) меню, плюс дополнительно добавляется кнопка <b>Закрыть</b>.
 Она позволит закрывать меню в чате бота, чтобы у нас не получилось куча
 сообщений от бота с открытыми менюшками. Ну и в конце текст в кавычках <b>'Меню'</b>
 тоже обязателен. В этом месте будет указываться название вышестоящей
ветки меню, т.к. первая строка уже является верхушкой дерева, то текст в
 этом месте дублирует начало.

Вторая строка - переход по дереву ниже на ветку <b>Зал</b>. Соответственно в квадратных скобках уже перечислены кнопки меню <b>Зал</b>. К <b>Закрыть</b>, добавилась кнопка <b>Назад</b>, которая позволит подняться на одну ветку выше и в конце укажем куда - <b>'Меню'</b>. 

Третья строка - переход еще ниже в меню <b>Зал. Сцены</b>. Для корректной работы кнопки <b>Назад</b>, в конце пишем - <b>'Зал'</b>. 

<i>Отступы для каждой строки сделаны лишь для удобства восприятия структуры меню и никакой функциональности не несут</i>.

Внимательный читатель, надеюсь, обратил внимание что названия веток меню в наброске и в коде отличаются :) Почему так сделано, будет описано дальше.  

Дальнейший код будет описан только в объеме, необходимом для оформления своего меню, плюс краткое пояснение функций.

Добавляем в скрипт весь остальной код. 

```javascript
var button = [{name: 'Меню',button: ['Зал', 'Кухня', 'Спальня', 'Ванная', 'Закрыть', 'Меню']},
                {name: 'Зал',button: ['Люстра', 'Зал. Кондиционер', 'Зал. Сцены', 'Назад', 'Закрыть', 'Меню']},
                    {name: 'Зал. Сцены', button: ['Кино', 'Весь свет', 'Приглушенный свет', 'Назад', 'Закрыть', 'Зал']},
                {name: 'Кухня', button: ['Кухня. Свет', 'Вентиляция', 'Назад', 'Закрыть', 'Меню']},
                    {name: 'Верхний свет', button: ['Включить', 'Выключить', 'Назад', 'Закрыть', 'Кухня']},
                {name: 'Спальня', button: ['Спальня. Свет', 'Спальня. Кондиционер', 'Спальня. Сцены', 'Назад', 'Закрыть', 'Меню']},
                    {name: 'Спальня. Свет', button: ['Верхний', 'Бра', 'Назад', 'Закрыть', 'Спальня']},
                    {name: 'Спальня. Сцены', button: ['Сцена 1', 'Сцена 2', 'Назад', 'Закрыть', 'Спальня']},
                {name: 'Ванная', button: ['Ванная. Свет', 'Бойлер', 'Вытяжка', 'Назад', 'Закрыть', 'Меню']},
    ];

var menuUp = 'Меню';
var first_tap = false;
var menu_current;
var topTextGlobal;

on({id: "telegram.0.communicate.request", change: 'any'}, function (obj) {
    command = obj.state.val.substring(obj.state.val.indexOf(']') + 1);
    user = obj.state.val.substring(obj.state.val.indexOf('[')+1, obj.state.val.indexOf(']'));
    //log(command);
    //log(user);
//************************************
// Меню
//************************************
    if (command ==="/buttons" || command ==="кнопки" || command ==="Кнопки")
        sendTo('telegram', {
            user: user,
            text:   'Показать меню',
            reply_markup: {
                keyboard: [['Показать меню']],
                resize_keyboard:   true,
                one_time_keyboard: true
            }
        });

//************************************
//  меню inline
//************************************
    var menu = {
        reply_markup: {
            inline_keyboard: [[],[],[],[],[],[],[]],
        }
    };

    if (command === 'Показать меню') command = menuUp;
    log (command);
    if (command === 'Меню') first_tap = true;
    if (command === 'Назад') command = menuUp;
    var but1 = getButtonArray(button, 'name', command).toString();

    if (but1.length > 0) {          // проверяем, что строка не пустая
        var but2 = but1.split(','); //преобразуем в массив
        menuUp = but2.pop();        //вырезаем последний элемент

        if (but2.length > 0) {      // проверяем что массив не пуст
            var index = 0;
            for (var i=0, len=but2.length; i<len; ((i%3="" if="" but2[i]});="" but2[i],="" menu.reply_markup.inline_keyboard[index].push({="" {="" i++)="">= 2)&&(index < 6)) index = ++index;
            }

            var topText = funcTopText(command);
            topTextGlobal = command;
            menu_current = menu.reply_markup;
            if (first_tap) {
                sendTo('telegram.0', {user: user, text: topText, parse_mode: 'markdown', reply_markup: menu.reply_markup});
                first_tap = false;
            } else {
                updateMenuButton(user, topText, menu.reply_markup);

            }
        }
    }
    //************************************
    // Команды
    //************************************
    // ищем в тексте команды
    switch (command) {

        case "Закрыть":
            sendTo('telegram', {
                user: user,
                deleteMessage: {
                    options: {
                        chat_id: getState("telegram.0.communicate.requestChatId").val,
                        message_id: getState("telegram.0.communicate.requestMessageId").val,
                    }
                }
            });
            break;
    }
});

function updateMenuButton(user, topText, menu){
    sendTo('telegram', {
        user: user,
        text: topText,
        editMessageText: {
            options: {
                chat_id: getState("telegram.0.communicate.requestChatId").val,
                message_id: getState("telegram.0.communicate.requestMessageId").val,
                parse_mode: 'markdown',
                reply_markup: menu
            }
        }
    });
}

function waitConfirmCommand(obj, command, timeout, ack = false){
    var mySubscription;

    var timeID = setTimeout(() => {
        unsubscribe(mySubscription);
        CommandDone('Не выполнено!');
    }, timeout);

    mySubscription = on({id: obj, change: 'ne'}, function (data) {
        // unsubscribe after first trigger
        if (ack === true)
            if (data.state.ack) ack = false;
        if (!ack) {
            updateMenuButton(user, funcTopText(command), menu_current);
            unsubscribe(mySubscription);
            clearTimeout(timeID);
            CommandDone('Выполнено!');
        }
    });
}

function CommandDone(text){
    if (text === '') text = "Выполнено!";
    sendTo('telegram', {
        user: user,
        answerCallbackQuery: {
            text: text,
            showAlert: false
        }
    });
}

function getButtonArray(obj, keyName, Name) {
    var result = [];
    for (var attr in obj) {
        if (obj[attr] && typeof obj[attr] === 'object') {
            result = result.concat(getButtonArray(obj[attr], keyName, Name));
        }
        if (attr === keyName && obj[attr] === Name) {
            result.push(obj.button);
        }
    }
    return result;
}

function stateSelection(state){
    if ((state.val === false) || (state.val === 'false')) return "ВЫКЛ. в " + formatDate(state.lc, "SS:mm:ss TT.MM");
    if ((state.val === true) || (state.val === 'true')) return "ВКЛ. в " + formatDate(state.lc, "SS:mm:ss TT.MM");
    return "неопределено";
}

function funcTopText (command){
    var text = command;
    switch (command) {
        case 'Меню':
            text =  "*Зал*: Температура 24.2 градуса \n"
            + "*Спальня*: Температура 23.5 градуса \n";
            break;
    }
    return text;
}</len;>
```

javascript [Копировать]()

Уже на этом этапе можно проверить работу меню. Для этого сохраняем скрипт, запускаем и в <b>Telegram</b>отправляем боту слово <b>Меню</b>(<i>внимание, слово должно быть с большой буквы</i>) или <b>Кнопки</b>. 

## Управление

Переходим к следующему шагу. Добавим в скрипт команды управления оборудованием (<i>свет, бойлер, сценарии, насосы, краны, телевизор, кондиционер и т.д.</i>). Для этого создадим виртуальный выключатель (<i>код добавим в самое начало скрипта</i>)

```javascript
createState("Test.Switch.command", false);  // Тестовый выключатель света
```

javascript [Копировать]()

Сохраним и команда <b>createState</b>создаст новый объект, который можно посмотреть на вкладке <b>Объекты - javascript.0</b>

Дальше вставим в скрипт команды управления светом на кухне на примере виртуального выключателя<br>

```javascript
case "Включить": //включить свет на кухне
setState("javascript.0.Test.Switch.command"/*Test.Switch.command*/, true);
CommandDone('Выполнено!');
break;

case "Выключить": //выключить свет на кухне
setState("javascript.0.Test.Switch.command"/*Test.Switch.command*/, false);
CommandDone('Выполнено!');
break;
```

javascript [Копировать]()

должно получиться так

 Ранее в статье был момент,
когда названия веток (кнопок) немного менялись и стали отличаться от
наброска меню. Весь смысл в том, что <b>названия всех кнопок в меню бота должны быть</b><b>уникальны</b>, это связано с особенностями API Telegram. Подробнее можно почитать [тут](https://core.telegram.org/bots/api#callbackquery).
 Иначе при нажатии на одинаковые названия кнопок в меню, всегда будут
выполняться команды только для какой-то одной кнопки, даже если вы на
нее не нажимали.

Разберем подробнее, что же мы сделали:<br>

<b>switch</b>- сравнивает
 выражение со случаями, перечисленными внутри неё, а затем выполняет
соответствующие инструкции. Подробнее [тут](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/switch).

<b>command</b>- будет содержать уникальное имя нажатой кнопки

<b>case "Включить"</b> - сравниваем со всеми описанными в секции <b>switch</b>именами кнопок и ищем команды для нажатой кнопки <b>Выключить</b>.

<b>setState</b><b>("</b><b>javascript</b><b>.0.</b><b>Test</b><b>.</b><b>Switch</b><b>.</b><b>command</b><b>"/*</b><b>Test</b><b>.</b><b>Switch</b><b>.</b><b>command</b><b>*/,</b><b>true</b><b>);</b>- записываем значение <b>True</b> в объект <b>Test</b><b>.</b><b>Switch</b><b>.</b><b>command</b>. Дополнительно по команде <b>setState</b>можно почитать [тут](https://github.com/ioBroker/ioBroker.javascript/blob/master/doc/en/javascript.md#setstate).

<b>CommandDone('Выполнено!');</b> - вызываем функцию <b>CommandDone</b> и передаем ей текст, который хотим отобразить во всплывающем сообщении при нажатии кнопки. В данном случае текст будет <b>Выполнено!</b>

<b>break;</b> - указываем, что выполнение операций для кнопки <b>Включить</b>завершаем. Если не добавить команду <b>break</b>, <b>JS</b> начнет выполнение следующего <b>case.</b>

Для кнопки <b>Выключить</b> все тоже самое, только записываемое значение <b>false</b>.

Сохраняем и пробуем! (<i>результат можно увидеть на вкладке Объекты. Будет меняться значение виртуального выключателя</i>). 

Для элементарного меню и простых команд этого уже достаточно. А для тех, кто хочет большего, продолжим.

## Красивости

Начнем добавлять различные
красивости: отображение текущего состояния, обновление состояний при
нажатии кнопки, красивый вывод состояний, эмодзи и т.д.

За вывод подобной информации отвечает функция <b>funcTopText</b>

```javascript
function funcTopText (command){
    var text = command;
    switch (command) {
        case 'Меню':
            text =  "*Зал*: Температура 24.2 градуса \n"
            + "*Спальня*: Температура 23.5 градуса \n";
            break;
    }
    return text;
}
```

javascript [Копировать]()

В функцию автоматически через переменную <b>command</b> передается нажатая кнопка дерева (<i>например Меню, Спальня, Кухня. Свет и т.д.</i>), самый нижний уровень дереве меню передать нельзя (<i>например Кино, Бойлер, Бра и т.д.</i>). И уже в самой функции с помощью уже знакомой конструкции  <b>switch (command)</b>
 можно добавить вывод необходимой информации о состоянии оборудования
или просто какой-то текст для выбранной ветки меню. В примере выше
указана ветка <b>Меню</b>, в ней выводим температуры (<i>пока как простой текст</i>) в комнатах <b>Зал</b> и <b>Спальня</b>.

Как сделать форматирование текста <b>жирным</b> или <i>курсивом</i>, можно почитать [тут](https://core.telegram.org/bots/api#markdown-style).

Выведем температуру в зале из реального объекта. <b>Надо выбрать из ваших существующих объектов, иначе будет ошибка.</b>

```javascript
case 'Меню':
            text =  "*Зал*: Температура " + getState("mysensors.0.70.3_TEMP.V_TEMP").val + "°C, \n"
            + "*Спальня*: Температура 23.5 градуса \n";
```

javascript [Копировать]()

Из объекта  <b>mysensors</b><b>.0.70.3_</b><b>TEMP</b><b>.</b><b>V</b><b>_</b><b>TEMP</b> считываем значение температуры и подставляем его в строку (подробнее о команде <b>getState</b> [тут](https://github.com/ioBroker/ioBroker.javascript/blob/master/doc/en/javascript.md#getstate)).

<b>\</b><b>n</b> – символ новой строки, является эквивалентом символа перевода строки.

Добавим в меню <b>Кухня</b>отображение состояния виртуального выключателя

```javascript
function funcTopText (command){
    var text = command;
    switch (command) {
        case 'Меню':
            text =  "*Зал*: Температура " + getState("mysensors.0.70.3_TEMP.V_TEMP").val + "°C, \n"
            + "*Спальня*: Температура 23.5 градуса \n";
            break;
        case 'Кухня':
            text =  "*Свет*: состояние: " + getState("Test.Switch.command").val + "\n";
            break;
    }
    return text;
}
```

javascript [Копировать]()

Состояние <b>false</b>, как-то скучно...  да объясняй потом жене, ребенку, друзьям что за <b>false</b> такой...

Используем функцию <b>stateSelection</b><b>(</b><b>state</b><b>)</b>, которая будет вместо <b>false</b><b>/</b><b>true</b> возвращать нормальный текст <b>Вкл/Выкл</b>. и дополнительно выводить время подачи команды (<i>или любой другой текст на ваше усмотрение</i>)

```javascript
function stateSelection(state){
    if ((state.val === false) || (state.val === 'false')) return "ВЫКЛ. в " + formatDate(state.lc, "SS:mm:ss TT.MM");
    if ((state.val === true) || (state.val === 'true')) return "ВКЛ. в " + formatDate(state.lc, "SS:mm:ss TT.MM");
    return "неопределено";
}

function funcTopText (command){
    var text = command;
    switch (command) {
        case 'Меню':
            text =  "*Зал*: Температура " + getState("mysensors.0.70.3_TEMP.V_TEMP").val + "°C, \n"
            + "*Спальня*: Температура 23.5 градуса \n";
            break;
        case 'Кухня':
            text =  "*Свет*: состояние: " + stateSelection(getState("Test.Switch.command")) + "\n";
            break;
    }
    return text;
}
```

javascript [Копировать]()

Так гораздо лучше и понятней смотрится!

В функцию <b>stateSelection</b><b>(</b><b>state</b><b>)</b> при необходимости можно добавить другие типы состояний, если они будут выводиться в заголовок меню.

Может еще улучшим? Сделаем, чтобы при нажатии кнопки
<b>Кухня. Свет</b> сразу в заголовке менялось состояние, для этого воспользуемся
функцией

```javascript
function waitConfirmCommand(obj, command, timeout, ack = false){
    var mySubscription;

    var timeID = setTimeout(() => {
        unsubscribe(mySubscription);
        CommandDone('Не выполнено!');
    }, timeout);

    mySubscription = on({id: obj, change: 'ne'}, function (data) {
        // unsubscribe after first trigger
        if (ack === true)
            if (data.state.ack) ack = false;
        if (!ack) {
            updateMenuButton(user, funcTopText(command), menu_current);
            unsubscribe(mySubscription);
            clearTimeout(timeID);
            CommandDone('Выполнено!');
        }
    });
}
```

javascript [Копировать]()

Функция подписывается на переданный объект <b>obj</b> и в течение заданного количества миллисекунд <b>timeout</b> ожидает изменения объекта <b>obj</b>. Если событие произошло и была задана дополнительно (но не обязательно) проверка флага <b>ACK</b>, проверяется на условие <b>ack</b><b>=</b><b>true</b>. Если совпало или не была задана проверка флага <b>ACK</b> – отобразится текст <b>Выполнено</b>. Если не совпало – <b>Не выполнено</b>. После этого функция отписывается от объекта, чтобы в будущем снова не реагировать на него.

Внесем изменения в скрипт

```javascript
function funcTopText (command){
    var text = command;
    switch (command) {
        case 'Меню':
            text =  "*Зал*: Температура " + getState("mysensors.0.70.3_TEMP.V_TEMP").val + "°C, \n"
            + "*Спальня*: Температура 23.5 градуса \n";
            break;
case 'Кухня':
            text =  "*Свет*: " + stateSelection(getState("javascript.0.Test.Switch.command")) + "\n";
            break;

case 'Кухня. Свет':
            text =  "*Свет*: " + stateSelection(getState("javascript.0.Test.Switch.command")) + "\n";
            break;
    }
    return text;
}
```

javascript [Копировать]()

```javascript
case "Включить": //включить свет на кухне
            setState("javascript.0.Test.Switch.command"/*Test.Switch.command*/, true);
            waitConfirmCommand("javascript.0.Test.Switch.command", topTextGlobal, 2000, true);
        break;

        case "Выключить": //выключить свет на кухне
            setState("javascript.0.Test.Switch.command"/*Test.Switch.command*/, false);
            waitConfirmCommand("javascript.0.Test.Switch.command", topTextGlobal, 2000);
        break;
```

javascript [Копировать]()

<b>javascript</b><b>.0.</b><b>Test</b><b>.</b><b>Switch</b><b>.</b><b>command</b>
 – объект для проверки выполнения команды. Т.к. у нас нет отдельного
объекта для обратной связи, используем для этой цели объект-команду.

<b>2000</b>– время ожидания в миллисекундах

<b>topTextGlobal</b> – <b>эту переменную ставим всегда!</b>

Для кнопки <b>Включить</b>добавили проверку флага <b>ACK</b>, для кнопки <b>Выключить</b> нет. Сохраняем и пробуем что получилось.

Уберем проверку флага <b>ACK</b> для кнопки <b>Включить</b> и снова пробуем.

Не забываем смотреть на всплывающие сообщения!

Надеюсь, суть уловили ?

## Эмодзи

Пришла очередь добавить символы <b>Эмодзи</b>. Для этого заходим в библиотеку [один](https://emojipedia.org/) или [два](https://getemoji.com/). Выбираем подходящие символы для меню. Возьмем для кнопки <b>Назад</b> следующий символ

Выделяем, копируем и вставим в наш скрипт.

<b><i>Внимание!</i></b><i>Менять надо во всех местах скрипта, где использовано слово<b>Назад</b>.</i>

Замену лучше делать через <b>Поиск</b>комбинацией клавиш <b>ctrl+H</b>

Сохраняем и любуемся результатом

Надеюсь у вас все получилось ?

Итоговый скрипт меню

```javascript
createState("Test.Switch.command", false);  // Тестовый выключатель света

var button = [{name: 'Меню',button: ['Зал', 'Кухня', '?', 'Ванная', 'Закрыть', 'Меню']},
                {name: 'Зал',button: ['Люстра', 'Зал. Кондиционер', 'Зал. Сцены', '◀️ Назад', 'Закрыть', 'Меню']},
                    {name: 'Зал. Сцены', button: ['Кино', 'Весь свет', 'Приглушенный свет', '◀️ Назад', 'Закрыть', 'Зал']},
                {name: 'Кухня', button: ['Кухня ?', 'Вентиляция', '◀️ Назад', 'Закрыть', 'Меню']},
                    {name: 'Кухня ?', button: ['Включить', 'Выключить', '◀️ Назад', 'Закрыть', 'Кухня']},
                {name: '?', button: ['? Спальня. Свет', '? Кондиционер', '? Сцены', '◀️ Назад', 'Закрыть', 'Меню']},
                    {name: '? Свет', button: ['Верхний', 'Бра', '◀️ Назад', 'Закрыть', '?']},
                    {name: '? Сцены', button: ['Сцена 1', 'Сцена 2', '◀️ Назад', 'Закрыть', '?']},
                {name: 'Ванная', button: ['Ванная. Свет', 'Бойлер', 'Вытяжка', '◀️ Назад', 'Закрыть', 'Меню']},
    ];

var menuUp = 'Меню';
var first_tap = false;
var menu_current;
var topTextGlobal;

on({id: "telegram.0.communicate.request", change: 'any'}, function (obj) {
    command = obj.state.val.substring(obj.state.val.indexOf(']') + 1);
    user = obj.state.val.substring(obj.state.val.indexOf('[')+1, obj.state.val.indexOf(']'));
    //log(command);
    //log(user);
//************************************
// Меню
//************************************
    if (command ==="/buttons" || command ==="кнопки" || command ==="Кнопки")
        sendTo('telegram', {
            user: user,
            text:   'Показать меню',
            reply_markup: {
                keyboard: [['Показать меню']],
                resize_keyboard:   true,
                one_time_keyboard: true
            }
        });

//************************************
//  меню inline
//************************************
    var menu = {
        reply_markup: {
            inline_keyboard: [[],[],[],[],[],[],[]],
        }
    };

    if (command === 'Показать меню') command = menuUp;
    log (command);
    if (command === 'Меню') first_tap = true;
    if (command === '◀️ Назад') command = menuUp;
    var but1 = getButtonArray(button, 'name', command).toString();

    if (but1.length > 0) {          // проверяем, что строка не пустая
        var but2 = but1.split(','); //преобразуем в массив
        menuUp = but2.pop();        //вырезаем последний элемент

        if (but2.length > 0) {      // проверяем что массив не пуст
            var index = 0;
            for (var i=0, len=but2.length; i<len; i++) {
                menu.reply_markup.inline_keyboard[index].push({ text: but2[i], callback_data: but2[i]});
                if ((i%3 >= 2)&&(index < 6)) index = ++index;
            }

            var topText = funcTopText(command);
            topTextGlobal = command;
            menu_current = menu.reply_markup;
            if (first_tap) {
                sendTo('telegram.0', {user: user, text: topText, parse_mode: 'markdown', reply_markup: menu.reply_markup});
                first_tap = false;
            } else {
                updateMenuButton(user, topText, menu.reply_markup);

            }
        }
    }
    //************************************
    // Команды
    //************************************
    // ищем в тексте команды
    switch (command) {
        case "Включить": //включить свет на кухне
            setState("javascript.0.Test.Switch.command"/*Test.Switch.command*/, true);
            waitConfirmCommand("javascript.0.Test.Switch.command", topTextGlobal, 2000, true);
        break;

        case "Выключить": //выключить свет на кухне
            setState("javascript.0.Test.Switch.command"/*Test.Switch.command*/, false);
            waitConfirmCommand("javascript.0.Test.Switch.command", topTextGlobal, 2000);
        break;

        case "Закрыть":
            sendTo('telegram', {
                user: user,
                deleteMessage: {
                    options: {
                        chat_id: getState("telegram.0.communicate.requestChatId").val,
                        message_id: getState("telegram.0.communicate.requestMessageId").val,
                    }
                }
            });
            break;
    }
});

function updateMenuButton(user, topText, menu){
    sendTo('telegram', {
        user: user,
        text: topText,
        editMessageText: {
            options: {
                chat_id: getState("telegram.0.communicate.requestChatId").val,
                message_id: getState("telegram.0.communicate.requestMessageId").val,
                parse_mode: 'markdown',
                reply_markup: menu
            }
        }
    });
}

function waitConfirmCommand(obj, command, timeout, ack = false){
    var mySubscription;

    var timeID = setTimeout(() => {
        unsubscribe(mySubscription);
        CommandDone('Не выполнено!');
    }, timeout);

    mySubscription = on({id: obj, change: 'any'}, function (data) {
        // unsubscribe after first trigger
        if (ack === true)
            if (data.state.ack) ack = false;
        if (!ack) {
            updateMenuButton(user, funcTopText(command), menu_current);
            unsubscribe(mySubscription);
            clearTimeout(timeID);
            CommandDone('Выполнено!');
        }
    });
}

function CommandDone(text){
    if (text === '') text = "Выполнено!";
    sendTo('telegram', {
        user: user,
        answerCallbackQuery: {
            text: text,
            showAlert: false
        }
    });
}

function getButtonArray(obj, keyName, Name) {
    var result = [];
    for (var attr in obj) {
        if (obj[attr] && typeof obj[attr] === 'object') {
            result = result.concat(getButtonArray(obj[attr], keyName, Name));
        }
        if (attr === keyName && obj[attr] === Name) {
            result.push(obj.button);
        }
    }
    return result;
}

function stateSelection(state){
    if ((state.val === false) || (state.val === 'false')) return "ВЫКЛ. в " + formatDate(state.lc, "SS:mm:ss TT.MM");
    if ((state.val === true) || (state.val === 'true')) return "ВКЛ. в " + formatDate(state.lc, "SS:mm:ss TT.MM");
    return "неопределено";
}

function funcTopText (command){
    var text = command;
    switch (command) {
        case 'Меню':
            text =  "*Зал*: ?️ " + getState("mysensors.0.70.3_TEMP.V_TEMP").val + "°C, \n"
            + "?  ? 23.5 градуса \n";
            break;
        case 'Кухня':
            text =  "? " + stateSelection(getState("javascript.0.Test.Switch.command")) + "\n";
            break;
        case 'Кухня ?':
            text =  "? " + stateSelection(getState("javascript.0.Test.Switch.command")) + "\n";
            break;
    }
    return text;
}
```

javascript [Копировать]()

## Лайфхак

Через <b>@botFather</b> можно добавить команду вызова меню 

Тогда для вызова меню будет достаточно ввести символ <b>/</b> и в выпадающем меню выбрать <b>/buttons</b>. В результате появится кнопочка вызова меню рядом с меню смайликов.
[Роман Б. (Haba)](https://sprut.ai/client/article/author/1375) Россия, ст-ца. Динская[@Habaaaa](https://t.me/Habaaaa) [Отблагодарить автора](https://sprut.ai/client/user/signin)


