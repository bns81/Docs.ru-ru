---
title: "Knockout.js MVVM Framework в ASP.NET Core"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Knockout.js MVVM Framework в ASP.NET Core

По [Стив Смит](https://ardalis.com/)

Маскирования — это популярных библиотека JavaScript, которая упрощает создание сложных данных пользовательских интерфейсов. Он может использоваться отдельно или совместно с другими библиотеками, например jQuery. Ее основная задача — для привязки элементов пользовательского интерфейса базовую модель данных определяется как объект JavaScript, таким образом, что при внесении изменений в пользовательском Интерфейсе, модель обновляется и наоборот. Маскировать облегчает использование шаблона Model-View-ViewModel (MVVM) в поведении клиентские веб-приложения. Две основные концепции, которые один нужно знать при работе с реализацией MVVM маскирования являются наблюдаемые объекты и привязки.

## <a name="getting-started"></a>Начало работы

Маскировать развертывается как отдельный файл JavaScript, поэтому установка, и его использования является очень простым с использованием [bower](bower.md). При условии, что у вас уже есть [bower](bower.md) и [gulp](using-gulp.md) , откройте *bower.json* в ваш ASP.NET Core проектов и добавить зависимость маскирования, как показано ниже:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

Это место после этого можно вручную выполнить bower, открыв диспетчер выполнения задач (в представлении ‣ ‣ другие окна Task Runner Explorer) в области задач, щелкните правой кнопкой мыши на bower и выберите выполнения. Результат должен выглядеть примерно следующим образом:

![bower работает маскирования в Task Runner Explorer](knockout/_static/bower-knockout.png)

Теперь, если искать в своем проекте `wwwroot` папку, вы увидите маскирования установлены в эту папку.

![устанавливается в папку lib маскирования](knockout/_static/wwwroot-knockout.png)

Рекомендуется в рабочей среде ссылаться маскирования через сеть доставки содержимого или CDN, как это повышает вероятность того, что пользователи уже будет кэшированную копию файла и таким образом не потребуется загрузить его вообще. Маскировать доступна на нескольких CDN, включая Microsoft Ajax CDN, здесь:

[http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Чтобы включить маскирования на странице, которая будет его использовать, просто добавьте `<script>` ссылающийся на файл из везде, где можно будет размещен ее (вместе с приложением или через CDN):

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Наблюдаемые объекты, ViewModels и простая привязка

Вы уже знакомы с использованием JavaScript для управления элементами на веб-странице, либо с помощью прямой доступ к модели DOM или библиотеки, например jQuery. Обычно такое поведение достигается путем написания кода для прямого присвоения значений элементов в ответ на определенные действия пользователя. С помощью Knockout декларативный подход берется вместо, через который элементов на странице, привязанные к свойствам объекта. Вместо написания кода для управления элементами DOM, действия пользователя для взаимодействия с объектом ViewModel просто и маскирования берет на себя убедитесь, что синхронизируются элементы страницы.

В качестве простого примера рассмотрим страницу списка. Он включает `<span>` элемент с `data-bind` атрибут, указывающий, что текстовое содержимое должно быть привязано к authorName. Далее, в блоке JavaScript с единственным свойством определяется переменной viewModel `authorName`, принимает значение. Наконец, вызов `ko.applyBindings` производится, передав в этой переменной viewModel.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

При просмотре в браузере, а содержимое <span> элемент заменяется значением переменной viewModel:

![Простая привязка маскирования](knockout/_static/simple-binding-screenshot.png)

Теперь у нас есть рабочий простой односторонние привязки. Обратите внимание, что нигде в коде мы писали JavaScript для присвоения значения в области содержимого. Если требуется управлять ViewModel, мы можно сделать это шаг дальнейшей и добавьте текстовое поле ввода HTML и, как привязать значение по таким:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Перезагрузить страницу, мы увидим, что это значение действительно привязывается к поле ввода:

![Привязка ввода маскирования](knockout/_static/input-binding-screenshot.png)

Однако если изменить значение в текстовом поле, соответствующее значение в `<span>` элемента не изменяется. Почему?

Проблема в том, ничего не уведомляется `<span>` , он не требуется обновлять. Достаточно обновить ViewModel не сам по себе недостаточно, если свойства ViewModel упаковываются в специальный тип. Необходимо использовать **наблюдаемые объекты** в ViewModel для всех свойств, которые должны иметь изменения автоматически обновляется по мере их появления. Изменив ViewModel использование `ko.observable("value")` вместо просто «value», ViewModel обновит HTML-элементов, привязанных к значению, при каждом изменении. Обратите внимание, что полей ввода не обновлять их значения до потери фокуса, поэтому вы не увидите изменения в привязанных элементов при вводе.

> [!NOTE]
> Добавление поддержки динамическое обновление, после каждого нажатия клавиши — это просто добавить `valueUpdate: "afterkeydown"` для `data-bind` содержимое атрибута. Это поведение можно также получить с помощью `data-bind="textInput: authorName"` мгновенного обновления значений. 

Наш viewModel после ее для использования ko.observable обновления:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Маскировать поддерживает несколько различных видов привязок. Пока мы рассмотрели для привязки к `text` и `value`. Также можно привязать к любой данного атрибута. Например, чтобы создать гиперссылку с тег, `src` атрибута может быть привязано к viewModel. Маскировать также поддерживает привязку к функции. Чтобы продемонстрировать это, давайте обновления viewModel для включения маркера twitter автора и отображать как ссылку на страницу twitter автора маркер twitter. Нам предстоит выполнить в три этапа.

Сначала добавьте HTML для отображения гиперссылки, которой будет показано в скобках после имени автора:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Затем обновите viewModel, чтобы включить свойства twitterUrl и twitterAlias:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Обратите внимание, что на этом этапе мы еще не обновлены twitterUrl, чтобы перейти на правильный URL-адрес для данного псевдонима twitter — просто обращены twitter.com. Также Обратите внимание, что мы используем новая функция маскирования `computed`, для twitterUrl. Это — это наблюдаемый функция, которая будет уведомлять все элементы пользовательского интерфейса, если он изменяет. Тем не менее оно должно иметь доступ к другим свойствам в viewModel, нам нужно изменить, как мы создаем viewModel, таким образом, чтобы каждое свойство собственную инструкцию.

Ниже показан измененный viewModel объявления. Теперь он объявляется как функция. Обратите внимание, что каждое свойство собственную инструкцию теперь заканчивается точкой с запятой. Кроме того, для доступа к значению свойства twitterAlias, необходимо выполнить его, поэтому его ссылка включает в себя ().

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

Результат работы должным образом в браузере.

![Гиперссылка маскирования](knockout/_static/hyperlink-screenshot.png)

Маскировать также поддерживает привязку к определенные события элемент пользовательского интерфейса, такие как события щелчка. Это позволяет легко и декларативно привязывать элементы пользовательского интерфейса для функций внутри приложения viewModel. В качестве простого примера можно добавить кнопку, при нажатии, изменяет автора twitterAlias быть прописными буквами.

Во-первых, мы добавьте кнопки, привязки на кнопку событий, а также ссылки на имя функции, которую мы собираемся добавить viewModel:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Затем добавьте в функцию viewModel и подключить его для изменения состояния viewModel. Обратите внимание, что присваивается новое значение свойства twitterAlias, мы вызвать в качестве метода передайте новое значение.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

Выполнение кода и нажав кнопку изменяет отображаемая ссылка должным образом:

![преобразовать в прописную гиперссылки](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Поток управления

Маскирования содержит привязок, которые могут выполнять операции условного и циклы. Циклические операции особенно полезны для привязки списков данных для пользовательского интерфейса списки, меню и таблицы или таблиц. Привязка по каждому элементу будет применен к массиву. При использовании с массивом наблюдаемый автоматически обновит элементы пользовательского интерфейса, когда элементы добавлены или удалены из массива, без повторного создания каждого элемента в дереве пользовательского интерфейса. В следующем примере используется новый viewModel, включая наблюдаемый массив результатов игры. Он связан с простой таблицы с двумя столбцами с помощью `foreach` привязки для `<tbody>` элемента. Каждый `<tr>` элемент в пределах `<tbody>` будет привязано к элементу коллекции gameResults.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

Обратите внимание, что это время мы используем ViewModel с заглавной буквы «V» так, как ожидается, что для создания с помощью «new» (в вызове applyBindings). При выполнении произведут следующий результат:

![модель представления записей маскирования](knockout/_static/record-screenshot.png)

Для демонстрации функциональности наблюдаемой коллекции, добавим немного более широкими функциональными возможностями. Мы позволяют записывать результаты другого игры ViewModel и затем добавить кнопки и пользовательский Интерфейс для работы с этой новой функции.  Во-первых давайте создадим метод addResult:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Этот метод Bind кнопки с помощью `click` привязки:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Откройте страницу в браузере и нажмите кнопку несколько раз, что приводит к новой строки каждого щелчка:

![Добавьте к результатам](knockout/_static/record-addresult-screenshot.png)

Существует несколько способов для поддержки добавления новых записей в пользовательском Интерфейсе, обычно либо встроенной или в отдельную форму. Можно легко изменять таблицу в текстовые поля и элементами управления DropDownList таким образом, чтобы его для редактирования. Замените `<tr>` элемента, как показано:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Обратите внимание, что `$root` означает корневой каталог ViewModel, который является, где предоставляются возможные варианты. `$data`ссылается на независимо от текущей модели — в данном контексте — в данном случае он относится к отдельному элементу массива resultChoices, каждый из которых представляет собой простую строку.

Благодаря этому изменению всей сетки становится доступным для редактирования:

![Редактируемая сетка](knockout/_static/editable-grid-screenshot.png)

Если мы не использовали маскирования, мы позволяет достигнуть все это с помощью jQuery, но скорее всего не может быть практически столь же эффективна. Маскировать отслеживает какие связанные данные, соответствующие элементы в ViewModel какие элементы пользовательского интерфейса и обновляет только те элементы, которые нужно добавить, удалить или обновить. Может потребоваться значительные усилия, чтобы добиться этого, сами с помощью jQuery или прямого управления DOM и даже если затем мы хотели отображения статистических результатов (например, потери запись) на основе данных таблицы, необходимо еще раз перебор его и выполнять синтаксический анализ HTML-элементов.  С помощью Knockout отображения-потери записи является тривиальным. Мы вычислений в пределах самого ViewModel и его отображения с привязкой простого текста и `<span>`.

Для создания строки потери записи, мы используем вычисляемый наблюдаемым. Обратите внимание, что ссылки на наблюдаемый свойства ViewModel должны быть вызовы функций, в противном случае они не получит значение наблюдаемым (т. е. `gameResults()` не `gameResults` в коде, показанном):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Привязки к диапазону в пределах этой функции `<h1>` элемент в верхней части страницы:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

Результат:

![Потери](knockout/_static/record-winloss-screenshot.png)

Добавление строк или изменения выбранного элемента в любой строке столбца результирующего будет обновить запись в верхней части окна.

Помимо привязки к значениям также можно использовать практически любой юридических выражение JavaScript, в привязке. Например если элемент пользовательского интерфейса отображается только при определенных условиях, например, если значение превышает определенное пороговое значение, можно указать это логически в выражение привязки:

```html
<div data-bind="visible: customerValue > 100"></div>
```

Это `<div>` будет показана только при customerValue — более 100.

## <a name="templates"></a>Шаблоны

Маскировать включает поддержку для шаблонов, таким образом, можно легко разделить пользовательского интерфейса из поведение или добавочной загрузки элементов пользовательского интерфейса в большого приложения по требованию. Корпорация Майкрософт может обновлять наш пример для создания своего собственного шаблона каждой строки, просто помещает удаление HTML в шаблон и указание шаблона по имени в вызове привязка к данным на `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Маскировать также поддерживает другие механизмы шаблонов, таких как библиотека jQuery.tmpl и Underscore.js в модуль шаблонов.

## <a name="components"></a>Компоненты

Компоненты позволяют упорядочивать и повторно использовать код пользовательского интерфейса, обычно вместе с данными ViewModel, от которого зависит ее код пользовательского интерфейса. Чтобы создать компонент, нужно просто укажите его шаблона и ее viewModel и присвойте ему имя. Это выполняется с помощью вызова метода `ko.components.register()`. Кроме определения шаблонов и встроенные viewmodel, их можно загрузить из внешних файлов с помощью библиотеки, такой как *require.js*, что может привести очень простого и эффективного кода.

## <a name="communicating-with-apis"></a>Взаимодействие с интерфейсами API

Маскировать можно работать с данными в формате JSON. Распространенным способом для получения и сохранения данных с помощью маскирования является jQuery, который поддерживает `$.getJSON()` функции для получения данных и `$.post()` метод отправки данных из браузера на конечную точку API. Конечно при желании способ отправки и получения данных JSON маскирования будет работать с ним также.

## <a name="summary"></a>Сводка

Маскировать обеспечивает простое и элегантное способ привязывать элементы пользовательского интерфейса в текущее состояние клиентского приложения, определенные в ViewModel. Синтаксис привязки маскирования использует атрибут привязка к данным, применяется к элементам HTML, которые должны быть обработаны. Маскирования может эффективно отображения и обновить больших наборов данных, отслеживание элементов пользовательского интерфейса, и только обработкой изменений затронутых элементов. Большие приложения можно разбить логика пользовательского интерфейса с помощью шаблонов и компонентов, которые могут загружаться по запросу из внешних файлов. В настоящее время версии 3 маскирования является стабильной библиотека JavaScript, которая может улучшить веб-приложений, в которых требуется взаимодействие клиентских.
