---
title: "Добавление проверки"
author: rick-anderson
description: "Практическое руководство. Добавление проверки на страницу Razor"
keywords: "ASP.NET Core,проверка,DataAnnotations,Razor,страницы Razor"
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 734dad7778eba41780f9d3ac0685879687288d47
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2017
---
# <a name="adding-validation-to-a-razor-page"></a><span data-ttu-id="a9214-104">Добавление проверки на страницу Razor</span><span class="sxs-lookup"><span data-stu-id="a9214-104">Adding validation to a Razor Page</span></span>

<span data-ttu-id="a9214-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="a9214-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9214-106">В этом разделе к модели `Movie` добавляется логика проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-106">In this section validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="a9214-107">Правила проверки применяются каждый раз, когда пользователь создает или редактирует фильм.</span><span class="sxs-lookup"><span data-stu-id="a9214-107">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="a9214-108">Проверка</span><span class="sxs-lookup"><span data-stu-id="a9214-108">Validation</span></span>

<span data-ttu-id="a9214-109">Ключевой принцип разработки программного обеспечения называется [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (от английского "**D**on't **R**epeat **Y**ourself" — не повторяйся).</span><span class="sxs-lookup"><span data-stu-id="a9214-109">A key tenet of software development is called [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="a9214-110">При разработке страниц Razor Pages рекомендуется задавать любые функциональные возможности лишь один раз и затем при необходимости отражать их в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="a9214-110">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="a9214-111">Этот принцип направлен на сокращение объема кода в приложении.</span><span class="sxs-lookup"><span data-stu-id="a9214-111">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="a9214-112">Следуя ему, вы снижаете вероятность возникновения ошибки в коде и упрощаете его тестирование и поддержку.</span><span class="sxs-lookup"><span data-stu-id="a9214-112">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="a9214-113">Ярким примером применения принципа "Не повторяйся" является поддержка проверки, реализуемая на страницах Razor и на платформе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a9214-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="a9214-114">Правила проверки декларативно определяются в одном месте (в классе модели) и затем применяются в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="a9214-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="a9214-115">Добавление правил проверки к модели фильма</span><span class="sxs-lookup"><span data-stu-id="a9214-115">Adding validation rules to the movie model</span></span>

<span data-ttu-id="a9214-116">Откройте файл *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a9214-116">Open the *Movie.cs* file.</span></span> <span data-ttu-id="a9214-117">Класс [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) предоставляет набор встроенных атрибутов проверки, которые декларативно применяются к классу или свойству.</span><span class="sxs-lookup"><span data-stu-id="a9214-117">[DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="a9214-118">Кроме того, DataAnnotations содержит атрибуты форматирования (такие как `DataType`), которые обеспечивают форматирование и не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-118">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="a9214-119">Обновите класс `Movie`, чтобы использовать преимущества атрибутов проверки `Required`, `StringLength`, `RegularExpression` и `Range`.</span><span class="sxs-lookup"><span data-stu-id="a9214-119">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

<span data-ttu-id="a9214-120">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="a9214-120">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span></span>

<span data-ttu-id="a9214-121">Атрибуты проверки задают поведение, которое применяется к свойствам модели.</span><span class="sxs-lookup"><span data-stu-id="a9214-121">Validation attributes specify behavior that is enforced on model properties.</span></span> <span data-ttu-id="a9214-122">Атрибуты `Required` и `MinimumLength` указывают, что свойство должно иметь значение. Тем не менее, чтобы удовлетворить ограничениям проверки, пользователю достаточно ввести пробел.</span><span class="sxs-lookup"><span data-stu-id="a9214-122">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy the validation constraint.</span></span> <span data-ttu-id="a9214-123">Атрибут `RegularExpression` ограничивает набор допустимых для ввода символов.</span><span class="sxs-lookup"><span data-stu-id="a9214-123">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="a9214-124">В приведенном выше коде в полях `Genre` и `Rating` можно использовать только буквы (пробелы, числа и специальные символы не допускаются).</span><span class="sxs-lookup"><span data-stu-id="a9214-124">In the preceding code, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="a9214-125">Атрибут `Range` ограничивает диапазон значений.</span><span class="sxs-lookup"><span data-stu-id="a9214-125">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="a9214-126">Атрибут `StringLength` задает максимальную и при необходимости минимальную длину строки.</span><span class="sxs-lookup"><span data-stu-id="a9214-126">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> <span data-ttu-id="a9214-127">[Типы значений](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (например, `decimal`, `int`, `float`, `DateTime`) по своей природе являются обязательными и не требуют атрибута `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="a9214-127">[Value types](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="a9214-128">Наличие правил проверки, которые автоматически применяются ASP.NET Core, помогает повысить степень надежности приложения.</span><span class="sxs-lookup"><span data-stu-id="a9214-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="a9214-129">Автоматическая проверка моделей помогает защитить приложение, поскольку вам не приходится беспокоиться о самостоятельной проверке добавляемого кода.</span><span class="sxs-lookup"><span data-stu-id="a9214-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="a9214-130">Пользовательский интерфейс проверки ошибок на страницах Razor</span><span class="sxs-lookup"><span data-stu-id="a9214-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="a9214-131">Запустите приложение и перейдите в раздел "Pages/Movies" (Страницы/фильмы).</span><span class="sxs-lookup"><span data-stu-id="a9214-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="a9214-132">Щелкните ссылку **Create New** (Создать).</span><span class="sxs-lookup"><span data-stu-id="a9214-132">Select the **Create New** link.</span></span> <span data-ttu-id="a9214-133">Введите в форму какие-либо недопустимые значения.</span><span class="sxs-lookup"><span data-stu-id="a9214-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="a9214-134">Если функция проверки jQuery на стороне клиента обнаруживает ошибку, сведения о ней отображаются в соответствующем сообщении.</span><span class="sxs-lookup"><span data-stu-id="a9214-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Форма просмотра фильма с несколькими ошибками проверки jQuery на стороне клиента](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="a9214-136">В поле `Price` нельзя вводить десятичные точки или запятые.</span><span class="sxs-lookup"><span data-stu-id="a9214-136">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="a9214-137">Чтобы обеспечить поддержку [проверки jQuery](http://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (","), а для отображения данных в форматах для других языков, кроме английского, выполните действия, необходимые для глобализации вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="a9214-137">To support [jQuery validation](http://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="a9214-138">Дополнительные сведения см. в разделе [Дополнительные ресурсы](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="a9214-138">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="a9214-139">А пока вводите целые числа, такие как 10.</span><span class="sxs-lookup"><span data-stu-id="a9214-139">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="a9214-140">Обратите внимание, что для каждого поля, содержащего недопустимое значение, в форме автоматически отображается сообщение об ошибке проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-140">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="a9214-141">Эти ошибки применяются как на стороне клиента (с помощью JavaScript и jQuery), так и на стороне сервера (если пользователь отключает JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a9214-141">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="a9214-142">Основным преимуществом является то, что на страницах создания или редактирования **не требуется** изменять код.</span><span class="sxs-lookup"><span data-stu-id="a9214-142">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="a9214-143">После применения класса DataAnnotations к модели активируется пользовательский интерфейс проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-143">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="a9214-144">На страницах Razor, создаваемых в рамках этого руководства, автоматически применяются правила проверки (для этого к свойствам класса модели `Movie` применяются атрибуты).</span><span class="sxs-lookup"><span data-stu-id="a9214-144">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="a9214-145">При проверке страницы редактирования применяются те же правила.</span><span class="sxs-lookup"><span data-stu-id="a9214-145">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="a9214-146">Данные формы передаются на сервер только после того, как будут устранены любые ошибки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a9214-146">The form data is not posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="a9214-147">Чтобы убедиться, что данные формы не отправляются, используйте любой из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="a9214-147">Verify form data is not posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="a9214-148">Поместите точку останова в метод `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a9214-148">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="a9214-149">Отправьте форму с помощью команды **Create** (Создать) или **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="a9214-149">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="a9214-150">Точка останова не достигается ни при каких обстоятельствах.</span><span class="sxs-lookup"><span data-stu-id="a9214-150">The break point is never hit.</span></span>
* <span data-ttu-id="a9214-151">Используйте [инструмент Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="a9214-151">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="a9214-152">Проследите сетевой трафик с помощью инструментов разработчика для браузера.</span><span class="sxs-lookup"><span data-stu-id="a9214-152">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="a9214-153">Проверка на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="a9214-153">Server-side validation</span></span>

<span data-ttu-id="a9214-154">Если в браузере отключен JavaScript, форма с ошибками отправляется на сервер.</span><span class="sxs-lookup"><span data-stu-id="a9214-154">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="a9214-155">Реализация проверки на стороне сервера:</span><span class="sxs-lookup"><span data-stu-id="a9214-155">Optional, test server-side validation:</span></span>

* <span data-ttu-id="a9214-156">Отключите JavaScript в браузере.</span><span class="sxs-lookup"><span data-stu-id="a9214-156">Disable JavaScript in the browser.</span></span> <span data-ttu-id="a9214-157">Если сделать это не удается, попробуйте использовать другой браузер.</span><span class="sxs-lookup"><span data-stu-id="a9214-157">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="a9214-158">Поместите точку останова в метод `OnPostAsync` страниц создания или редактирования.</span><span class="sxs-lookup"><span data-stu-id="a9214-158">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="a9214-159">Отправьте форму с ошибками проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-159">Submit a form with validation errors.</span></span>
* <span data-ttu-id="a9214-160">Проверка недопустимого состояния модели:</span><span class="sxs-lookup"><span data-stu-id="a9214-160">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="a9214-161">В следующем коде демонстрируется часть страницы *Create.cshtml*, сформированной ранее в рамках этого руководства.</span><span class="sxs-lookup"><span data-stu-id="a9214-161">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="a9214-162">Она используется на страницах создания и редактирования для отображения исходной формы и повторного вывода формы в случае ошибки.</span><span class="sxs-lookup"><span data-stu-id="a9214-162">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

<span data-ttu-id="a9214-163">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]</span><span class="sxs-lookup"><span data-stu-id="a9214-163">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]</span></span>

<span data-ttu-id="a9214-164">[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms) использует атрибуты [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) и создает HTML-атрибуты, необходимые для проверки jQuery на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a9214-164">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="a9214-165">[Вспомогательная функция тега Validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) отображает ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-165">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="a9214-166">Дополнительные сведения см. в разделе [Проверка](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="a9214-166">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="a9214-167">На страницах создания и редактирования не определены правила проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-167">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="a9214-168">Правила проверки и строки ошибок указываются только в классе `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a9214-168">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="a9214-169">Они автоматически применяются к страницам Razor, которые редактируют модель `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a9214-169">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="a9214-170">Любые необходимые изменения логики проверки осуществляются исключительно в модели.</span><span class="sxs-lookup"><span data-stu-id="a9214-170">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="a9214-171">Проверка применяется согласованно на уровне всего приложения, для чего логика проверки определяется в одном месте.</span><span class="sxs-lookup"><span data-stu-id="a9214-171">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="a9214-172">Такой подход позволяет максимально оптимизировать код и упростить его поддержку и обновление.</span><span class="sxs-lookup"><span data-stu-id="a9214-172">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="a9214-173">Использование атрибутов DataType</span><span class="sxs-lookup"><span data-stu-id="a9214-173">Using DataType Attributes</span></span>

<span data-ttu-id="a9214-174">Проверьте класс `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a9214-174">Examine the `Movie` class.</span></span> <span data-ttu-id="a9214-175">В пространстве имен `System.ComponentModel.DataAnnotations` в дополнение к набору встроенных атрибутов проверки предоставляются атрибуты форматирования.</span><span class="sxs-lookup"><span data-stu-id="a9214-175">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="a9214-176">Атрибут `DataType` применяется к свойствам `ReleaseDate` и `Price`.</span><span class="sxs-lookup"><span data-stu-id="a9214-176">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

<span data-ttu-id="a9214-177">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="a9214-177">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span></span>

<span data-ttu-id="a9214-178">Атрибуты `DataType` предоставляют модулю просмотра только рекомендации по форматированию данных, а также другие атрибуты, например `<a>` для URL-адресов и `<a href="mailto:EmailAddress.com">` для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="a9214-178">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="a9214-179">Используйте атрибут `RegularExpression` для проверки формата данных.</span><span class="sxs-lookup"><span data-stu-id="a9214-179">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="a9214-180">Атрибут `DataType` позволяет указать тип данных с более точным определением относительно встроенного типа базы данных.</span><span class="sxs-lookup"><span data-stu-id="a9214-180">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="a9214-181">Атрибуты `DataType` не предназначены для проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-181">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="a9214-182">В том же приложении отображается только дата (без времени).</span><span class="sxs-lookup"><span data-stu-id="a9214-182">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="a9214-183">В перечислении `DataType` представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и других.</span><span class="sxs-lookup"><span data-stu-id="a9214-183">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="a9214-184">Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="a9214-184">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="a9214-185">Например, можно создавать ссылку `mailto:` для `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="a9214-185">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="a9214-186">Для `DataType.Date` в браузерах с поддержкой HTML5 можно предоставить селектор даты.</span><span class="sxs-lookup"><span data-stu-id="a9214-186">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="a9214-187">Атрибут `DataType` создает атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="a9214-187">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="a9214-188">Атрибуты `DataType` **не предназначены** для проверки.</span><span class="sxs-lookup"><span data-stu-id="a9214-188">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="a9214-189">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="a9214-189">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="a9214-190">По умолчанию поле данных отображается с использованием форматов, установленных в параметрах `CultureInfo` сервера.</span><span class="sxs-lookup"><span data-stu-id="a9214-190">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="a9214-191">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="a9214-191">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="a9214-192">Параметр `ApplyFormatInEditMode` указывает, что формат должен применяться при отображении значения для редактирования.</span><span class="sxs-lookup"><span data-stu-id="a9214-192">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="a9214-193">Для некоторых полей такое поведение нежелательно.</span><span class="sxs-lookup"><span data-stu-id="a9214-193">You might not want that behavior for some fields.</span></span> <span data-ttu-id="a9214-194">Например, в полях валюты в пользовательском интерфейсе редактирования использовать символ денежной единицы, как правило, не требуется.</span><span class="sxs-lookup"><span data-stu-id="a9214-194">For example, in currency values, you probably do not want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="a9214-195">Атрибут `DisplayFormat` может использоваться отдельно, однако чаще всего его рекомендуется применять вместе с атрибутом `DataType`.</span><span class="sxs-lookup"><span data-stu-id="a9214-195">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="a9214-196">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран) и дает следующие преимущества по сравнению с атрибутом DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="a9214-196">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="a9214-197">Поддержка функций HTML5 в браузере (отображение элемента управления календарем, соответствующего языковому стандарту символа валюты, ссылок электронной почты и т. д.)</span><span class="sxs-lookup"><span data-stu-id="a9214-197">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="a9214-198">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="a9214-198">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="a9214-199">Атрибут `DataType` обеспечивает поддержку платформы ASP.NET Core для выбора соответствующего шаблона поля, применяемого при отображении данных.</span><span class="sxs-lookup"><span data-stu-id="a9214-199">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="a9214-200">При отдельном использовании атрибут `DisplayFormat` базируется на строковом шаблоне.</span><span class="sxs-lookup"><span data-stu-id="a9214-200">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="a9214-201">Примечание. Проверка jQuery не работает с атрибутами `Range` и `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="a9214-201">Note: jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="a9214-202">Например, следующий код всегда приводит к возникновению ошибки проверки на стороне клиента, даже если дата попадает в указанный диапазон:</span><span class="sxs-lookup"><span data-stu-id="a9214-202">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="a9214-203">Как правило, не рекомендуется компилировать модели с фиксированными датами, поэтому использовать атрибуты `Range` и `DateTime` следует крайне осторожно.</span><span class="sxs-lookup"><span data-stu-id="a9214-203">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="a9214-204">В следующем коде демонстрируется объединение атрибутов в одной строке:</span><span class="sxs-lookup"><span data-stu-id="a9214-204">The following code shows combining attributes on one line:</span></span>

<span data-ttu-id="a9214-205">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="a9214-205">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span></span>

<span data-ttu-id="a9214-206">Благодарим вас за изучение общих сведений о страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="a9214-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="a9214-207">Мы будем рады любым комментариям.</span><span class="sxs-lookup"><span data-stu-id="a9214-207">We appreciate any comments you leave.</span></span> <span data-ttu-id="a9214-208">Отличным дополнением к этому учебнику является статья [Начало работы с EF и MVC](xref:data/ef-mvc/intro).</span><span class="sxs-lookup"><span data-stu-id="a9214-208">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9214-209">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a9214-209">Additional resources</span></span>

* [<span data-ttu-id="a9214-210">Работа с формами</span><span class="sxs-lookup"><span data-stu-id="a9214-210">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="a9214-211">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="a9214-211">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="a9214-212">Общие сведения о вспомогательных функциях тегов</span><span class="sxs-lookup"><span data-stu-id="a9214-212">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="a9214-213">Создание вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="a9214-213">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)

>[!div class="step-by-step"]
[<span data-ttu-id="a9214-214">Предыдущая тема. Добавление нового поля</span><span class="sxs-lookup"><span data-stu-id="a9214-214">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)