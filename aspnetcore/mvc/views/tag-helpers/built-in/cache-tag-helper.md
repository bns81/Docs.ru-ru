---
title: "Кэшировать вспомогательный тег в ядре ASP.NET MVC"
author: pkellner
description: "Показано, как работать с вспомогательный тег кэша"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: da5b7b3bf1aa01ee22edf9bd003d8f79a00a5d0b
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Кэшировать вспомогательный тег в ядре ASP.NET MVC

Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) 


Вспомогательный тег кэша позволяет существенно повысить производительность приложения ASP.NET Core путем кэширования ее содержимое, чтобы внутренний поставщик кэша ASP.NET Core.

Значение по умолчанию задает представлений Razor `expires-after` до 20 минут.

Приведенная ниже разметка Razor кэширует даты и времени:

```cshtml
<Cache>@DateTime.Now<Cache>
```

Первый запрос страницы, содержащей `CacheTagHelper` отобразит текущее значение даты и времени. Дополнительные запросы покажет кэшированное значение, до истечения срока действия (по умолчанию 20 минут) или исключен с нехваткой памяти в кэше.

Можно задать длительность кэширования со следующими атрибутами:

## <a name="cache-tag-helper-attributes"></a>Кэшировать вспомогательные атрибуты тега

- - -

### <a name="enabled"></a>enabled    


| Тип атрибута    | Допустимые значения      |
|----------------   |----------------   |
| boolean           | «true» (по умолчанию)  |
|                   | "false"   |


Определяет, кэшируются ли содержимое, заключенное в тег вспомогательный объект кэша. Значение по умолчанию — `true`.  Если значение `false` этого вспомогательного объекта тег кэша не будет действовать кэширования на выводимых данных.

Пример.

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a>Срок действия истекает в 

| Тип атрибута    | Пример значения     |
|----------------   |----------------   |
| DateTimeOffset    | «@new DateTime(2025,1,29,17,02,0)»    |


Задает абсолютный срок действия. Следующий пример будет кэшировать содержимое тега вспомогательный объект кэша до 17:02:00 на 29 января 2025.

Пример.

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a>После истечения срока действия

| Тип атрибута    | Пример значения     |
|----------------   |----------------   |
| TimeSpan    | " @TimeSpan.FromSeconds (120)"    |


Задает интервал времени с момента первого запроса для кэширования содержимого. 

Пример.

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a>скользящий срок действия истекает

| Тип атрибута    | Пример значения     |
|----------------   |----------------   |
| TimeSpan    | " @TimeSpan.FromSeconds (60)"     |


Задает время, следует удалить запись кэша, если оно не было обращений.

Пример.

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a>различаются по заголовок

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Строка            | «User-Agent»                  |
|                   | «User-Agent, content-encoding» |

Принимает значение одного заголовка или список разделенных запятыми значений заголовка, запускающих обновление кэша, при их изменения. Следующий пример отслеживает значение заголовка `User-Agent`. Пример будет кэшировать содержимое для каждого разные `User-Agent` представлены на веб-сервер.

Пример.

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a>различаются по запроса

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Строка            | «Обеспечить»                |
|                   | «Создание модели» |

Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, при изменении значения заголовка. Следующий пример просматривает значения `Make` и `Model`.

Пример.

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a>различаются по Маршрутизация

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Строка            | «Обеспечить»                |
|                   | «Создание модели» |

Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, когда изменение значения параметра данных маршрута. Пример.

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a>различаются по cookie

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Строка            | ". AspNetCore.Identity.Application»                |
|                   | ". AspNetCore.Identity.Application,HairColor» |

Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, когда изменение (s) значения заголовка. Следующий пример просматривает файл cookie, связанные с ASP.NET Identity. Когда пользователь проходит проверку подлинности запроса куки-файл задать активизирующий обновления кэша.

Пример.

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a>изменяться на уровне пользователей

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Boolean             | "true"                  |
|                     | «false» (по умолчанию) |

Указывает, должна ли сбросить кэш, при изменении пользователя, выполнившего вход (или участника контекста). Текущий пользователь, также называемая участника контекста запроса и можно просмотреть с помощью ссылки на представления Razor `@User.Identity.Name`.

Следующий пример просматривает текущего вошедшего пользователя.  

Пример.

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

С помощью этого атрибута сохраняет содержимое в кэше журнала и журнала цикла.  При использовании `vary-by-user="true"`, журнала и журнала действий делает недействительным кэш для проверенного пользователя.  Кэш становится недействительным, поскольку новое значение уникальный файл cookie создается при входе в систему. Кэш хранится для анонимных состояния при объекты cookie не существует или уже истек. Это означает, что если пользователь не входит в систему, будет поддерживаться кэша.

- - -

### <a name="vary-by"></a>изменяющиеся

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Строка             | " @Model "                 |


Позволяет настраивать Возвращает кэшированные данные. При обновлении объект, упоминаемый в строковое значение атрибута изменяется, содержимое тега вспомогательный объект кэша. Часто объединение строк значений модели назначаются данному атрибуту.  По сути, это означает, что делает кэш недействительным обновления к любому из объединенных значений.

В следующем примере предполагается визуализации представления суммы целочисленное значение между двумя параметрами маршрута метода контроллера `myParam1` и `myParam2`и возвращает, как свойство одной модели. При изменении этой суммы, содержимое тега вспомогательного метода кэша к просмотру и снова кэшируется.  

Пример.

Действие:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a>priority

| Тип атрибута    | Примеры значений                |
|----------------   |----------------               |
| Перечисления CacheItemPriority  | «Высокие»                   |
|                    | «Низкое» |
|                    | «NeverRemove» |
|                    | «Normal» |

Руководство вытеснения кэша поставщику встроенного кэша. Веб-сервер будет вытеснять `Low` первым кэша записи при нехватке памяти.

Пример.

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

`priority` Атрибута не гарантирует определенный уровень хранения кэша. `CacheItemPriority`— только это предложение. Задав этому атрибуту значение `NeverRemove` не гарантирует, что кэша всегда будет сохранено. В разделе [дополнительные ресурсы](#additional-resources) для получения дополнительной информации.

Вспомогательный тег кэша зависит от [памяти служба кэша](xref:performance/caching/memory). Вспомогательный тег кэша добавляет службу, если он не был добавлен.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
