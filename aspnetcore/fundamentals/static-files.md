---
title: "Работа с статических файлов в ASP.NET Core"
author: rick-anderson
description: "Работа с статические файлы"
keywords: "ASP.NET Core, статические файлы, статические активы, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ea6c180332dd5ab3a7238dcd73a4a1c8534c6243
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a><span data-ttu-id="c830f-104">Введение в работу с статических файлов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c830f-104">Introduction to working with static files in ASP.NET Core</span></span>

<a name=fundamentals-static-files></a>

<span data-ttu-id="c830f-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c830f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c830f-106">Статические файлы, такие как HTML, CSS, JavaScript и изображение, активы, которые может обслуживать приложения ASP.NET Core непосредственно на клиентах.</span><span class="sxs-lookup"><span data-stu-id="c830f-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

[<span data-ttu-id="c830f-107">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="c830f-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a><span data-ttu-id="c830f-108">Обработку статических файлов</span><span class="sxs-lookup"><span data-stu-id="c830f-108">Serving static files</span></span>

<span data-ttu-id="c830f-109">Статические файлы обычно хранятся в `web root` (*\<содержимое корневой > / wwwroot*) папки.</span><span class="sxs-lookup"><span data-stu-id="c830f-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="c830f-110">В разделе [содержимое корневого](xref:fundamentals/index#content-root) и [корневого веб-каталога](xref:fundamentals/index#web-root) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="c830f-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="c830f-111">Обычно задать содержимое корневого для текущего каталога, чтобы ваш проект `web root` будет найден во время разработки.</span><span class="sxs-lookup"><span data-stu-id="c830f-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

<span data-ttu-id="c830f-112">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]</span><span class="sxs-lookup"><span data-stu-id="c830f-112">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]</span></span>

<span data-ttu-id="c830f-113">Статические файлы могут храниться в любой папке под `web root` и доступ по относительному пути, корневой каталог.</span><span class="sxs-lookup"><span data-stu-id="c830f-113">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="c830f-114">Например, при создании проект веб-приложения по умолчанию, с помощью Visual Studio существует несколько папок, созданных в *wwwroot* папку - *css*, *изображения*, и *js*.</span><span class="sxs-lookup"><span data-stu-id="c830f-114">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="c830f-115">URI для доступа к изображению в *изображения* вложенную папку:</span><span class="sxs-lookup"><span data-stu-id="c830f-115">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="c830f-116">Чтобы статические файлы к обработке, необходимо настроить [по промежуточного слоя](middleware.md) добавление статических файлов в конвейер.</span><span class="sxs-lookup"><span data-stu-id="c830f-116">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="c830f-117">По промежуточного слоя статических файлов можно настроить путем добавления зависимость на *Microsoft.AspNetCore.StaticFiles* пакета в проект и затем вызова `UseStaticFiles` метод расширения из `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c830f-117">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="c830f-118">[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-118">[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="c830f-119">`app.UseStaticFiles();`устанавливает файлы в `web root` (*wwwroot* по умолчанию) servable.</span><span class="sxs-lookup"><span data-stu-id="c830f-119">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="c830f-120">Далее будет показано как сделать servable с другими содержимое каталога `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="c830f-120">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="c830f-121">Необходимо включить в пакет NuGet «Microsoft.AspNetCore.StaticFiles».</span><span class="sxs-lookup"><span data-stu-id="c830f-121">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="c830f-122">`web root`по умолчанию используется значение *wwwroot* каталога, но можно задать `web root` каталог с `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="c830f-122">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="c830f-123">Предположим, что имеется иерархии проекта, где находятся статические файлы, которые вы хотите использовать за пределами `web root`.</span><span class="sxs-lookup"><span data-stu-id="c830f-123">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="c830f-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="c830f-124">For example:</span></span>

* <span data-ttu-id="c830f-125">wwwroot</span><span class="sxs-lookup"><span data-stu-id="c830f-125">wwwroot</span></span>
  * <span data-ttu-id="c830f-126">css</span><span class="sxs-lookup"><span data-stu-id="c830f-126">css</span></span>
  * <span data-ttu-id="c830f-127">images</span><span class="sxs-lookup"><span data-stu-id="c830f-127">images</span></span>
  * <span data-ttu-id="c830f-128">...</span><span class="sxs-lookup"><span data-stu-id="c830f-128">...</span></span>
* <span data-ttu-id="c830f-129">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="c830f-129">MyStaticFiles</span></span>
  * <span data-ttu-id="c830f-130">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="c830f-130">test.png</span></span>

<span data-ttu-id="c830f-131">Для запроса на доступ к *test.png*, настройте по промежуточного слоя статических файлов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c830f-131">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

<span data-ttu-id="c830f-132">[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-132">[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]</span></span>

<span data-ttu-id="c830f-133">Запрос на `http://<app>/StaticFiles/test.png` будет обслуживать *test.png* файла.</span><span class="sxs-lookup"><span data-stu-id="c830f-133">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="c830f-134">`StaticFileOptions()`можно задать заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="c830f-134">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="c830f-135">Например, приведенный ниже код задает обработку из статических файлов *wwwroot* папок и наборов `Cache-Control` заголовок, чтобы сделать их публично кэшируемый 10 минут (600 секунд):</span><span class="sxs-lookup"><span data-stu-id="c830f-135">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

<span data-ttu-id="c830f-136">[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-136">[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]</span></span>

![Отображение заголовка Cache-Control заголовки ответа были добавлены](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="c830f-138">Авторизация статических файлов</span><span class="sxs-lookup"><span data-stu-id="c830f-138">Static file authorization</span></span>

<span data-ttu-id="c830f-139">Предоставляет статический файл модуля **не** проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="c830f-139">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="c830f-140">Все файлы, обслуживаемых, включая те, в разделе *wwwroot* являются общедоступными.</span><span class="sxs-lookup"><span data-stu-id="c830f-140">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="c830f-141">Чтобы обрабатывать файлы на основе авторизации:</span><span class="sxs-lookup"><span data-stu-id="c830f-141">To serve files based on authorization:</span></span>

* <span data-ttu-id="c830f-142">Сохраните их за пределами *wwwroot* и любой каталог, доступный для по промежуточного слоя статических файлов **и**</span><span class="sxs-lookup"><span data-stu-id="c830f-142">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="c830f-143">Обслуживать их через действия контроллера, возвращая `FileResult` применении авторизации</span><span class="sxs-lookup"><span data-stu-id="c830f-143">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="c830f-144">Включение просмотра каталогов</span><span class="sxs-lookup"><span data-stu-id="c830f-144">Enabling directory browsing</span></span>

<span data-ttu-id="c830f-145">Просмотр каталогов позволяет пользователю просмотреть список каталогов и файлов в указанном каталоге веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c830f-145">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="c830f-146">Обзор каталогов отключен по умолчанию, по соображениям безопасности (см. [вопросы](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="c830f-146">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="c830f-147">Чтобы включить просмотр каталогов, вызовите `UseDirectoryBrowser` метод расширения из `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c830f-147">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

<span data-ttu-id="c830f-148">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-148">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]</span></span>

<span data-ttu-id="c830f-149">И добавить требуемые службы путем вызова `AddDirectoryBrowser` метод расширения из `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c830f-149">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="c830f-150">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c830f-150">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]</span></span>

<span data-ttu-id="c830f-151">Приведенный выше код позволяет просматривать каталог *wwwroot/images* папки с помощью URL-адрес http://\<приложения > / MyImages со ссылками на всех файлов и папок:</span><span class="sxs-lookup"><span data-stu-id="c830f-151">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![Просмотр каталогов](static-files/_static/dir-browse.png)

<span data-ttu-id="c830f-153">В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.</span><span class="sxs-lookup"><span data-stu-id="c830f-153">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="c830f-154">Обратите внимание на два `app.UseStaticFiles` вызовов.</span><span class="sxs-lookup"><span data-stu-id="c830f-154">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="c830f-155">Первый необходимый для обслуживания CSS, изображения и JavaScript в *wwwroot* папки, а второй вызов для просмотра каталога *wwwroot/images* папки с помощью URL-адрес http://\<приложения > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="c830f-155">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

<span data-ttu-id="c830f-156">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-156">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]</span></span>

## <a name="serving-a-default-document"></a><span data-ttu-id="c830f-157">Обслуживает документ по умолчанию</span><span class="sxs-lookup"><span data-stu-id="c830f-157">Serving a default document</span></span>

<span data-ttu-id="c830f-158">Установка домашней страницы по умолчанию предоставляет посетители сайта можно запустить при посещении веб-узла.</span><span class="sxs-lookup"><span data-stu-id="c830f-158">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="c830f-159">Для веб-приложения для обслуживания страницы по умолчанию без участия пользователя полные URI, вызывать `UseDefaultFiles` метод расширения из `Startup.Configure` следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c830f-159">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

<span data-ttu-id="c830f-160">[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-160">[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]</span></span>

> [!NOTE]
> <span data-ttu-id="c830f-161">`UseDefaultFiles`должен быть вызван перед `UseStaticFiles` для обслуживания файла по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c830f-161">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="c830f-162">`UseDefaultFiles`является повторной записи URL-адрес, который фактически не предоставлять этот файл.</span><span class="sxs-lookup"><span data-stu-id="c830f-162">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="c830f-163">Необходимо включить по промежуточного слоя статических файлов (`UseStaticFiles`) для обслуживания файла.</span><span class="sxs-lookup"><span data-stu-id="c830f-163">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="c830f-164">С `UseDefaultFiles`, будет выполнен поиск запросы в папку:</span><span class="sxs-lookup"><span data-stu-id="c830f-164">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="c830f-165">default.htm</span><span class="sxs-lookup"><span data-stu-id="c830f-165">default.htm</span></span>
* <span data-ttu-id="c830f-166">default.html</span><span class="sxs-lookup"><span data-stu-id="c830f-166">default.html</span></span>
* <span data-ttu-id="c830f-167">index.htm</span><span class="sxs-lookup"><span data-stu-id="c830f-167">index.htm</span></span>
* <span data-ttu-id="c830f-168">index.html</span><span class="sxs-lookup"><span data-stu-id="c830f-168">index.html</span></span>

<span data-ttu-id="c830f-169">Первый файл найден в списке будет предоставляться как в случае запроса полный URI (несмотря на то, что URL-адрес браузера будут отображать URI, запрошенный).</span><span class="sxs-lookup"><span data-stu-id="c830f-169">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="c830f-170">Ниже показано, как изменить имя файла по умолчанию для *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="c830f-170">The following code shows how to change the default file name to *mydefault.html*.</span></span>

<span data-ttu-id="c830f-171">[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-171">[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]</span></span>

## <a name="usefileserver"></a><span data-ttu-id="c830f-172">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="c830f-172">UseFileServer</span></span>

<span data-ttu-id="c830f-173">`UseFileServer`объединяет функциональные возможности `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="c830f-173">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="c830f-174">Следующий пример кода позволяет статические файлы и файла для обслуживания, по умолчанию, но не допускает просмотр каталогов:</span><span class="sxs-lookup"><span data-stu-id="c830f-174">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="c830f-175">Следующий пример кода позволяет статических файлов по умолчанию и просмотр каталогов:</span><span class="sxs-lookup"><span data-stu-id="c830f-175">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="c830f-176">В разделе [вопросы](#considerations) на угрозы безопасности при включении просмотра.</span><span class="sxs-lookup"><span data-stu-id="c830f-176">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="c830f-177">Как и в `UseStaticFiles`, `UseDefaultFiles`, и `UseDirectoryBrowser`, если вы хотите предоставлять файлы, которые существуют за пределами `web root`, создать и настроить `FileServerOptions` объекта, передаваемого как параметр `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="c830f-177">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="c830f-178">Например имеется следующая иерархия каталогов в веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="c830f-178">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="c830f-179">wwwroot</span><span class="sxs-lookup"><span data-stu-id="c830f-179">wwwroot</span></span>

  * <span data-ttu-id="c830f-180">css</span><span class="sxs-lookup"><span data-stu-id="c830f-180">css</span></span>

  * <span data-ttu-id="c830f-181">images</span><span class="sxs-lookup"><span data-stu-id="c830f-181">images</span></span>

  * <span data-ttu-id="c830f-182">...</span><span class="sxs-lookup"><span data-stu-id="c830f-182">...</span></span>

* <span data-ttu-id="c830f-183">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="c830f-183">MyStaticFiles</span></span>

  * <span data-ttu-id="c830f-184">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="c830f-184">test.png</span></span>

  * <span data-ttu-id="c830f-185">default.html</span><span class="sxs-lookup"><span data-stu-id="c830f-185">default.html</span></span>

<span data-ttu-id="c830f-186">Использовать приведенный выше пример иерархии, может потребоваться включить статические файлы, файлы по умолчанию и поиске `MyStaticFiles` каталога.</span><span class="sxs-lookup"><span data-stu-id="c830f-186">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="c830f-187">В следующем фрагменте кода, осуществляются с помощью одного вызова `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="c830f-187">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

<span data-ttu-id="c830f-188">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-188">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]</span></span>

<span data-ttu-id="c830f-189">Если `enableDirectoryBrowsing` равно `true` требуется вызывать `AddDirectoryBrowser` метод расширения из `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c830f-189">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="c830f-190">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c830f-190">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]</span></span>

<span data-ttu-id="c830f-191">С помощью иерархии файлов и приведенный выше код:</span><span class="sxs-lookup"><span data-stu-id="c830f-191">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="c830f-192">URI</span><span class="sxs-lookup"><span data-stu-id="c830f-192">URI</span></span>            |                             <span data-ttu-id="c830f-193">Ответ</span><span class="sxs-lookup"><span data-stu-id="c830f-193">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="c830f-194">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="c830f-194">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="c830f-195">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="c830f-195">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="c830f-196">Если нет значения по умолчанию с именем файлы находятся в *MyStaticFiles* каталог, http://\<приложения > / StaticFiles возвращает каталога с интерактивными ссылками:</span><span class="sxs-lookup"><span data-stu-id="c830f-196">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Список статических файлов](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="c830f-198">`UseDefaultFiles`и `UseDirectoryBrowser` будет иметь URL-адрес http://\<приложения > / StaticFiles без завершающей косой черты и причина стороны клиента перенаправления http://\<приложения > /StaticFiles/ (Добавление завершающей косой черты).</span><span class="sxs-lookup"><span data-stu-id="c830f-198">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="c830f-199">Без конечные косая черта относительные URL-адреса в документах будет неверным.</span><span class="sxs-lookup"><span data-stu-id="c830f-199">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="c830f-200">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="c830f-200">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="c830f-201">`FileExtensionContentTypeProvider` Класс содержит коллекцию, которая сопоставляет расширения файлов для типов содержимого MIME.</span><span class="sxs-lookup"><span data-stu-id="c830f-201">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="c830f-202">В следующем образце зарегистрированных несколько расширений файлов для известных типов MIME, заменяется «.rtf» и «.mp4» удаляется.</span><span class="sxs-lookup"><span data-stu-id="c830f-202">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

<span data-ttu-id="c830f-203">[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-203">[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]</span></span>

<span data-ttu-id="c830f-204">В разделе [типы содержимого MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="c830f-204">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="c830f-205">Нестандартные типы содержимого</span><span class="sxs-lookup"><span data-stu-id="c830f-205">Non-standard content types</span></span>

<span data-ttu-id="c830f-206">По промежуточного слоя статических файлов ASP.NET понимает почти 400 типы содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="c830f-206">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="c830f-207">Если пользователь запрашивает файл неизвестного типа, по промежуточного слоя статических файлов возвращает ответ HTTP 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="c830f-207">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="c830f-208">Если просмотр каталогов включен, будет отображаться ссылка на файл, но URI будет возвращать ошибку HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="c830f-208">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="c830f-209">Следующий код позволяет обслуживает неизвестные типы и сделает Неизвестный файл как изображение.</span><span class="sxs-lookup"><span data-stu-id="c830f-209">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

<span data-ttu-id="c830f-210">[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c830f-210">[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]</span></span>

<span data-ttu-id="c830f-211">С приведенный выше код запрашивает файл с Неизвестный тип содержимого будет возвращаться в виде изображения.</span><span class="sxs-lookup"><span data-stu-id="c830f-211">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="c830f-212">Включение `ServeUnknownFileTypes` представляет риск для безопасности и использовать его не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="c830f-212">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="c830f-213">`FileExtensionContentTypeProvider`(описано выше) предоставляет более безопасная альтернатива обслужить файлы с нестандартные расширения.</span><span class="sxs-lookup"><span data-stu-id="c830f-213">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="c830f-214">Особенности</span><span class="sxs-lookup"><span data-stu-id="c830f-214">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="c830f-215">`UseDirectoryBrowser`и `UseStaticFiles` может вызвать утечку секретные данные.</span><span class="sxs-lookup"><span data-stu-id="c830f-215">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="c830f-216">Рекомендуется, чтобы вы **не** directory Включение просмотра в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="c830f-216">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="c830f-217">Будьте внимательны о какие каталоги включения с `UseStaticFiles` или `UseDirectoryBrowser` как весь каталог и все вложенные каталоги будут доступны.</span><span class="sxs-lookup"><span data-stu-id="c830f-217">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="c830f-218">Рекомендуется оставлять открытый содержимое в своем собственном каталоге например  *\<содержимое корневого > / wwwroot*, от представления приложений, файлы конфигурации и т. д.</span><span class="sxs-lookup"><span data-stu-id="c830f-218">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="c830f-219">URL-адреса для содержимого, представлены `UseDirectoryBrowser` и `UseStaticFiles` чувствительность к регистру и ограничения на символы их базовой файловой системы.</span><span class="sxs-lookup"><span data-stu-id="c830f-219">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="c830f-220">Например Windows не учитывает регистр, но Mac и Linux не.</span><span class="sxs-lookup"><span data-stu-id="c830f-220">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="c830f-221">Приложения ASP.NET Core, размещенные в IIS использовать модуль ASP.NET Core для перенаправления всех запросов приложения, включая запросы статических файлов.</span><span class="sxs-lookup"><span data-stu-id="c830f-221">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="c830f-222">Обработчик файла статистики IIS не используется, поскольку он не будет возможность обработки запросов, прежде чем они будут обработаны с модуль ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c830f-222">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="c830f-223">Чтобы удалить обработчик файла статистики IIS (на уровне сервера или веб-сайт):</span><span class="sxs-lookup"><span data-stu-id="c830f-223">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="c830f-224">Перейдите к **модули** функции</span><span class="sxs-lookup"><span data-stu-id="c830f-224">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="c830f-225">Выберите **StaticFileModule** в списке</span><span class="sxs-lookup"><span data-stu-id="c830f-225">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="c830f-226">Коснитесь **удалить** в **действия** боковой панели</span><span class="sxs-lookup"><span data-stu-id="c830f-226">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="c830f-227">Если обработчик файла статистики IIS включена **и** модуля ASP.NET Core (ANCM) настроен неправильно (например если *web.config* не было развернуто), невозможно предоставить статические файлы.</span><span class="sxs-lookup"><span data-stu-id="c830f-227">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="c830f-228">Файлы кода (включая c# и Razor) должны располагаться за пределами проекта приложения `web root` (*wwwroot* по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="c830f-228">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="c830f-229">При этом создается четкое разделение стороны содержимое своего приложения клиента и сервера стороны исходный код, который предотвращает утечку код со стороны сервера.</span><span class="sxs-lookup"><span data-stu-id="c830f-229">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c830f-230">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c830f-230">Additional Resources</span></span>

* [<span data-ttu-id="c830f-231">По промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="c830f-231">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="c830f-232">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c830f-232">Introduction to ASP.NET Core</span></span>](../index.md)