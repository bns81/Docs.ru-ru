---
title: "Добавление нового поля на страницу Razor"
author: rick-anderson
description: "Демонстрирует, как добавить новое поле на страницу Razor с помощью Entity Framework Core"
keywords: "ASP.NET Core,Entity Framework Core,миграция"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 128b69513976a56104524bb803f2b8cb1daf1967
ms.sourcegitcommit: 79bbe7481c3d1297a0db8e41dd2b635b0f778264
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a>Добавление нового поля на страницу Razor

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом разделе вы будете использовать [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations для добавления нового поля к модели и переноса этого изменения в базу данных.

Если вы используете EF Code First для автоматического создания базы данных, Code First добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана. Если синхронизация нарушена, EF вызывает исключение. Это позволяет упростить поиск проблем с согласованностью между базой данных и кодом.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Добавление свойства Rating в модель Movie

Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Постройте приложение (CTRL+SHIFT+B).

Измените файл *Pages/Movies/Index.cshtml* и добавьте в него поле `Rating`:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Добавьте поле `Rating` на страницы "Delete" (Удаление) и "Details" (Сведения).

Обновите файл *Create.cshtml*, добавив в него поле `Rating`. Вы можете скопировать и вставить предыдущий элемент `<div>` и дождаться автоматического обновления полей с помощью IntelliSense. IntelliSense работает со [вспомогательными функциями тегов](xref:mvc/views/tag-helpers/intro).

![Разработчик ввел букву R в качестве значения атрибута asp-for во втором элементе label представления. Появились контекстные меню Intellisense, в которых представлены доступные поля, в том числе Rating, выделяемое в списке автоматически. Если разработчик щелкнет поле или нажмет клавишу ВВОД, полю Rating будет присвоено значение.](new-field/_static/cr.png)

В следующем коде показан файл *Create.cshtml* с полем `Rating`:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Добавьте поле `Rating` на страницу "Edit" (Редактирование).

Для работы приложения необходимо обновить базу данных, включив в нее новое поле. Если запустить приложение сейчас, возникнет исключение `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в базе данных. (В таблице базы данных отсутствует столбец `Rating`.)

Устранить эту ошибку можно несколькими способами:

1. Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели. Этот подход удобен на ранних стадиях цикла разработки. В этом случае развитие модели и схемы базы данных осуществляется одновременно. Недостатком такого подхода является потеря существующих данных в базе. В рабочей базе данных применять этот подход невозможно. При разработке приложения часто выполняется удаление базы данных при изменении схемы, для чего используется инициализатор для автоматического заполнения базы тестовыми данными.

2. Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели. Преимущество такого подхода состоит в том, что сохраняются все данные. Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.

3. Можно обновить схему базы данных с помощью Code First Migrations.

В этом руководстве используется Code First Migrations.

Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца. Ниже показан пример изменения, которое необходимо выполнить для каждого блока `new Movie`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

См. [готовый файл SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).

Постройте решение.

<a name="pmc"></a> В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.
В PMC введите следующие команды:

```powershell
Add-Migration Rating
Update-Database
```

Команда `Add-Migration` задает следующие инструкции для платформы:

* Сравните модель `Movie` со схемой базы данных `Movie`.
* Создайте код для переноса схемы базы данных в новую модель.

В качестве имени файла переноса используется произвольное имя "Rating". Рекомендуется присваивать этому файлу понятное имя.

<a name="ssox"></a> Если удалить все записи из базы данных, при инициализации она будет заполнена значениями и в нее будет включено поле `Rating`. Это можно сделать с помощью ссылок удаления в браузере или из [обозревателя объектов SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Удаление базы данных из SSOX:

* Выберите базу данных в SSOX.
* Щелкните базу данных правой кнопкой мыши и выберите *Удалить*.
* Выберите **Закрыть существующие соединения**.
* Нажмите кнопку **ОК**.
* Обновите базу данных в [PMC](xref:tutorials/razor-pages/new-field#pmc).

  ```powershell
  Update-Database
  ```

Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`. Если база данных не заполнена начальными значениями, остановите IIS Express и затем запустите приложение.

>[!div class="step-by-step"]
[Предыдущая тема — "Добавление поиска"](xref:tutorials/razor-pages/search)
[Следующая тема — "Добавление проверки"](xref:tutorials/razor-pages/validation)
