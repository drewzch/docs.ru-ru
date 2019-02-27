---
title: Создание и использование асинхронных потоков
description: Это расширенное руководство иллюстрирует сценарии, в которых создание и использование асинхронных потоков обеспечивает более естественный способ работы с последовательностями данных, которые могут создаваться асинхронно.
ms.date: 02/10/2019
ms.custom: mvc
ms.openlocfilehash: c8be9cf4b83e3dd72232279e7c15dcba639c2058
ms.sourcegitcommit: bef803e2025642df39f2f1e046767d89031e0304
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2019
ms.locfileid: "56306009"
---
# <a name="tutorial-generate-and-consume-async-streams-using-c-80-and-net-core-30"></a><span data-ttu-id="476c1-103">Учебник. Создание и использование асинхронных потоков с использованием C# 8.0 и .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="476c1-103">Tutorial: Generate and consume async streams using C# 8.0 and .NET Core 3.0</span></span>

<span data-ttu-id="476c1-104">C# 8.0 представляет **асинхронные потоки**, которые моделируют источник потоковых данных, когда можно получить элементы в потоке данных или создать их асинхронно.</span><span class="sxs-lookup"><span data-stu-id="476c1-104">C# 8.0 introduces **async streams**, which model a streaming source of data when the elements in the data stream may be retrieved or generated asynchronously.</span></span> <span data-ttu-id="476c1-105">Асинхронные потоки опираются на новые интерфейсы, представленные в .NET Standard 2.1 и реализованные в .NET Core 3.0, чтобы обеспечить естественную модель программирования для асинхронных источников потоковых данных.</span><span class="sxs-lookup"><span data-stu-id="476c1-105">Async streams rely on new interfaces introduced in .NET Standard 2.1 and implemented in .NET Core 3.0 to provide a natural programming model for asynchronous streaming data sources.</span></span>

<span data-ttu-id="476c1-106">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="476c1-106">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="476c1-107">Создать источник данных, который формирует последовательность элементов данных асинхронно.</span><span class="sxs-lookup"><span data-stu-id="476c1-107">Create a data source that generates a sequence of data elements asynchronously.</span></span>
> * <span data-ttu-id="476c1-108">Использовать этот источник данных асинхронно.</span><span class="sxs-lookup"><span data-stu-id="476c1-108">Consume that data source asynchronously.</span></span>
> * <span data-ttu-id="476c1-109">Распознавать, когда новый интерфейс и источник данных предпочтительнее для более ранних синхронных последовательностей данных.</span><span class="sxs-lookup"><span data-stu-id="476c1-109">Recognize when the new interface and data source are preferred to earlier synchronous data sequences.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="476c1-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="476c1-110">Prerequisites</span></span>

<span data-ttu-id="476c1-111">Вам нужно настроить на компьютере выполнение .NET Core, включая бета-компилятор C# 8.0.</span><span class="sxs-lookup"><span data-stu-id="476c1-111">You’ll need to set up your machine to run .NET Core, including the C# 8.0 beta compiler.</span></span> <span data-ttu-id="476c1-112">Бета-компилятор C# 8 доступен, начиная с [предварительной версии 1 Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/), или [в пакете SDK предварительной версии 1 .NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="476c1-112">The C# 8 beta compiler is available starting with [Visual Studio 2019 preview 1](https://visualstudio.microsoft.com/vs/preview/), or [.NET Core 3.0 preview 1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span> <span data-ttu-id="476c1-113">Асинхронные потоки впервые доступны в предварительной версии 1 .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="476c1-113">Async streams are first available in .NET Core 3.0 preview 1.</span></span>

<span data-ttu-id="476c1-114">Чтобы вы могли получить доступ к конечной точке GraphQL GitHub, необходимо создать [маркер доступа GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token).</span><span class="sxs-lookup"><span data-stu-id="476c1-114">You'll need to create a [GitHub access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/#creating-a-token) so that you can access the GitHub GraphQL endpoint.</span></span> <span data-ttu-id="476c1-115">Выберите следующие разрешения для маркеров доступа GitHub.</span><span class="sxs-lookup"><span data-stu-id="476c1-115">Select the following permissions for your GitHub Access Token:</span></span>

- <span data-ttu-id="476c1-116">repo:status</span><span class="sxs-lookup"><span data-stu-id="476c1-116">repo:status</span></span>
- <span data-ttu-id="476c1-117">public_repo</span><span class="sxs-lookup"><span data-stu-id="476c1-117">public_repo</span></span>

<span data-ttu-id="476c1-118">Храните маркер доступа в надежном месте, чтобы вы могли использовать его для получения доступа к конечной точке API GitHub.</span><span class="sxs-lookup"><span data-stu-id="476c1-118">Save the access token in a safe place so you can use it to gain access to the GitHub API endpoint.</span></span>

> [!WARNING]
> <span data-ttu-id="476c1-119">Храните свой личный маркер доступа в безопасном месте.</span><span class="sxs-lookup"><span data-stu-id="476c1-119">Keep your personal access token secure.</span></span> <span data-ttu-id="476c1-120">Любое программное обеспечение с вашим личным маркером доступа может выполнять вызовы API GitHub с помощью ваших прав доступа.</span><span class="sxs-lookup"><span data-stu-id="476c1-120">Any software with your personal access token could make GitHub API calls using your access rights.</span></span>

<span data-ttu-id="476c1-121">В этом руководстве предполагается, что вы знакомы с C# и .NET, включая Visual Studio или .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="476c1-121">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="run-the-starter-application"></a><span data-ttu-id="476c1-122">Запуск начального приложения</span><span class="sxs-lookup"><span data-stu-id="476c1-122">Run the starter application</span></span>

<span data-ttu-id="476c1-123">Вы можете получить код для начального приложения, используемый в этом руководстве в репозитории [dotnet/samples](https://github.com/dotnet/samples) в папке [csharp/tutorials/AsyncStreams](https://github.com/dotnet/samples/tree/master/csharp/tutorials/AsyncStreams/start).</span><span class="sxs-lookup"><span data-stu-id="476c1-123">You can get the code for the starter application used in this tutorial from our [dotnet/samples](https://github.com/dotnet/samples) repository in the [csharp/tutorials/AsyncStreams](https://github.com/dotnet/samples/tree/master/csharp/tutorials/AsyncStreams/start) folder.</span></span>

<span data-ttu-id="476c1-124">Начальное приложение представляет собой консольное приложение, которое использует интерфейс [GraphQL GitHub](https://developer.github.com/v4/) для получения последних проблем, написанных в репозитории [dotnet/docs](https://github.com/dotnet/docs).</span><span class="sxs-lookup"><span data-stu-id="476c1-124">The starter application is a console application that uses the [GitHub GraphQL](https://developer.github.com/v4/) interface to retrieve recent issues written in the [dotnet/docs](https://github.com/dotnet/docs) repository.</span></span> <span data-ttu-id="476c1-125">Начнем с просмотра следующего кода для метода `Main` начального приложения.</span><span class="sxs-lookup"><span data-stu-id="476c1-125">Start by looking at the following code for the starter app `Main` method:</span></span>

[!code-csharp[StarterAppMain](~/samples/csharp/tutorials/AsyncStreams/start/IssuePRreport/IssuePRreport/Program.cs#StarterAppMain)]

<span data-ttu-id="476c1-126">Вы можете задать переменную среды `GitHubKey` личному маркеру доступа или заменить последний аргумент в вызове на `GenEnvVariable` с помощью личного маркера доступа.</span><span class="sxs-lookup"><span data-stu-id="476c1-126">You can either set a `GitHubKey` environment variable to your personal access token, or you can replace the last argument in the call to `GenEnvVariable` with your personal access token.</span></span> <span data-ttu-id="476c1-127">Не помещайте свой код доступа в исходный код, если вы будете сохранять исходный код вместе с другими или помещать его в общий репозиторий с исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="476c1-127">Don't put your access code in source code if you'll be saving the source with others, or putting it in a shared source repository.</span></span>

<span data-ttu-id="476c1-128">После создания клиента GitHub код в `Main` создает объект отчета о ходе выполнения и маркер отмены.</span><span class="sxs-lookup"><span data-stu-id="476c1-128">After creating the GitHub client, the code in `Main` creates a progress reporting object and a cancellation token.</span></span> <span data-ttu-id="476c1-129">После создания этих объектов `Main` вызывает `runPagedQueryAsync`, чтобы получить более 250 недавно созданных проблем.</span><span class="sxs-lookup"><span data-stu-id="476c1-129">Once those objects are created, `Main` calls `runPagedQueryAsync` to retrieve the most recent 250 created issues.</span></span> <span data-ttu-id="476c1-130">Результаты отобразятся после выполнения этой задачи.</span><span class="sxs-lookup"><span data-stu-id="476c1-130">After that task has finished, the results are displayed.</span></span>

<span data-ttu-id="476c1-131">При запуске начального приложения вы можете обнаружить некоторые важные замечания о том, как будет выполняться приложение.</span><span class="sxs-lookup"><span data-stu-id="476c1-131">When you run the starter application, you can make some important observations about how this application runs.</span></span>  <span data-ttu-id="476c1-132">Вы увидите ход выполнения, передаваемый каждой странице, возвращенной с GitHub.</span><span class="sxs-lookup"><span data-stu-id="476c1-132">You'll see progress reported for each page returned from GitHub.</span></span> <span data-ttu-id="476c1-133">Прежде чем GitHub вернет каждую новую страницу проблем, возникает заметная пауза.</span><span class="sxs-lookup"><span data-stu-id="476c1-133">You can observe a noticeable pause before GitHub returns each new page of issues.</span></span> <span data-ttu-id="476c1-134">Наконец, проблемы отображаются только после того, как получены все 10 страниц с GitHub.</span><span class="sxs-lookup"><span data-stu-id="476c1-134">Finally, the issues are displayed only after all 10 pages have been retrieved from GitHub.</span></span>

## <a name="examine-the-implementation"></a><span data-ttu-id="476c1-135">Изучение реализации</span><span class="sxs-lookup"><span data-stu-id="476c1-135">Examine the implementation</span></span>

<span data-ttu-id="476c1-136">Реализация показывает, почему возникло поведение, обсуждавшееся в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="476c1-136">The implementation reveals why you observed the behavior discussed in the previous section.</span></span> <span data-ttu-id="476c1-137">Изучите код для `runPagedQueryAsync`.</span><span class="sxs-lookup"><span data-stu-id="476c1-137">Examine the code for `runPagedQueryAsync`:</span></span>

[!code-csharp[RunPagedQueryStarter](~/samples/csharp/tutorials/AsyncStreams/start/IssuePRreport/IssuePRreport/Program.cs#RunPagedQuery)]

<span data-ttu-id="476c1-138">Давайте сконцентрируемся на алгоритме разбивки по страницам и асинхронной структуре предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="476c1-138">Let's concentrate on the paging algorithm and async structure of the preceding code.</span></span> <span data-ttu-id="476c1-139">(Дополнительные сведения об API GraphQL GitHub см. в [этой документации](https://developer.github.com/v4/guides/).) Метод `runPagedQueryAsync` перечисляет проблемы от самых последних до самых старых.</span><span class="sxs-lookup"><span data-stu-id="476c1-139">(You can consult the [GitHub GraphQL documentation](https://developer.github.com/v4/guides/) for details on the GitHub GraphQL API.) The `runPagedQueryAsync` method enumerates the issues from most recent to oldest.</span></span> <span data-ttu-id="476c1-140">Чтобы продолжить с предыдущей страницы, он запрашивает по 25 выпусков на страницу и проверяет структуру ответа `pageInfo`.</span><span class="sxs-lookup"><span data-stu-id="476c1-140">It requests 25 issues per page and examines the `pageInfo` structure of the response to continue with the previous page.</span></span> <span data-ttu-id="476c1-141">Это следует за стандартной поддержкой страниц GraphQL для многостраничных ответов.</span><span class="sxs-lookup"><span data-stu-id="476c1-141">That follows GraphQL's standard paging support for multi-page responses.</span></span> <span data-ttu-id="476c1-142">Ответ включает в себя объект `pageInfo`, который содержит значение `hasPreviousPages` и `startCursor`, используемые для запроса предыдущей страницы.</span><span class="sxs-lookup"><span data-stu-id="476c1-142">The response includes a `pageInfo` object that includes a `hasPreviousPages` value and a `startCursor` value used to request the previous page.</span></span> <span data-ttu-id="476c1-143">Проблемы в массиве `nodes`.</span><span class="sxs-lookup"><span data-stu-id="476c1-143">The issues are in the `nodes` array.</span></span> <span data-ttu-id="476c1-144">Метод `runPagedQueryAsync` добавляет эти узлы в массив, который содержит результаты со всех страниц.</span><span class="sxs-lookup"><span data-stu-id="476c1-144">The `runPagedQueryAsync` method appends these nodes to an array that contains all the results from all pages.</span></span>

<span data-ttu-id="476c1-145">После получения и восстановления страницы результатов `runPagedQueryAsync` сообщает о ходе выполнения и проверяет наличие отмены.</span><span class="sxs-lookup"><span data-stu-id="476c1-145">After retrieving and restoring a page of results, `runPagedQueryAsync` reports progress and checks for cancellation.</span></span> <span data-ttu-id="476c1-146">Если есть запрос на отмену, `runPagedQueryAsync` выдает <xref:System.OperationCanceledException>.</span><span class="sxs-lookup"><span data-stu-id="476c1-146">If cancellation has been requested, `runPagedQueryAsync` throws an <xref:System.OperationCanceledException>.</span></span>

<span data-ttu-id="476c1-147">Существует несколько элементов в этом коде, которые можно улучшить.</span><span class="sxs-lookup"><span data-stu-id="476c1-147">There are several elements in this code that can be improved.</span></span> <span data-ttu-id="476c1-148">Самое главное, `runPagedQueryAsync` должен выделить хранилище для всех возвращенных проблем.</span><span class="sxs-lookup"><span data-stu-id="476c1-148">Most importantly, `runPagedQueryAsync` must allocate storage for all the issues returned.</span></span> <span data-ttu-id="476c1-149">Этот пример останавливается после нахождения 250 проблем, так как для извлечения всех открытых проблем потребуется гораздо больше памяти на их хранение.</span><span class="sxs-lookup"><span data-stu-id="476c1-149">This sample stops at 250 issues because retrieving all open issues would require much more memory to store all the retrieved issues.</span></span> <span data-ttu-id="476c1-150">Кроме того, протоколы для поддержки хода выполнения и отмены делают алгоритм более сложным для понимания при первом чтении.</span><span class="sxs-lookup"><span data-stu-id="476c1-150">In addition, the protocols for supporting progress and supporting cancellation make the algorithm harder to understand on its first reading.</span></span> <span data-ttu-id="476c1-151">Необходимо определить класс, где предоставляется ход выполнения.</span><span class="sxs-lookup"><span data-stu-id="476c1-151">You must look for the progress class to find where progress is reported.</span></span> <span data-ttu-id="476c1-152">Вы также должны отслеживать передачу данных с помощью <xref:System.Threading.CancellationTokenSource> и связанный с ним <xref:System.Threading.CancellationToken>, чтобы понять, где запрашивается отмена и где она предоставляется.</span><span class="sxs-lookup"><span data-stu-id="476c1-152">You also have to trace the communications through the <xref:System.Threading.CancellationTokenSource> and its associated <xref:System.Threading.CancellationToken> to understand where cancellation is requested and where it's granted.</span></span>

## <a name="async-streams-provide-a-better-way"></a><span data-ttu-id="476c1-153">Предоставление лучшего способа асинхронных потоков</span><span class="sxs-lookup"><span data-stu-id="476c1-153">Async streams provide a better way</span></span>

<span data-ttu-id="476c1-154">Асинхронные потоки и связанная языковая поддержка обращаются ко всем этим вопросам.</span><span class="sxs-lookup"><span data-stu-id="476c1-154">Async streams and the associated language support address all those concerns.</span></span> <span data-ttu-id="476c1-155">Код, который формирует последовательность, теперь может использовать `yield return` для возврата элементов в методе, который был объявлен с помощью модификатора `async`.</span><span class="sxs-lookup"><span data-stu-id="476c1-155">The code that generates the sequence can now use `yield return` to return elements in a method that was declared with the `async` modifier.</span></span> <span data-ttu-id="476c1-156">Вы можете применить асинхронный поток, используя цикл `await foreach`, аналогично любой последовательности с помощью цикла `foreach`.</span><span class="sxs-lookup"><span data-stu-id="476c1-156">You can consume an async stream using an `await foreach` loop just as you consume any sequence using a `foreach` loop.</span></span>

<span data-ttu-id="476c1-157">Эти новые языковые функции зависят от трех новых интерфейсов, добавленных в .NET Standard 2.1 и реализованных в .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="476c1-157">These new language features depend on three new interfaces added to .NET Standard 2.1 and implemented in .NET Core 3.0:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        T Current { get; }

        ValueTask<bool> MoveNextAsync();
    }
}

namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```

<span data-ttu-id="476c1-158">Большинство разработчиков C# должны знать об этих трех интерфейсах.</span><span class="sxs-lookup"><span data-stu-id="476c1-158">These three interfaces should be familiar to most C# developers.</span></span> <span data-ttu-id="476c1-159">Они ведут себя подобно своим синхронным аналогам.</span><span class="sxs-lookup"><span data-stu-id="476c1-159">They behave in a manner similar to their synchronous counterparts:</span></span>

- <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IEnumerator%601?displayProperty=nameWithType>
- <xref:System.IDisposable?displayProperty=nameWithType>

<span data-ttu-id="476c1-160">Один тип, который может быть незнаком, — <xref:System.Threading.Tasks.ValueTask?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="476c1-160">One type that may be unfamiliar is <xref:System.Threading.Tasks.ValueTask?displayProperty=nameWithType>.</span></span> <span data-ttu-id="476c1-161">Структура `ValueTask` предоставляет API, аналогичный классу <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="476c1-161">The `ValueTask` struct provides a similar API to the <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> class.</span></span> <span data-ttu-id="476c1-162">`ValueTask` используется в этих интерфейсах по причинам производительности.</span><span class="sxs-lookup"><span data-stu-id="476c1-162">`ValueTask` is used in these interfaces for performance reasons.</span></span>

## <a name="convert-to-async-streams"></a><span data-ttu-id="476c1-163">Преобразование в асинхронные потоки</span><span class="sxs-lookup"><span data-stu-id="476c1-163">Convert to async streams</span></span>

<span data-ttu-id="476c1-164">Затем для создания асинхронного потока преобразуйте метод `runPagedQueryAsync`.</span><span class="sxs-lookup"><span data-stu-id="476c1-164">Next, convert the `runPagedQueryAsync` method to generate an async stream.</span></span> <span data-ttu-id="476c1-165">Сначала измените подпись `runPagedQueryAsync`, чтобы вернуть `IAsyncEnumerable<JToken>`, затем удалите маркер отмены и объекты хода выполнения из списка параметров, как показано в следующем коде.</span><span class="sxs-lookup"><span data-stu-id="476c1-165">First, change the signature of `runPagedQueryAsync` to return an `IAsyncEnumerable<JToken>`, and remove the cancellation token and progress objects from the parameter list as shown in the following code:</span></span>

[!code-csharp[FinishedSignature](~/samples/csharp/tutorials/AsyncStreams/finished/IssuePRreport/IssuePRreport/Program.cs#UpdateSignature)]

<span data-ttu-id="476c1-166">В следующем коде показано, как начальный код обрабатывает каждую страницу для извлечения.</span><span class="sxs-lookup"><span data-stu-id="476c1-166">The starter code processes each page as the page is retrieved, as shown in the following code:</span></span>

[!code-csharp[StarterPaging](~/samples/csharp/tutorials/AsyncStreams/start/IssuePRreport/IssuePRreport/Program.cs#ProcessPage)]

<span data-ttu-id="476c1-167">Замените эти три строки следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="476c1-167">Replace those three lines with the following code:</span></span>

[!code-csharp[FinishedPaging](~/samples/csharp/tutorials/AsyncStreams/finished/IssuePRreport/IssuePRreport/Program.cs#YieldReturnPage)]

<span data-ttu-id="476c1-168">Вы также можете удалить объявление `finalResults` ранее в этом методе и оператор `return`, следующий за измененным циклом.</span><span class="sxs-lookup"><span data-stu-id="476c1-168">You can also remove the declaration of `finalResults` earlier in this method and the `return` statement that follows the loop you modified.</span></span>

<span data-ttu-id="476c1-169">Вы завершили изменения для создания асинхронного потока.</span><span class="sxs-lookup"><span data-stu-id="476c1-169">You've finished the changes to generate an async stream.</span></span> <span data-ttu-id="476c1-170">Готовый метод должен напоминать код, указанный ниже.</span><span class="sxs-lookup"><span data-stu-id="476c1-170">The finished method should resemble the code below:</span></span>

[!code-csharp[FinishedGenerate](~/samples/csharp/tutorials/AsyncStreams/finished/IssuePRreport/IssuePRreport/Program.cs#GenerateAsyncStream)]

<span data-ttu-id="476c1-171">Затем измените код, который использует коллекцию, для асинхронного потока.</span><span class="sxs-lookup"><span data-stu-id="476c1-171">Next, you change the code that consumes the collection to consume the async stream.</span></span> <span data-ttu-id="476c1-172">Найдите следующий код в `Main`, который обрабатывает коллекцию проблем.</span><span class="sxs-lookup"><span data-stu-id="476c1-172">Find the following code in `Main` that processes the collection of issues:</span></span>

[!code-csharp[EnumerateOldStyle](~/samples/csharp/tutorials/AsyncStreams/start/IssuePRreport/IssuePRreport/Program.cs#EnumerateOldStyle)]

<span data-ttu-id="476c1-173">Замените код следующим циклом `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="476c1-173">Replace that code with the following `await foreach` loop:</span></span>

[!code-csharp[FinishedEnumerateAsyncStream](~/samples/csharp/tutorials/AsyncStreams/finished/IssuePRreport/IssuePRreport/Program.cs#EnumerateAsyncStream)]

<span data-ttu-id="476c1-174">Вы можете получить код для 	готового руководства, используемый в репозитории [dotnet/samples](https://github.com/dotnet/samples) в папке [csharp/tutorials/AsyncStreams](https://github.com/dotnet/samples/tree/master/csharp/tutorials/AsyncStreams/finished).</span><span class="sxs-lookup"><span data-stu-id="476c1-174">You can get the code for the finished tutorial from the [dotnet/samples](https://github.com/dotnet/samples) repository in the [csharp/tutorials/AsyncStreams](https://github.com/dotnet/samples/tree/master/csharp/tutorials/AsyncStreams/finished) folder.</span></span>

## <a name="run-the-finished-application"></a><span data-ttu-id="476c1-175">Запуск готового приложения</span><span class="sxs-lookup"><span data-stu-id="476c1-175">Run the finished application</span></span>

<span data-ttu-id="476c1-176">Снова запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="476c1-176">Run the application again.</span></span> <span data-ttu-id="476c1-177">Сравните его поведение с поведением начального приложения.</span><span class="sxs-lookup"><span data-stu-id="476c1-177">Contrast its behavior with the behavior of the starter application.</span></span> <span data-ttu-id="476c1-178">Первая страница результатов перечисляется, как только она становится доступной.</span><span class="sxs-lookup"><span data-stu-id="476c1-178">The first page of results is enumerated as soon as it's available.</span></span> <span data-ttu-id="476c1-179">Поскольку каждую новую страницу запрашивают и извлекают, результаты следующей страницы быстро перечисляются, возникает пауза.</span><span class="sxs-lookup"><span data-stu-id="476c1-179">There's an observable pause as each new page is requested and retrieved, then the next page's results are quickly enumerated.</span></span> <span data-ttu-id="476c1-180">Блок `try` / `catch` не требует обработки отмены. Вызывающий может прекратить перечисление коллекции.</span><span class="sxs-lookup"><span data-stu-id="476c1-180">The `try` / `catch` block isn't needed to handle cancellation: the caller can stop enumerating the collection.</span></span> <span data-ttu-id="476c1-181">Отчет о ходе выполнения четко сформирован, так как асинхронный поток формирует результаты скачивания каждой страницы.</span><span class="sxs-lookup"><span data-stu-id="476c1-181">Progress is clearly reported because the async stream generates results as each page is downloaded.</span></span>

<span data-ttu-id="476c1-182">Изучив код, вы увидите улучшения в использовании памяти.</span><span class="sxs-lookup"><span data-stu-id="476c1-182">You can see improvements in memory use by examining the code.</span></span> <span data-ttu-id="476c1-183">Вам больше не нужно выделять коллекцию для хранения всех результатов до их перечисления.</span><span class="sxs-lookup"><span data-stu-id="476c1-183">You no longer need to allocate a collection to store all the results before they're enumerated.</span></span> <span data-ttu-id="476c1-184">Вызывающий может определить, как использовать результаты и нужен ли набор хранилищ.</span><span class="sxs-lookup"><span data-stu-id="476c1-184">The caller can determine how to consume the results and if a storage collection is needed.</span></span>

<span data-ttu-id="476c1-185">Запустите начальное и готовое приложение, и вы увидите различия между реализациями самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="476c1-185">Run both the starter and finished applications and you can observe the differences between the implementations for yourself.</span></span> <span data-ttu-id="476c1-186">Вы можете удалить маркер доступа GitHub, созданный при начале работы с этим руководством, после завершения изучения.</span><span class="sxs-lookup"><span data-stu-id="476c1-186">You can delete the GitHub access token you created when you started this tutorial after you've finished.</span></span> <span data-ttu-id="476c1-187">Если злоумышленник получил доступ к этому маркеру, ему удастся получить доступ к API GitHub с помощью ваших учетных данных.</span><span class="sxs-lookup"><span data-stu-id="476c1-187">If an attacker gained access to that token, they could access GitHub APIs using your credentials.</span></span>