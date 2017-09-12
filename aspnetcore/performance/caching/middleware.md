---
title: "Кэширование по промежуточного слоя в ASP.NET Core ответов"
author: guardrex
description: "Конфигурация и использование по промежуточного слоя, кэширование ответа в приложениях ASP.NET Core."
keywords: "ASP.NET Core кэширование ответов, кэширование, ResponseCache, ResponseCaching, Cache-Control, VaryByQueryKeys, по промежуточного слоя"
ms.author: riande
manager: wpickett
ms.date: 08/22/2017
ms.topic: article
ms.assetid: f9267eab-2762-42ac-1638-4a25d2c9d67c
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: 7790f38dda61eabd3cbbc6088ad455c07289b739
ms.sourcegitcommit: 70089de5bfd8ecd161261aa95faf07a4e1534cf8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2017
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="f8f61-104">Кэширование по промежуточного слоя в ASP.NET Core ответов</span><span class="sxs-lookup"><span data-stu-id="f8f61-104">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="f8f61-105">По [Latham Люк](https://github.com/GuardRex) и [Джон Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="f8f61-105">By [Luke Latham](https://github.com/GuardRex) and [John Luo](https://github.com/JunTaoLuo)</span></span>

[<span data-ttu-id="f8f61-106">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="f8f61-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples)

<span data-ttu-id="f8f61-107">Этот документ содержит сведения о способах настройки по промежуточного слоя ответа кэширование в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8f61-107">This document provides details on how to configure the Response Caching Middleware in ASP.NET Core apps.</span></span> <span data-ttu-id="f8f61-108">По промежуточного слоя определяет, когда ответов может быть кэширован, ответы на магазины и служит ответы из кэша.</span><span class="sxs-lookup"><span data-stu-id="f8f61-108">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="f8f61-109">Основные сведения о кэшировании HTTP и `ResponseCache` см. в разделе [кэширование ответов](response.md).</span><span class="sxs-lookup"><span data-stu-id="f8f61-109">For an introduction to HTTP caching and the `ResponseCache` attribute, see [Response Caching](response.md).</span></span>

## <a name="package"></a><span data-ttu-id="f8f61-110">Пакет</span><span class="sxs-lookup"><span data-stu-id="f8f61-110">Package</span></span>
<span data-ttu-id="f8f61-111">Чтобы включить по промежуточного слоя в проекте, добавьте ссылку на [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) пакета или использовать [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) пакета.</span><span class="sxs-lookup"><span data-stu-id="f8f61-111">To include the middleware in a project, add a reference to the [`Microsoft.AspNetCore.ResponseCaching`](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package or use the [`Microsoft.AspNetCore.All`](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) package.</span></span>

## <a name="configuration"></a><span data-ttu-id="f8f61-112">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="f8f61-112">Configuration</span></span>
<span data-ttu-id="f8f61-113">В `ConfigureServices`, добавление в коллекцию службы по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="f8f61-113">In `ConfigureServices`, add the middleware to the service collection.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8f61-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f8f61-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f8f61-115">[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="f8f61-115">[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8f61-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f8f61-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f8f61-117">[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f8f61-117">[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]</span></span>

---

<span data-ttu-id="f8f61-118">Настройка приложения для использования по промежуточного слоя с `UseResponseCaching` метод расширения, который добавляет по промежуточного слоя в конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="f8f61-118">Configure the app to use the middleware with the `UseResponseCaching` extension method, which adds the middleware to the request processing pipeline.</span></span> <span data-ttu-id="f8f61-119">Добавляет в пример приложения [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) заголовок в ответ, который кэширует кэшируемых ответов на срок до 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="f8f61-119">The sample app adds a [`Cache-Control`](https://tools.ietf.org/html/rfc7234#section-5.2) header to the response that caches cacheable responses for up to 10 seconds.</span></span> <span data-ttu-id="f8f61-120">Образец отправляет [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) заголовок для настройки по промежуточного слоя для обслуживания только если кэшированный ответ [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) заголовок последующих запросов таковыми исходного запроса.</span><span class="sxs-lookup"><span data-stu-id="f8f61-120">The sample sends a [`Vary`](https://tools.ietf.org/html/rfc7231#section-7.1.4) header to configure the middleware to serve a cached response only if the [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8f61-121">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f8f61-121">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f8f61-122">[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="f8f61-122">[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8f61-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f8f61-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f8f61-124">[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f8f61-124">[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]</span></span>

---

<span data-ttu-id="f8f61-125">По промежуточного слоя кэширования ответа только кэширует ответы сервера 200 (ОК).</span><span class="sxs-lookup"><span data-stu-id="f8f61-125">The Response Caching Middleware only caches 200 (OK) server responses.</span></span> <span data-ttu-id="f8f61-126">Все другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), учитываются по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="f8f61-126">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="f8f61-127">Ответы с содержимым для авторизованных клиентов должен быть помечен как не может быть кэширован, чтобы предотвратить по промежуточного слоя в хранении и обслуживании этих ответов.</span><span class="sxs-lookup"><span data-stu-id="f8f61-127">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="f8f61-128">В разделе [условия для кэширования](#conditions-for-caching) сведения о том, как по промежуточного слоя определяет, является ли ответ может быть кэширован.</span><span class="sxs-lookup"><span data-stu-id="f8f61-128">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="f8f61-129">Параметры</span><span class="sxs-lookup"><span data-stu-id="f8f61-129">Options</span></span>
<span data-ttu-id="f8f61-130">По промежуточного слоя предоставляет три варианта управлении кэшем ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-130">The middleware offers three options for controlling response caching.</span></span>

| <span data-ttu-id="f8f61-131">Параметр</span><span class="sxs-lookup"><span data-stu-id="f8f61-131">Option</span></span>                | <span data-ttu-id="f8f61-132">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f8f61-132">Default Value</span></span> |
| --------------------- | ------------- |
| <span data-ttu-id="f8f61-133">UseCaseSensitivePaths</span><span class="sxs-lookup"><span data-stu-id="f8f61-133">UseCaseSensitivePaths</span></span> | <span data-ttu-id="f8f61-134">Определяет, если ответы кэшируются на пути с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="f8f61-134">Determines if responses are cached on case-sensitive paths.</span></span></p><p><span data-ttu-id="f8f61-135">Значение по умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="f8f61-135">The default value is `false`.</span></span> |
| <span data-ttu-id="f8f61-136">MaximumBodySize</span><span class="sxs-lookup"><span data-stu-id="f8f61-136">MaximumBodySize</span></span>       | <span data-ttu-id="f8f61-137">Максимальный размер кэшируемого текст ответа в байтах.</span><span class="sxs-lookup"><span data-stu-id="f8f61-137">The largest cacheable size for the response body in bytes.</span></span></p><span data-ttu-id="f8f61-138">Значение по умолчанию — `64 * 1024 * 1024` (64 МБ).</span><span class="sxs-lookup"><span data-stu-id="f8f61-138">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <span data-ttu-id="f8f61-139">Значение SizeLimit</span><span class="sxs-lookup"><span data-stu-id="f8f61-139">SizeLimit</span></span>             | <span data-ttu-id="f8f61-140">Предельный размер для по промежуточного слоя кэша ответа в байтах.</span><span class="sxs-lookup"><span data-stu-id="f8f61-140">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="f8f61-141">Значение по умолчанию — `100 * 1024 * 1024` (100 МБ).</span><span class="sxs-lookup"><span data-stu-id="f8f61-141">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |

<span data-ttu-id="f8f61-142">В следующем примере настраивается по промежуточного слоя для кэширования ответов меньше или равна 1 024 байт, с использованием путей, с учетом регистра, хранение ответы `/page1` и `/Page1` отдельно.</span><span class="sxs-lookup"><span data-stu-id="f8f61-142">The following example configures the middleware to cache responses smaller than or equal to 1,024 bytes using case-sensitive paths, storing the responses to `/page1` and `/Page1` separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="f8f61-143">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="f8f61-143">VaryByQueryKeys</span></span>
<span data-ttu-id="f8f61-144">При использовании MVC, `ResponseCache` атрибут задает параметры, необходимые для установки соответствующие заголовки для кэширования ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-144">When using MVC, the `ResponseCache` attribute specifies the parameters necessary for setting appropriate headers for response caching.</span></span> <span data-ttu-id="f8f61-145">Единственным параметром `ResponseCache` атрибут, строго требует по промежуточного слоя — `VaryByQueryKeys`, которой не соответствует фактическое заголовка HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8f61-145">The only parameter of the `ResponseCache` attribute that strictly requires the middleware is `VaryByQueryKeys`, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="f8f61-146">Дополнительные сведения см. в разделе [ResponseCache атрибут](response.md#responsecache-attribute).</span><span class="sxs-lookup"><span data-stu-id="f8f61-146">For more information, see [ResponseCache Attribute](response.md#responsecache-attribute).</span></span>

<span data-ttu-id="f8f61-147">Если не используется MVC, можно варьировать кэширование ответов с `VaryByQueryKeys` компонентов.</span><span class="sxs-lookup"><span data-stu-id="f8f61-147">When not using MVC, you can vary response caching with the `VaryByQueryKeys` feature.</span></span> <span data-ttu-id="f8f61-148">Используйте `ResponseCachingFeature` непосредственно из `IFeatureCollection` из `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="f8f61-148">Use the `ResponseCachingFeature` directly from the `IFeatureCollection` of the `HttpContext`:</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="f8f61-149">Заголовки HTTP, используемые по промежуточного слоя кэширования ответа</span><span class="sxs-lookup"><span data-stu-id="f8f61-149">HTTP headers used by Response Caching Middleware</span></span>
<span data-ttu-id="f8f61-150">Кэширование по промежуточного слоя ответов настраивается с помощью заголовков HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8f61-150">Response caching by the middleware is configured via HTTP headers.</span></span> <span data-ttu-id="f8f61-151">Ниже перечислены соответствующие заголовки с заметками на их влияние на кэширование.</span><span class="sxs-lookup"><span data-stu-id="f8f61-151">The relevant headers are listed below with notes on how they affect caching.</span></span>

| <span data-ttu-id="f8f61-152">Header</span><span class="sxs-lookup"><span data-stu-id="f8f61-152">Header</span></span> | <span data-ttu-id="f8f61-153">Подробные сведения</span><span class="sxs-lookup"><span data-stu-id="f8f61-153">Details</span></span> |
| ------ | ------- |
| <span data-ttu-id="f8f61-154">Авторизация</span><span class="sxs-lookup"><span data-stu-id="f8f61-154">Authorization</span></span> | <span data-ttu-id="f8f61-155">Ответ не кэшируется. Если заголовок существует.</span><span class="sxs-lookup"><span data-stu-id="f8f61-155">The response isn't cached if the header exists.</span></span> |
| <span data-ttu-id="f8f61-156">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="f8f61-156">Cache-Control</span></span> | <span data-ttu-id="f8f61-157">По промежуточного слоя рассматривает только кэширование ответов, отмеченные `public` директива кэша.</span><span class="sxs-lookup"><span data-stu-id="f8f61-157">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="f8f61-158">Можно управлять кэшированием со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="f8f61-158">You can control caching with the following parameters:</span></span><ul><li><span data-ttu-id="f8f61-159">max-age</span><span class="sxs-lookup"><span data-stu-id="f8f61-159">max-age</span></span></li><li><span data-ttu-id="f8f61-160">Устаревшая max &#8224;</span><span class="sxs-lookup"><span data-stu-id="f8f61-160">max-stale&#8224;</span></span></li><li><span data-ttu-id="f8f61-161">Min нуля</span><span class="sxs-lookup"><span data-stu-id="f8f61-161">min-fresh</span></span></li><li><span data-ttu-id="f8f61-162">должен revalidate</span><span class="sxs-lookup"><span data-stu-id="f8f61-162">must-revalidate</span></span></li><li><span data-ttu-id="f8f61-163">Нет-cache</span><span class="sxs-lookup"><span data-stu-id="f8f61-163">no-cache</span></span></li><li><span data-ttu-id="f8f61-164">нет хранилища</span><span class="sxs-lookup"><span data-stu-id="f8f61-164">no-store</span></span></li><li><span data-ttu-id="f8f61-165">только если кэширования</span><span class="sxs-lookup"><span data-stu-id="f8f61-165">only-if-cached</span></span></li><li><span data-ttu-id="f8f61-166">private</span><span class="sxs-lookup"><span data-stu-id="f8f61-166">private</span></span></li><li><span data-ttu-id="f8f61-167">public</span><span class="sxs-lookup"><span data-stu-id="f8f61-167">public</span></span></li><li><span data-ttu-id="f8f61-168">s-maxage</span><span class="sxs-lookup"><span data-stu-id="f8f61-168">s-maxage</span></span></li><li><span data-ttu-id="f8f61-169">Proxy-revalidate &#8225;</span><span class="sxs-lookup"><span data-stu-id="f8f61-169">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="f8f61-170">&#8224; Если указано без ограничений `max-stale`, по промежуточного слоя не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="f8f61-170">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="f8f61-171">&#8225; `proxy-revalidate` действует так же, как `must-revalidate`.</span><span class="sxs-lookup"><span data-stu-id="f8f61-171">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="f8f61-172">Дополнительные сведения см. в разделе [RFC 7231: запрос директивы управления кэшем](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span><span class="sxs-lookup"><span data-stu-id="f8f61-172">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| <span data-ttu-id="f8f61-173">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="f8f61-173">Pragma</span></span> | <span data-ttu-id="f8f61-174">Объект `Pragma: no-cache` заголовка в запросе дает тот же эффект, что `Cache-Control: no-cache`.</span><span class="sxs-lookup"><span data-stu-id="f8f61-174">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="f8f61-175">Этот заголовок переопределяется соответствующей директивы в `Cache-Control` заголовка, если он имеется.</span><span class="sxs-lookup"><span data-stu-id="f8f61-175">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="f8f61-176">Для обеспечения обратной совместимости с HTTP/1.0 во внимание.</span><span class="sxs-lookup"><span data-stu-id="f8f61-176">Considered for backward compatibility with HTTP/1.0.</span></span> |
| <span data-ttu-id="f8f61-177">Set-Cookie</span><span class="sxs-lookup"><span data-stu-id="f8f61-177">Set-Cookie</span></span> | <span data-ttu-id="f8f61-178">Ответ не кэшируется. Если заголовок существует.</span><span class="sxs-lookup"><span data-stu-id="f8f61-178">The response isn't cached if the header exists.</span></span> |
| <span data-ttu-id="f8f61-179">Различаются</span><span class="sxs-lookup"><span data-stu-id="f8f61-179">Vary</span></span> | <span data-ttu-id="f8f61-180">`Vary` Заголовок используется другой заголовок для изменения кэшированного ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-180">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="f8f61-181">Например, можно кэшировать ответы при кодировании, включая `Vary: Accept-Encoding` заголовок, который кэширует ответы для запросов с заголовками `Accept-Encoding: gzip` и `Accept-Encoding: text/plain` отдельно.</span><span class="sxs-lookup"><span data-stu-id="f8f61-181">For example, you can cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="f8f61-182">Значение заголовка ответа `*` никогда не хранится.</span><span class="sxs-lookup"><span data-stu-id="f8f61-182">A response with a header value of `*` is never stored.</span></span> |
| <span data-ttu-id="f8f61-183">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="f8f61-183">Expires</span></span> | <span data-ttu-id="f8f61-184">Как устаревшие средством этот заголовок ответа не хранятся или получить, если не переопределено другим `Cache-Control` заголовки.</span><span class="sxs-lookup"><span data-stu-id="f8f61-184">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| <span data-ttu-id="f8f61-185">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="f8f61-185">If-None-Match</span></span> | <span data-ttu-id="f8f61-186">Полный ответ подано из кэша, если значение не `*` и `ETag` ответа не соответствует ни одному из предлагаемых значений.</span><span class="sxs-lookup"><span data-stu-id="f8f61-186">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="f8f61-187">В противном случае выдается в отклике 304 (не изменено).</span><span class="sxs-lookup"><span data-stu-id="f8f61-187">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| <span data-ttu-id="f8f61-188">If-Modified-Since</span><span class="sxs-lookup"><span data-stu-id="f8f61-188">If-Modified-Since</span></span> | <span data-ttu-id="f8f61-189">Если `If-None-Match` заголовок отсутствует, полный ответ подано из кэша, если дата кэшированного ответа новее, чем указанное значение.</span><span class="sxs-lookup"><span data-stu-id="f8f61-189">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="f8f61-190">В противном случае выдается в отклике 304 (не изменено).</span><span class="sxs-lookup"><span data-stu-id="f8f61-190">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| <span data-ttu-id="f8f61-191">Дата</span><span class="sxs-lookup"><span data-stu-id="f8f61-191">Date</span></span> | <span data-ttu-id="f8f61-192">При выполнении из кэша, `Date` задать заголовок по промежуточного слоя, если он не указан исходный ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-192">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| <span data-ttu-id="f8f61-193">Content-Length</span><span class="sxs-lookup"><span data-stu-id="f8f61-193">Content-Length</span></span> | <span data-ttu-id="f8f61-194">При выполнении из кэша, `Content-Length` задать заголовок по промежуточного слоя, если он не указан исходный ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-194">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| <span data-ttu-id="f8f61-195">Срок действия</span><span class="sxs-lookup"><span data-stu-id="f8f61-195">Age</span></span> | <span data-ttu-id="f8f61-196">`Age` Заголовок, отправляемый в ответ исходного игнорируется.</span><span class="sxs-lookup"><span data-stu-id="f8f61-196">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="f8f61-197">По промежуточного слоя вычисляет новое значение, при выполнении кэшированного ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-197">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="troubleshooting"></a><span data-ttu-id="f8f61-198">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="f8f61-198">Troubleshooting</span></span>
<span data-ttu-id="f8f61-199">Если режим кэширования не должным образом, проверка ответов может быть кэширован и может быть предоставлен из кэша с помощью проверки запроса заголовки входящих и исходящих заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-199">If caching behavior isn't as you expect, confirm that responses are cacheable and capable of being served from the cache by examining the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="f8f61-200">Включение [входа](xref:fundamentals/logging) может помочь при отладке.</span><span class="sxs-lookup"><span data-stu-id="f8f61-200">Enabling [logging](xref:fundamentals/logging) can help when debugging.</span></span> <span data-ttu-id="f8f61-201">Журналы по промежуточного слоя, кэширование поведение и при получении ответа из кэша.</span><span class="sxs-lookup"><span data-stu-id="f8f61-201">The middleware logs caching behavior and when a response is retrieved from cache.</span></span>

<span data-ttu-id="f8f61-202">При тестирование и устранение неполадок поведение кэширования, браузер может задать заголовки запроса, влияющих на кэширование нежелательно способами.</span><span class="sxs-lookup"><span data-stu-id="f8f61-202">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="f8f61-203">Например, браузер может задать `Cache-Control` заголовка `no-cache` при обновлении страницы.</span><span class="sxs-lookup"><span data-stu-id="f8f61-203">For example, a browser may set the `Cache-Control` header to `no-cache` when you refresh the page.</span></span> <span data-ttu-id="f8f61-204">Следующие средства можно явно задать заголовки запроса и являются предпочтительными для тестирования кэширования:</span><span class="sxs-lookup"><span data-stu-id="f8f61-204">The following tools can explicitly set request headers, and are preferred for testing caching:</span></span>

* [<span data-ttu-id="f8f61-205">Fiddler</span><span class="sxs-lookup"><span data-stu-id="f8f61-205">Fiddler</span></span>](http://www.telerik.com/fiddler)
* [<span data-ttu-id="f8f61-206">Firebug</span><span class="sxs-lookup"><span data-stu-id="f8f61-206">Firebug</span></span>](http://getfirebug.com/)
* [<span data-ttu-id="f8f61-207">Почтальон</span><span class="sxs-lookup"><span data-stu-id="f8f61-207">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="f8f61-208">Условия для кэширования</span><span class="sxs-lookup"><span data-stu-id="f8f61-208">Conditions for caching</span></span>
* <span data-ttu-id="f8f61-209">Запрос должен привести ответ 200 (ОК) с сервера.</span><span class="sxs-lookup"><span data-stu-id="f8f61-209">The request must result in a 200 (OK) response from the server.</span></span>
* <span data-ttu-id="f8f61-210">Метод запроса должно быть GET или HEAD.</span><span class="sxs-lookup"><span data-stu-id="f8f61-210">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="f8f61-211">Терминалов по промежуточного слоя, например по промежуточного слоя статических файлов не должен обработать ответ перед по промежуточного слоя кэширования ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-211">Terminal middleware, such as Static File Middleware, must not process the response prior to the Response Caching Middleware.</span></span>
* <span data-ttu-id="f8f61-212">`Authorization` Заголовок не должен присутствовать.</span><span class="sxs-lookup"><span data-stu-id="f8f61-212">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="f8f61-213">`Cache-Control`Параметры заголовка должен быть допустимым, а ответ должен быть помечен `public` и не помечен `private`.</span><span class="sxs-lookup"><span data-stu-id="f8f61-213">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="f8f61-214">`Pragma: no-cache` Заголовка значение не должно быть Если `Cache-Control` заголовок отсутствует, как `Cache-Control` переопределяет заголовок `Pragma` заголовка, если они есть.</span><span class="sxs-lookup"><span data-stu-id="f8f61-214">The `Pragma: no-cache` header/value must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="f8f61-215">`Set-Cookie` Заголовок не должен присутствовать.</span><span class="sxs-lookup"><span data-stu-id="f8f61-215">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="f8f61-216">`Vary`Параметры заголовка должно быть допустимым и не равно `*`.</span><span class="sxs-lookup"><span data-stu-id="f8f61-216">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="f8f61-217">`Content-Length` Значение заголовка (если задать) должно соответствовать размеру текста ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-217">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="f8f61-218">`HttpSendFileFeature` Не используется.</span><span class="sxs-lookup"><span data-stu-id="f8f61-218">The `HttpSendFileFeature` isn't used.</span></span>
* <span data-ttu-id="f8f61-219">Ответ не должно быть устаревшей в соответствии с `Expires` заголовок и `max-age` и `s-maxage` кэшировать директивы.</span><span class="sxs-lookup"><span data-stu-id="f8f61-219">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="f8f61-220">Буферизация прошла успешно, и размер ответа меньше, чем настроенное или по умолчанию `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="f8f61-220">Response buffering is successful, and the size of the response is smaller than the configured or default `SizeLimit`.</span></span>
* <span data-ttu-id="f8f61-221">Требуется ответ может быть кэширован в соответствии с [RFC 7234](https://tools.ietf.org/html/rfc7234) спецификации.</span><span class="sxs-lookup"><span data-stu-id="f8f61-221">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="f8f61-222">Например `no-store` директива не должен существовать в поля заголовка запроса или ответа.</span><span class="sxs-lookup"><span data-stu-id="f8f61-222">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="f8f61-223">В разделе *раздел 3: хранение ответы в кэше* из [RFC 7234](https://tools.ietf.org/html/rfc7234) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="f8f61-223">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="f8f61-224">Сложные системы, для создания токенов безопасности для предотвращения подделки межсайтовых запросов (CSRF) атак наборы `Cache-Control` и `Pragma` заголовки `no-cache` , чтобы ответы не кэшируются.</span><span class="sxs-lookup"><span data-stu-id="f8f61-224">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8f61-225">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f8f61-225">Additional resources</span></span>

* [<span data-ttu-id="f8f61-226">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="f8f61-226">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="f8f61-227">По промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="f8f61-227">Middleware</span></span>](xref:fundamentals/middleware)