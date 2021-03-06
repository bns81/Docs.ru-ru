---
title: "Методы и представления контроллера"
author: rick-anderson
description: "Работа с методами, представлениями контроллера и DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a>Методы и представления контроллера

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения Movie, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](working-with-sql/_static/m55.png)

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите **> Быстрые действия и операции рефакторинга**.

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](controller-methods-views/_static/qa.png)


Выберите `using System.ComponentModel.DataAnnotations;`

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](controller-methods-views/_static/da.png)

  Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.

Удалим операторы `using`, которые не требуются. По умолчанию они отображаются светло-серым шрифтом. Щелкните правой кнопкой мыши файл *Movie.cs* и выберите **> Удалить и сортировать операторы using**.

![Удалить и сортировать операторы using](controller-methods-views/_static/rm.png)

Обновленный код выглядит следующим образом:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Назад](working-with-sql.md)
[Вперед](search.md)  
