---
title: "Включение запросы независимо от источника (CORS)"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 0d1c34f5c82fe1a28a405617ea77084db899139b
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="enabling-cross-origin-requests-cors"></a>Включение запросы независимо от источника (CORS)

По [Mike Wasson](https://github.com/mikewasson), [Бойера Shayne](https://twitter.com/spboyer), и [Tom Dykstra](https://github.com/tdykstra)

Безопасность обозревателя предотвращает внесение запросы AJAX в другой домен на веб-странице. Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт. Однако иногда может потребоваться разрешить другим узлам, которые делают запросы независимо от источника веб-API.

[Кросс-общий доступ к ресурсам источника](http://www.w3.org/TR/cors/) (CORS) — это стандарт W3C, которая позволяет серверу ослабить политика одного источника. С помощью CORS, сервер можно явно разрешить некоторые запросы независимо от источника при отклонении другим пользователям. CORS является более безопасным и более гибким, чем ранее методов, например [JSONP](https://wikipedia.org/wiki/JSONP). В этом разделе показано, как включить CORS в приложении ASP.NET Core.

## <a name="what-is-same-origin"></a>Что такое «того же происхождения»?

Два URL-адреса имеют того же источника, если они имеют одинаковые схемы, узлов и портов. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Эти два URL-адреса имеют того же источника:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Эти URL-адреса имеют различные источники, чем предыдущий два:

* `http://example.net`-Другой домен

* `http://www.example.com/foo.html`-Другой поддомен

* `https://example.com/foo.html`-Другой схемы

* `http://example.com:9000/foo.html`-Другой порт

> [!NOTE]
> При сравнении источников Internet Explorer не учитывает порт.

## <a name="setting-up-cors"></a>Настройка CORS

Чтобы добавить CORS для вашего приложения `Microsoft.AspNetCore.Cors` пакета в проект.

Добавление служб CORS в файле Startup.cs:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>Включение CORS с по промежуточного слоя

Чтобы включить CORS для всего приложения добавьте по промежуточного слоя CORS в конвейер запроса с помощью `UseCors` метода расширения. Обратите внимание, что по промежуточного слоя CORS должно предшествовать определенные конечные точки в приложении, который требуется поддерживать запросы независимо от источника (например,). Перед вызовом любого метода `UseMvc`).

Можно задать политику независимо от источника, при добавлении по промежуточного слоя CORS с помощью `CorsPolicyBuilder` класса. Это можно сделать двумя способами. Первый — вызов UseCors с лямбда-выражения:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Примечание:** URL-адрес должен быть указан без косой чертой (`/`). Если URL-адрес завершается с `/`, сравнение будет возвращать `false` и будет возвращаться без заголовка.

Лямбда-выражение принимает `CorsPolicyBuilder` объекта. Вы найдете список [параметры конфигурации](#cors-policy-options) далее в этом разделе. В этом примере политика разрешает запросы независимо от источника от `http://example.com` и другие источники.

Обратите внимание, что CorsPolicyBuilder fluent API, поэтому можно соединить в цепочку вызовы методов:

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

Второй подход заключается в том, чтобы определить именованный политики CORS, а затем выберите политику по имени во время выполнения.

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

В этом примере добавляется политика CORS с именем «AllowSpecificOrigin». Чтобы выбрать политику, передать имя для `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Включение CORS в MVC

Также можно использовать MVC для применения определенных CORS каждого действия каждого контроллера или глобально для всех контроллеров. При использовании MVC для включения CORS используются те же службы CORS, но не по промежуточного слоя CORS.

### <a name="per-action"></a>Каждого действия

Чтобы задать политику CORS для определенных действий добавьте `[EnableCors]` атрибут действия. Укажите имя политики.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>На каждый контроллер

Чтобы задать политику CORS для определенного контроллера добавьте `[EnableCors]` атрибут в класс контроллера. Укажите имя политики.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Глобально

Можно включить CORS глобально для всех контроллеров, добавив `CorsAuthorizationFilterFactory` фильтр в глобальную коллекцию фильтров:

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Очередность выполнения:: действия контроллера, глобальные. Политики на уровне действия имеют приоритет над политиками уровня контроллера, и уровня контроллера политики имеют приоритет над глобальные политики.

### <a name="disable-cors"></a>Отключить CORS

Чтобы отключить CORS для контроллера или действия, используйте `[DisableCors]` атрибута.

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>Параметры политики CORS

В этом разделе описываются различные параметры, которые можно задать в политику CORS.

* [Задайте разрешенные источники](#set-the-allowed-origins)

* [Набор разрешенных методов HTTP](#set-the-allowed-http-methods)

* [Задать заголовки запросов](#set-the-allowed-request-headers)

* [Задать заголовки ответа предоставляется](#set-the-exposed-response-headers)

* [Учетные данные в запросы независимо от источника](#credentials-in-cross-origin-requests)

* [Задайте предварительный истечения срока действия](#set-the-preflight-expiration-time)

Для некоторых параметров может оказаться удобным для чтения [работает как CORS](#how-cors-works) первой.

### <a name="set-the-allowed-origins"></a>Задайте разрешенные источники

Чтобы разрешить один или несколько конкретных источников:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Чтобы разрешить все источники:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Внимательно рассмотрите перед предоставлением запросов из любого источника. Он означает, что практически любой веб-сайта можно вносить вызовы AJAX к вашему API.

### <a name="set-the-allowed-http-methods"></a>Набор разрешенных методов HTTP

Чтобы разрешить все методы HTTP:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Это влияет на возможность предварительного запросов и -методы управления доступом — разрешить заголовок.

### <a name="set-the-allowed-request-headers"></a>Задать заголовки запросов

Предварительный запрос CORS может включать заголовок Access-Control-Request-Headers список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).

Белый список указанные заголовки:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

Чтобы разрешить все создавать заголовки запроса:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Браузеры не полностью соответствуют в установке Access-Control-Request-Headers. Если задать заголовки на что-либо отличное от «*», следует включать по крайней мере «принять,» «content-type» и «источник», а также любые пользовательские заголовки, которые требуется поддерживать.

### <a name="set-the-exposed-response-headers"></a>Задать заголовки ответа предоставляется

По умолчанию браузер не предоставляет все заголовки ответа для приложения. (См. [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Заголовки ответа, которые доступны по умолчанию являются:

* Cache-Control

* Content-Language

* Тип содержимого

* Срок действия истекает

* Дата последнего изменения

* Директивы pragma

Спецификация CORS вызывает эти *заголовки ответа на простой*. Чтобы сделать другие заголовки, доступны для приложения.

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Учетные данные в запросы независимо от источника

Учетные данные, требующие особых действий в запрос CORS. По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника. Учетные данные включают файлы cookie, а также схемы проверки подлинности HTTP. Для отправки учетных данных с помощью запроса независимо от источника, клиент должен задать XMLHttpRequest.withCredentials значение true.

Непосредственно с помощью XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

В jQuery:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Кроме того сервер необходимо разрешить учетные данные. Чтобы разрешить учетные данные независимо от источника:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Теперь в HTTP-ответе будет включать доступ-элемент управления-Allow-Credentials заголовок, который предписывает браузеру, поддерживает ли сервер учетные данные для запроса независимо от источника.

Если браузер отправляет учетные данные, но ответа не содержит допустимый заголовок доступа-элемент управления-Allow-Credentials, браузер не предоставляет приложению ответ и происходит сбой запроса AJAX.

Следует с осторожностью позволяя учетных данных независимо от источника, так как это означает, что веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя в приложение от имени пользователя без оповещения пользователя. CORS spec также состояния этого параметра источники, которые можно «*» (все источники) является недопустимым, если присутствует заголовок доступа-элемент управления-Allow-Credentials.

### <a name="set-the-preflight-expiration-time"></a>Задайте предварительный истечения срока действия

Заголовок доступа-элемент управления-Max-Age указывает, как долго ответ на Предварительный запрос может быть кэширован. Для установки этого заголовка:

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>Как работает CORS

В этом разделе описывается, что происходит в запрос CORS, на уровне сообщений HTTP. Важно понять, как работает CORS, чтобы правильно настроить политики CORS и устранения неполадок, если не все работает должным образом.

Спецификация CORS представлены несколько заголовки HTTP, которые позволяют запросы независимо от источника. Если браузер поддерживает CORS, он устанавливает эти заголовки автоматически для запросов независимо от источника. не требуется никаких действий в коде JavaScript.

Ниже приведен пример запроса независимо от источника. Заголовок «Источник» предоставляет домена сайта, который был выполнен запрос:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Если сервер разрешает запрос, он устанавливает заголовка Access-Control-Allow-Origin. Значение этого заголовка соответствует заголовку источника либо является использование подстановочного знака «*», это значит, что разрешены любые источники.:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Если ответ содержит Access-Control-Allow-Origin заголовок, происходит сбой запроса AJAX. В частности браузер блокирует запрос. Даже если сервер возвращает успешный ответ, браузер не делает ответ доступной клиентскому приложению.

### <a name="preflight-requests"></a>Предварительные запросы

Для некоторых запросов CORS браузер отправляет запрос на дополнительные, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса. Браузер может пропустить Предварительный запрос, если выполняются следующие условия:

* Метод запроса является GET, HEAD или POST, и

* Приложение не устанавливает все заголовки запросов, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, и

* Заголовок Content-Type (если задать) является одним из следующих:

  * application/x-www-form-urlencoded

  * данные multipart/формы

  * text/plain.

Заголовки, которые приложение задает путем вызова setRequestHeader на объект XMLHttpRequest применяется правило о заголовках запроса. (Спецификации CORS вызывает эти «автор запроса заголовки»). Правило не применяется к заголовки, которые можно установить браузер, например User-Agent, узлу или Content-Length.

Ниже приведен пример Предварительный запрос:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Предварительный запрос с помощью метода HTTP OPTIONS. Он включает два особых заголовков:

* Access-Control-Request-Method: HTTP метод, который будет использоваться для самого запроса.

* Access-Control-Request-Headers: Список заголовков запросов, которые установлены приложения, для самого запроса. (Опять же, это не включает заголовки, которые задает браузера.)

Ниже приведен пример ответа, при условии, что сервер разрешает запрос:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Ответ включает и-методы управления доступом — разрешить заголовок, который содержит список допустимых методов и при необходимости заголовок Access-Control-разрешить-Headers, в котором перечислены разрешенные заголовки. Если Предварительный запрос завершается успешно, браузер отправляет сам запрос, как описано выше.
