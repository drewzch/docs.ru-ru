---
ms.openlocfilehash: 75945e7ff26c1c6db891d12cef4c16ed210da6af
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72393896"
---
### <a name="mvc-web-api-compatibility-shim-removed"></a><span data-ttu-id="632d4-101">MVC. Удалена оболочка совместимости веб-API</span><span class="sxs-lookup"><span data-stu-id="632d4-101">MVC: Web API compatibility shim removed</span></span>

<span data-ttu-id="632d4-102">Начиная с пакета SDK для .NET Core 3.0, пакет `Microsoft.AspNetCore.Mvc.WebApiCompatShim` недоступен.</span><span class="sxs-lookup"><span data-stu-id="632d4-102">Starting with ASP.NET Core 3.0, the `Microsoft.AspNetCore.Mvc.WebApiCompatShim` package is no longer available.</span></span>

#### <a name="change-description"></a><span data-ttu-id="632d4-103">Описание изменений</span><span class="sxs-lookup"><span data-stu-id="632d4-103">Change description</span></span>

<span data-ttu-id="632d4-104">Пакет `Microsoft.AspNetCore.Mvc.WebApiCompatShim` (WebApiCompatShim) обеспечивает в ASP.NET Core частичную совместимость с веб-API 2 для ASP.NET 4.x, чтобы упростить миграцию существующих реализаций веб-API на ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="632d4-104">The `Microsoft.AspNetCore.Mvc.WebApiCompatShim` (WebApiCompatShim) package provides partial compatibility in ASP.NET Core with ASP.NET 4.x Web API 2 to simplify migrating existing Web API implementations to ASP.NET Core.</span></span> <span data-ttu-id="632d4-105">Но при этом приложения, которые используют WebApiCompatShim, не могут воспользоваться функциями API из последних выпусков ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="632d4-105">However, apps using the WebApiCompatShim don't benefit from the API-related features shipping in recent ASP.NET Core releases.</span></span> <span data-ttu-id="632d4-106">Сюда относятся, среди прочего, улучшенное создание спецификаций Open API, стандартизованная обработка ошибок и создание клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="632d4-106">Such features include improved Open API specification generation, standardized error handling, and client code generation.</span></span> <span data-ttu-id="632d4-107">Чтобы уделить больше внимания развитию API в версии 3.0, WebApiCompatShim удален.</span><span class="sxs-lookup"><span data-stu-id="632d4-107">To better focus the API efforts in 3.0, WebApiCompatShim was removed.</span></span> <span data-ttu-id="632d4-108">Существующие приложения, которые используют WebApiCompatShim, необходимо перевести на новую модель `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="632d4-108">Existing apps using the WebApiCompatShim should migrate to the newer `[ApiController]` model.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="632d4-109">Представленная версия</span><span class="sxs-lookup"><span data-stu-id="632d4-109">Version introduced</span></span>

<span data-ttu-id="632d4-110">3.0</span><span class="sxs-lookup"><span data-stu-id="632d4-110">3.0</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="632d4-111">Причина изменения</span><span class="sxs-lookup"><span data-stu-id="632d4-111">Reason for change</span></span>

<span data-ttu-id="632d4-112">Оболочка совместимости веб-API являлась временным инструментом для миграции.</span><span class="sxs-lookup"><span data-stu-id="632d4-112">The Web API compatibility shim was a migration tool.</span></span> <span data-ttu-id="632d4-113">Она ограничивает доступ пользователей к новым функциям, добавляемым в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="632d4-113">It restricts user access to new functionality added in ASP.NET Core.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="632d4-114">Рекомендуемое действие</span><span class="sxs-lookup"><span data-stu-id="632d4-114">Recommended action</span></span>

<span data-ttu-id="632d4-115">Прекратите использование оболочки и переходите сразу к использованию аналогичных функций в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="632d4-115">Remove usage of this shim and migrate directly to the similar functionality in ASP.NET Core itself.</span></span>

#### <a name="category"></a><span data-ttu-id="632d4-116">Категория</span><span class="sxs-lookup"><span data-stu-id="632d4-116">Category</span></span>

<span data-ttu-id="632d4-117">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="632d4-117">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="632d4-118">Затронутые API</span><span class="sxs-lookup"><span data-stu-id="632d4-118">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Mvc.WebApiCompatShim?displayProperty=fullName>

<!--

#### Affected APIs

N:Microsoft.AspNetCore.Mvc.WebApiCompatShim

-->