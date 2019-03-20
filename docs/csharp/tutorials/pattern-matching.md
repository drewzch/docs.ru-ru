---
title: Использование функций сопоставления шаблонов для расширения типов данных
description: Этом руководстве показано, как использовать методы сопоставления шаблонов для создания функций с помощью данных и алгоритмов, которые создаются отдельно.
ms.date: 03/13/2019
ms.custom: mvc
ms.openlocfilehash: 0d7c853709d0986710bf4d1a72daeb1f7cda3109
ms.sourcegitcommit: 16aefeb2d265e69c0d80967580365fabf0c5d39a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2019
ms.locfileid: "58125815"
---
# <a name="tutorial-using-pattern-matching-features-to-extend-data-types"></a><span data-ttu-id="a11fe-103">Учебник. Использование функций сопоставления шаблонов для расширения типов данных</span><span class="sxs-lookup"><span data-stu-id="a11fe-103">Tutorial: Using pattern matching features to extend data types</span></span>

<span data-ttu-id="a11fe-104">В C# 7 появились базовые функции сопоставления шаблонов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-104">C# 7 introduced basic pattern matching features.</span></span> <span data-ttu-id="a11fe-105">Эти функции были расширены в C# 8 за счет новых выражений и шаблонов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-105">Those features are extended in C# 8 with new expressions and patterns.</span></span> <span data-ttu-id="a11fe-106">Вы можете написать функции, которые работают так, будто вы расширили типы, которые могут быть в других библиотеках.</span><span class="sxs-lookup"><span data-stu-id="a11fe-106">You can write functionality that behaves as though you extended types that may be in other libraries.</span></span> <span data-ttu-id="a11fe-107">Еще один вариант использования шаблонов — создать функции, которые требуются для приложения и не являются фундаментальными функциями для расширяемого типа.</span><span class="sxs-lookup"><span data-stu-id="a11fe-107">Another use for patterns is to create functionality your application requires that isn't a fundamental feature of the type being extended.</span></span>

<span data-ttu-id="a11fe-108">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="a11fe-108">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a11fe-109">Распознавать случаи, в которых следует использовать сопоставление шаблонов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-109">How to recognize situations where pattern matching should be used.</span></span>
> * <span data-ttu-id="a11fe-110">Использовать выражения сопоставления шаблонов для реализации поведения с учетом типов и значений свойств.</span><span class="sxs-lookup"><span data-stu-id="a11fe-110">How to use pattern matching expressions to implement behavior based on types and property values.</span></span>
> * <span data-ttu-id="a11fe-111">Комбинировать сопоставление шаблонов и другие методы для создания полных алгоритмов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-111">How to combine pattern matching with other techniques to create complete algorithms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a11fe-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a11fe-112">Prerequisites</span></span>

<span data-ttu-id="a11fe-113">Вам нужно настроить свой компьютер для работы с .NET Core, включая предварительную версию компилятора C# 8.0.</span><span class="sxs-lookup"><span data-stu-id="a11fe-113">You'll need to set up your machine to run .NET Core, including the C# 8.0 preview compiler.</span></span> <span data-ttu-id="a11fe-114">Предварительная версия компилятора C# 8 доступна с последней предварительной версией [Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019+preview) или [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="a11fe-114">The C# 8 preview compiler is available with the latest [Visual Studio 2019 preview](https://visualstudio.microsoft.com/vs/preview/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019+preview), or the latest [.NET Core 3.0 preview](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

<span data-ttu-id="a11fe-115">В этом руководстве предполагается, что вы знакомы с C# и .NET, включая Visual Studio или .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="a11fe-115">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="scenarios-for-pattern-matching"></a><span data-ttu-id="a11fe-116">Сценарии для сопоставления шаблонов</span><span class="sxs-lookup"><span data-stu-id="a11fe-116">Scenarios for pattern matching</span></span>

<span data-ttu-id="a11fe-117">Современная разработка часто предусматривает использование данных из нескольких источников, а также представление информации и идей на основе этих данных в одном связном приложении.</span><span class="sxs-lookup"><span data-stu-id="a11fe-117">Modern development often includes integrating data from multiple sources and presenting information and insights from that data in a single cohesive application.</span></span> <span data-ttu-id="a11fe-118">У вас и вашей команды не всегда будет возможность контроля над всеми типами входящих данных или доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="a11fe-118">You and your team won't have control or access for all the types that represent the incoming data.</span></span>

<span data-ttu-id="a11fe-119">Для классического объектно-ориентированного приложения необходимо создавать в приложении типы данных, которые представляют каждый тип данных из этих нескольких источников.</span><span class="sxs-lookup"><span data-stu-id="a11fe-119">The classic object-oriented design would call for creating types in your application that represent each data type from those multiple data sources.</span></span> <span data-ttu-id="a11fe-120">Затем ваше приложение будет работать с этими новыми типами, создавать иерархии наследования и виртуальные методы, а также реализовывать абстракции.</span><span class="sxs-lookup"><span data-stu-id="a11fe-120">Then, your application would work with those new types, build inheritance hierarchies, create virtual methods, and implement abstractions.</span></span> <span data-ttu-id="a11fe-121">Эти методы работают, и иногда это лучшее средство для решения проблемы.</span><span class="sxs-lookup"><span data-stu-id="a11fe-121">Those techniques work, and sometimes they are the best tools.</span></span> <span data-ttu-id="a11fe-122">Но в некоторых случаях можно писать меньше кода.</span><span class="sxs-lookup"><span data-stu-id="a11fe-122">Other times you can write less code.</span></span> <span data-ttu-id="a11fe-123">Вы можете писать более понятный код, используя методы, которые разделяют сами данные и операции с этими данными.</span><span class="sxs-lookup"><span data-stu-id="a11fe-123">You can write more clear code using techniques that separate the data from the operations that manipulate that data.</span></span>

<span data-ttu-id="a11fe-124">В этом руководстве описано, как создать и оценить приложение, которое принимает входящие данные из нескольких внешних источников для одного сценария.</span><span class="sxs-lookup"><span data-stu-id="a11fe-124">In this tutorial, you'll create and explore an application that takes incoming data from several external sources for a single scenario.</span></span> <span data-ttu-id="a11fe-125">Вы увидите, что **сопоставление шаблонов** позволяет эффективно использовать и обрабатывать данные такими способами, которые изначально не были частью системы.</span><span class="sxs-lookup"><span data-stu-id="a11fe-125">You'll see how **pattern matching** provides an efficient way to consume and process that data in ways that weren't part of the original system.</span></span>

<span data-ttu-id="a11fe-126">Рассмотрим крупную транспортную сеть, в которой для управления трафиком используются дорожные сборы и тарификация на основе пиковой загрузки.</span><span class="sxs-lookup"><span data-stu-id="a11fe-126">Consider a major metro area that is using tolls and peak time pricing to manage traffic.</span></span> <span data-ttu-id="a11fe-127">Вы напишете приложение, которое рассчитывает плату за автомобиль в зависимости от его типа.</span><span class="sxs-lookup"><span data-stu-id="a11fe-127">You write an application that calculates tolls for a vehicle based on its type.</span></span> <span data-ttu-id="a11fe-128">Затем вы добавите цены с учетом количества пассажиров в автомобиле, а также</span><span class="sxs-lookup"><span data-stu-id="a11fe-128">Later enhancements incorporate pricing based on the number of occupants in the vehicle.</span></span> <span data-ttu-id="a11fe-129">времени и дня недели.</span><span class="sxs-lookup"><span data-stu-id="a11fe-129">Further enhancements add pricing based on the time and the day of the week.</span></span>

<span data-ttu-id="a11fe-130">Из этого краткого описания вы можете быстро составить иерархию объектов для моделирования этой системы.</span><span class="sxs-lookup"><span data-stu-id="a11fe-130">From that brief description, you may have quickly sketched out an object hierarchy to model this system.</span></span> <span data-ttu-id="a11fe-131">Но ваши данные поступают из разных источников, включая другие системы управления регистрацией транспортных средств.</span><span class="sxs-lookup"><span data-stu-id="a11fe-131">However, your data is coming from multiple sources like other vehicle registration management systems.</span></span> <span data-ttu-id="a11fe-132">Эти системы предоставляют разные классы для моделирования таких данных, и у вас нет единой объектной модели, которую можно использовать.</span><span class="sxs-lookup"><span data-stu-id="a11fe-132">These systems provide different classes to model that data and you don't have a single object model you can use.</span></span> <span data-ttu-id="a11fe-133">При работе с этим руководством для моделирования данных автомобиля вы будете использовать упрощенные классы из этих внешних систем, как показано в следующем примере кода:</span><span class="sxs-lookup"><span data-stu-id="a11fe-133">In this tutorial, you'll use these simplified classes to model for the vehicle data from these external systems, as shown in the following code:</span></span>

[!code-csharp[ExternalSystems](~/samples/csharp/tutorials/patterns/start/toll-calculator/ExternalSystems.cs)]

<span data-ttu-id="a11fe-134">Скачать начальный код можно из репозитория GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start).</span><span class="sxs-lookup"><span data-stu-id="a11fe-134">You can download the starter code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start) GitHub repository.</span></span> <span data-ttu-id="a11fe-135">Вы можете видеть, что классы транспортных средств принадлежат разным системам и находятся в разных пространствах имен.</span><span class="sxs-lookup"><span data-stu-id="a11fe-135">You can see that the vehicle classes are from different systems, and are in different namespaces.</span></span> <span data-ttu-id="a11fe-136">Вы не можете использовать другие базовые классы, кроме `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-136">No common base class, other than `System.Object` can be leveraged.</span></span>

## <a name="pattern-matching-designs"></a><span data-ttu-id="a11fe-137">Схемы сопоставления шаблонов</span><span class="sxs-lookup"><span data-stu-id="a11fe-137">Pattern matching designs</span></span>

<span data-ttu-id="a11fe-138">Сценарий, используемый в этом руководстве, позволяет выделить виды проблем, для решения которых рекомендуется использовать сопоставление шаблонов:</span><span class="sxs-lookup"><span data-stu-id="a11fe-138">The scenario used in this tutorial highlights the kinds of problems that are well suited to use pattern matching to solve:</span></span> 

- <span data-ttu-id="a11fe-139">Объекты, с которыми вам нужно работать, не находятся в иерархии объектов, которая соответствует вашим целям.</span><span class="sxs-lookup"><span data-stu-id="a11fe-139">The objects you need to work with aren't in an object hierarchy that matches your goals.</span></span> <span data-ttu-id="a11fe-140">Вам может потребоваться работать с классами, которые являются частью несвязанных систем.</span><span class="sxs-lookup"><span data-stu-id="a11fe-140">You may be working with classes that are part of unrelated systems.</span></span>
- <span data-ttu-id="a11fe-141">Функции, которые вы добавляете, не является частью основной абстракции для этих классов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-141">The functionality you're adding isn't part of the core abstraction for these classes.</span></span> <span data-ttu-id="a11fe-142">Плата за автомобиль *изменяется* в зависимости от его типа. При этом плата не является основной функцией этого автомобиля.</span><span class="sxs-lookup"><span data-stu-id="a11fe-142">The toll paid by a vehicle *changes* for different types of vehicles, but the toll isn't a core function of the vehicle.</span></span>

<span data-ttu-id="a11fe-143">Если *форма* данных и *операции* с данными не описаны вместе, можно использовать функцию сопоставления шаблонов в C#, чтобы упростить работу.</span><span class="sxs-lookup"><span data-stu-id="a11fe-143">When the *shape* of the data and the *operations* on that data are not described together, the pattern matching features in C# make it easier to work with.</span></span> 

## <a name="implement-the-basic-toll-calculations"></a><span data-ttu-id="a11fe-144">Реализация расчетов базового сбора</span><span class="sxs-lookup"><span data-stu-id="a11fe-144">Implement the basic toll calculations</span></span>

<span data-ttu-id="a11fe-145">Самый простой расчет базового сбора выполняется с учетом типа автомобиля:</span><span class="sxs-lookup"><span data-stu-id="a11fe-145">The most basic toll calculation relies only on the vehicle type:</span></span>

- <span data-ttu-id="a11fe-146">`Car` — 2,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-146">A `Car` is $2.00.</span></span>
- <span data-ttu-id="a11fe-147">`Taxi` — $3,50 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-147">A `Taxi` is $3.50.</span></span>
- <span data-ttu-id="a11fe-148">`Bus` — $5,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-148">A `Bus` is $5.00.</span></span>
- <span data-ttu-id="a11fe-149">`DeliveryTruck` — $10,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-149">A `DeliveryTruck` is $10.00</span></span>

<span data-ttu-id="a11fe-150">Создайте класс `TollCalculator` и реализуйте сопоставление шаблонов по типу автомобиля, чтобы получить сумму сбора.</span><span class="sxs-lookup"><span data-stu-id="a11fe-150">Create a new `TollCalculator` class, and implement pattern matching on the vehicle type to get the toll amount.</span></span>

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    public class TollCalculator
    {
        public decimal CalculateToll(object vehicle) =>
            vehicle switch
        {
            Car c           => 2.00m,
            Taxi t          => 3.50m,
            Bus b           => 5.00m,
            DeliveryTruck t => 10.00m,
            { }             => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
            null            => throw new ArgumentNullException(nameof(vehicle))
        };
    }
}
```

<span data-ttu-id="a11fe-151">В коде предыдущего примера используется **выражение switch** (которое отличается от оператора [`switch`](../language-reference/keywords/switch.md)), проверяющее **шаблон типа**.</span><span class="sxs-lookup"><span data-stu-id="a11fe-151">The preceding code uses a **switch expression** (not the same as a [`switch`](../language-reference/keywords/switch.md) statement) that tests the **type pattern**.</span></span> <span data-ttu-id="a11fe-152">**Выражение switch** начинается с переменной `vehicle` в приведенном выше коде, за которой следует ключевое слово `switch`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-152">A **switch expression** begins with the variable, `vehicle` in the preceding code, followed by the `switch` keyword.</span></span> <span data-ttu-id="a11fe-153">Далее следуют все **доступные ветви switch** внутри фигурных скобок.</span><span class="sxs-lookup"><span data-stu-id="a11fe-153">Next comes all the **switch arms** inside curly braces.</span></span> <span data-ttu-id="a11fe-154">Выражение `switch`вносит другие уточнения в синтаксис, который окружает оператор `switch`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-154">The `switch` expression makes other refinements to the syntax that surrounds the `switch` statement.</span></span> <span data-ttu-id="a11fe-155">Ключевое слово `case` опущено, и результатом каждой ветви является выражение.</span><span class="sxs-lookup"><span data-stu-id="a11fe-155">The `case` keyword is omitted, and the result of each arm is an expression.</span></span> <span data-ttu-id="a11fe-156">Последние две ветви демонстрируют новую функцию языка.</span><span class="sxs-lookup"><span data-stu-id="a11fe-156">The last two arms show a new language feature.</span></span> <span data-ttu-id="a11fe-157">Случай `{ }` соответствует любому ненулевому объекту, который не совпадает с предыдущей ветвью.</span><span class="sxs-lookup"><span data-stu-id="a11fe-157">The `{ }` case matches any non-null object that didn't match an earlier arm.</span></span> <span data-ttu-id="a11fe-158">Этот случай позволяет перехватить все неправильные типы, переданные этому методу.</span><span class="sxs-lookup"><span data-stu-id="a11fe-158">This arm catches any incorrect types passed to this method.</span></span> <span data-ttu-id="a11fe-159">Наконец шаблон со значением `null` перехватывается, когда этому методу передается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-159">Finally, the `null` pattern catches when `null` is passed to this method.</span></span> <span data-ttu-id="a11fe-160">Шаблон `null` может быть последним, так как шаблоны другого типа соответствуют только ненулевому объекту правильного типа.</span><span class="sxs-lookup"><span data-stu-id="a11fe-160">The `null` pattern can be last because the other type patterns match only a non-null object of the correct type.</span></span>

<span data-ttu-id="a11fe-161">Этот код можно проверить с помощью следующего кода в файле `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="a11fe-161">You can test this code using the following code in `Program.cs`:</span></span>

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    class Program
    {
        static void Main(string[] args)
        {
            var tollCalc = new TollCalculator();

            var car = new Car();
            var taxi = new Taxi();
            var bus = new Bus();
            var truck = new DeliveryTruck();

            Console.WriteLine($"The toll for a car is {tollCalc.CalculateToll(car)}");
            Console.WriteLine($"The toll for a taxi is {tollCalc.CalculateToll(taxi)}");
            Console.WriteLine($"The toll for a bus is {tollCalc.CalculateToll(bus)}");
            Console.WriteLine($"The toll for a truck is {tollCalc.CalculateToll(truck)}");

            try
            {
                tollCalc.CalculateToll("this will fail");
            }
            catch (ArgumentException e)
            {
                Console.WriteLine("Caught an argument exception when using the wrong type", DayOfWeek.Friday);
            }
            try
            {
                tollCalc.CalculateToll(null);
            }
            catch (ArgumentNullException e)
            {
                Console.WriteLine("Caught an argument exception when using null");
            }
        }
    }
}
```

<span data-ttu-id="a11fe-162">Этот код включен в начальный проект, но закомментирован. Удалите символы комментария, чтобы проверить написанный код.</span><span class="sxs-lookup"><span data-stu-id="a11fe-162">That code is included in the starter project, but is commented out. Remove the comments, and you can test what you've written.</span></span>

<span data-ttu-id="a11fe-163">На этом примере можно понять, как шаблоны помогают создавать алгоритмы, в которых код и данные разделены.</span><span class="sxs-lookup"><span data-stu-id="a11fe-163">You're starting to see how patterns can help you create algorithms where the code and the data are separate.</span></span> <span data-ttu-id="a11fe-164">Выражение `switch` проверяет тип и создает различные значения в зависимости от результатов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-164">The `switch` expression tests the type and produces different values based on the results.</span></span> <span data-ttu-id="a11fe-165">Но это только начало.</span><span class="sxs-lookup"><span data-stu-id="a11fe-165">That's only the beginning.</span></span>

## <a name="add-occupancy-pricing"></a><span data-ttu-id="a11fe-166">Добавление цен с учетом загруженности дороги</span><span class="sxs-lookup"><span data-stu-id="a11fe-166">Add occupancy pricing</span></span>

<span data-ttu-id="a11fe-167">Орган, взимающий сбор, поощряет передвижение автомобилей с максимальным количеством пассажиров.</span><span class="sxs-lookup"><span data-stu-id="a11fe-167">The toll authority wants to encourage vehicles to travel at maximum capacity.</span></span> <span data-ttu-id="a11fe-168">Следовательно, плата за проезд возрастает, когда в автомобиле находится меньше пассажиров, и наоборот:</span><span class="sxs-lookup"><span data-stu-id="a11fe-168">They've decided to charge more when vehicles have fewer passengers, and encourage full vehicles by offering lower pricing:</span></span>

- <span data-ttu-id="a11fe-169">Автомобили и такси без пассажиров должны заплатить дополнительные 0,50 долл. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-169">Cars and taxis with no passengers pay an extra $0.50.</span></span>
- <span data-ttu-id="a11fe-170">Автомобили и такси с двумя пассажирами получают скидку 0,50 долл. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-170">Cars and taxis with two passengers get a 0.50 discount.</span></span>
- <span data-ttu-id="a11fe-171">Автомобили и такси с тремя пассажирами получают скидку 1,00 долл. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-171">Cars and taxis with three or more passengers get a $1.00 discount.</span></span>
- <span data-ttu-id="a11fe-172">Если автобус заполнен менее чем на половину, взимается дополнительная плата — 2,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-172">Buses that are less than 50% full pay an extra $2.00.</span></span>
- <span data-ttu-id="a11fe-173">Если автобус заполнен более чем на 90 %, действует скидка 1,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-173">Buses that are more than 90% full get a $1.00 discount.</span></span>

<span data-ttu-id="a11fe-174">Эти правила можно реализовать с помощью **шаблона свойств** в этом же выражении switch.</span><span class="sxs-lookup"><span data-stu-id="a11fe-174">These rules can be implemented using the **property pattern** in the same switch expression.</span></span> <span data-ttu-id="a11fe-175">Шаблон свойств проверяет свойства объекта после определения его типа.</span><span class="sxs-lookup"><span data-stu-id="a11fe-175">The property pattern examines properties of the object once the type has been determined.</span></span>  <span data-ttu-id="a11fe-176">Один случай для `Car` включает четыре разных случая:</span><span class="sxs-lookup"><span data-stu-id="a11fe-176">The single case for a `Car` expands to four different cases:</span></span>

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1 }       => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c when c.Passengers > 2 => 2.00m - 1.0m,

    // ...
};
```

<span data-ttu-id="a11fe-177">Первые три случая проверяют тип как `Car`, а затем проверяют значение свойства `Passengers`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-177">The first three cases test the type as a `Car`, then check the value of the `Passengers` property.</span></span> <span data-ttu-id="a11fe-178">Если оба типа совпадают, это выражение вычисляется и возвращается результат.</span><span class="sxs-lookup"><span data-stu-id="a11fe-178">If both match, that expression is evaluated and returned.</span></span> <span data-ttu-id="a11fe-179">В последнем предложении показано предложение `when` ветви switch.</span><span class="sxs-lookup"><span data-stu-id="a11fe-179">The final clause shows the `when` clause of a switch arm.</span></span> <span data-ttu-id="a11fe-180">Используйте предложение `when` для проверки условий для свойства, отличающихся от равенства.</span><span class="sxs-lookup"><span data-stu-id="a11fe-180">You use the `when` clause to test conditions other than equality on a property.</span></span> <span data-ttu-id="a11fe-181">В приведенном выше примере предложение `when` проверяет, что в автомобиле находится более двух пассажиров.</span><span class="sxs-lookup"><span data-stu-id="a11fe-181">In the preceding example, the `when` clause tests to see that there are more than 2 passengers in the car.</span></span> <span data-ttu-id="a11fe-182">Строго говоря, в этом примере это не обязательно.</span><span class="sxs-lookup"><span data-stu-id="a11fe-182">Strictly speaking, it's not necessary in this example.</span></span>

<span data-ttu-id="a11fe-183">Необходимо также реализовать аналогичные варианты для такси:</span><span class="sxs-lookup"><span data-stu-id="a11fe-183">You would also expand the cases for taxis in a similar manner:</span></span>

```csharp
vehicle switch
{
    // ...

    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,

    // ...
};
```

<span data-ttu-id="a11fe-184">В предыдущем примере предложение `when` было опущено в последнем случае.</span><span class="sxs-lookup"><span data-stu-id="a11fe-184">In the preceding example, the `when` clause was omitted on the final case.</span></span>

<span data-ttu-id="a11fe-185">Затем реализуйте правила заполнения транспорта, расширив регистры для автобусов, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a11fe-185">Next, implement the occupancy rules by expanding the cases for buses, as shown in the following example:</span></span>

```csharp
vehicle switch
{
    // ...

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m, 
    Bus b => 5.00m,
    
    // ...
};
```

<span data-ttu-id="a11fe-186">Орган, взимающий сборы, не учитывает количество пассажиров в грузовых автомобилях, выполняющих доставку.</span><span class="sxs-lookup"><span data-stu-id="a11fe-186">The toll authority isn't concerned with the number of passengers in the delivery trucks.</span></span> <span data-ttu-id="a11fe-187">Вместо этого, плата взимается с учетом веса грузовых автомобилей.</span><span class="sxs-lookup"><span data-stu-id="a11fe-187">Instead, they charge more based on the weight class of the trucks.</span></span> <span data-ttu-id="a11fe-188">Для грузовых автомобилей весом более 2268 кг взимается дополнительная плата в размере 5,00 долл. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-188">Trucks over 5000 lbs are charged an extra $5.00.</span></span> <span data-ttu-id="a11fe-189">Для легких грузовиков весом до 1361 кг применяется скидка 2,00 дол. США.</span><span class="sxs-lookup"><span data-stu-id="a11fe-189">Light trucks, under 3000 lbs are given a $2.00 discount.</span></span>  <span data-ttu-id="a11fe-190">Это правило, реализуется с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="a11fe-190">That rule is implemented with the following code:</span></span>

```csharp
vehicle switch
{
    // ...

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

<span data-ttu-id="a11fe-191">Завершив работу, вы получите метод, который выглядит приблизительно так:</span><span class="sxs-lookup"><span data-stu-id="a11fe-191">When you've finished, you'll have a method that looks much like the following:</span></span>

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1}        => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c when c.Passengers > 2 => 2.00m - 1.0m,
   
    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,
    
    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m, 
    Bus b => 5.00m,
    
    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

<span data-ttu-id="a11fe-192">Многие из этих вариантов являются примерами **рекурсивных шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="a11fe-192">Many of these switch arms are examples of **recursive patterns**.</span></span> <span data-ttu-id="a11fe-193">Например, в `Car { Passengers: 1}` показан шаблон константы внутри шаблона свойств.</span><span class="sxs-lookup"><span data-stu-id="a11fe-193">For example, `Car { Passengers: 1}` shows a constant pattern inside a property pattern.</span></span>

<span data-ttu-id="a11fe-194">Вы можете уменьшить количество повторяемых участков кода, использовав вложенные операторы switch.</span><span class="sxs-lookup"><span data-stu-id="a11fe-194">You can make this code less repetitive by using nested switches.</span></span> <span data-ttu-id="a11fe-195">В предыдущих примерах объекты `Car` и `Taxi` имеют по четыре варианта.</span><span class="sxs-lookup"><span data-stu-id="a11fe-195">The `Car` and `Taxi` both have four different arms in the preceding examples.</span></span> <span data-ttu-id="a11fe-196">В обоих случаях вы можете создать шаблон типа, который передается в шаблон свойства.</span><span class="sxs-lookup"><span data-stu-id="a11fe-196">In both cases, you can create a type pattern that feeds into a property pattern.</span></span> <span data-ttu-id="a11fe-197">Пример использования этого способа показан в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="a11fe-197">This technique is shown in the following code:</span></span>

```csharp
public decimal CalculateToll(object vehicle) =>
    vehicle switch
    {
        Car c => c.Passengers switch
        {
            0 => 2.00m + 0.5m,
            1 => 2.0m,
            2 => 2.0m - 0.5m,
            _ => 2.00m - 1.0m
        },
    
        Taxi t => t.Fares switch
        {
            0 => 3.50m + 1.00m,
            1 => 3.50m,
            2 => 3.50m - 0.50m,
            _ => 3.50m - 1.00m
        },
    
        Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
        Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m, 
        Bus b => 5.00m,
    
        DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
        DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
        DeliveryTruck t => 10.00m,

        { }  => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
        null => throw new ArgumentNullException(nameof(vehicle))
    };
```

<span data-ttu-id="a11fe-198">В предыдущем примере применение рекурсивного выражения позволяет не использовать повторно ветви `Car` и `Taxi` с дочерними элементами, которые проверяют значение свойства.</span><span class="sxs-lookup"><span data-stu-id="a11fe-198">In the preceding sample, using a recursive expression means you don't repeat the `Car` and `Taxi` arms contain child arms that test the property value.</span></span> <span data-ttu-id="a11fe-199">Этот метод не используется для ветвей `Bus` и `DeliveryTruck`, так как они представляют собой проверочные диапазоны для свойства, а не дискретные значения.</span><span class="sxs-lookup"><span data-stu-id="a11fe-199">This technique isn't used for the `Bus` and `DeliveryTruck` arms because those arms are testing ranges for the property, not discrete values.</span></span>

## <a name="add-peak-pricing"></a><span data-ttu-id="a11fe-200">Добавление цен с учетом пиковой нагрузки</span><span class="sxs-lookup"><span data-stu-id="a11fe-200">Add peak pricing</span></span>

<span data-ttu-id="a11fe-201">Для последней функции орган, взимающий сборы, хочет добавить цену с учетом пиковой нагрузки.</span><span class="sxs-lookup"><span data-stu-id="a11fe-201">For the final feature, the toll authority wants to add time sensitive peak pricing.</span></span> <span data-ttu-id="a11fe-202">Утром и вечером, когда дороги наиболее загружены, сумма сбора удваивается.</span><span class="sxs-lookup"><span data-stu-id="a11fe-202">During the morning and evening rush hours, the tolls are doubled.</span></span> <span data-ttu-id="a11fe-203">Это правило влияет только на движение в одном направлении: при въезде в город утром и на выезде вечером в час пик.</span><span class="sxs-lookup"><span data-stu-id="a11fe-203">That rule only affects traffic in one direction: inbound to the city in the morning, and outbound in the evening rush hour.</span></span> <span data-ttu-id="a11fe-204">В другое время в течение рабочего дня плата увеличивается на 50 %.</span><span class="sxs-lookup"><span data-stu-id="a11fe-204">During other times during the workday, tolls increase by 50%.</span></span> <span data-ttu-id="a11fe-205">Поздно ночью и ранним утром плата уменьшается на 25 %.</span><span class="sxs-lookup"><span data-stu-id="a11fe-205">Late night and early morning, tolls are reduced by 25%.</span></span> <span data-ttu-id="a11fe-206">В выходные дни взимается стандартная плата, независимо от времени суток.</span><span class="sxs-lookup"><span data-stu-id="a11fe-206">During the weekend, it's the normal rate, regardless of the time.</span></span>

<span data-ttu-id="a11fe-207">Для этой функции вы будете использовать сопоставление шаблонов, но вместе с другими методами.</span><span class="sxs-lookup"><span data-stu-id="a11fe-207">You'll use pattern matching for this feature, but you'll integrate it with other techniques.</span></span> <span data-ttu-id="a11fe-208">Вы можете создать одно выражение для сопоставления шаблонов, учитывающее все комбинации направления, дня недели и времени.</span><span class="sxs-lookup"><span data-stu-id="a11fe-208">You could build a single pattern match expression that would account all the combinations of direction, day of the week, and time.</span></span> <span data-ttu-id="a11fe-209">Результат будет представлен в виде сложного выражения.</span><span class="sxs-lookup"><span data-stu-id="a11fe-209">The result would be a complicated expression.</span></span> <span data-ttu-id="a11fe-210">Такое выражение затрудняет чтение и понимание кода.</span><span class="sxs-lookup"><span data-stu-id="a11fe-210">It would be hard to read and difficult to understand.</span></span> <span data-ttu-id="a11fe-211">В таком случает будет сложнее обеспечить правильность результатов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-211">That makes it hard to ensure correctness.</span></span> <span data-ttu-id="a11fe-212">Но вы можете объединить эти методы для создания набора значений, который кратко описывает все эти состояния.</span><span class="sxs-lookup"><span data-stu-id="a11fe-212">Instead, combine those methods to build a tuple of values that concisely describes all those states.</span></span> <span data-ttu-id="a11fe-213">Затем, используйте сопоставление шаблонов, чтобы вычислить множитель для платы за проезд.</span><span class="sxs-lookup"><span data-stu-id="a11fe-213">Then use pattern matching to calculate a multiplier for the toll.</span></span> <span data-ttu-id="a11fe-214">Кортеж содержит три дискретные условия:</span><span class="sxs-lookup"><span data-stu-id="a11fe-214">The tuple contains three discrete conditions:</span></span>

- <span data-ttu-id="a11fe-215">День — выходной или рабочий.</span><span class="sxs-lookup"><span data-stu-id="a11fe-215">The day is either a weekday or a weekend.</span></span>
- <span data-ttu-id="a11fe-216">Диапазон времени, за который взимается плата.</span><span class="sxs-lookup"><span data-stu-id="a11fe-216">The band of time when the toll is collected.</span></span>
- <span data-ttu-id="a11fe-217">Направление в город или за город.</span><span class="sxs-lookup"><span data-stu-id="a11fe-217">The direction is into the city or out of the city</span></span>

<span data-ttu-id="a11fe-218">В таблице ниже показаны комбинации входных значений и множителя для цены в часы пик:</span><span class="sxs-lookup"><span data-stu-id="a11fe-218">The following table shows the combinations of input values and the peak pricing multiplier:</span></span>

| <span data-ttu-id="a11fe-219">День</span><span class="sxs-lookup"><span data-stu-id="a11fe-219">Day</span></span>        | <span data-ttu-id="a11fe-220">Время</span><span class="sxs-lookup"><span data-stu-id="a11fe-220">Time</span></span>         | <span data-ttu-id="a11fe-221">Направление</span><span class="sxs-lookup"><span data-stu-id="a11fe-221">Direction</span></span> | <span data-ttu-id="a11fe-222">Премиум</span><span class="sxs-lookup"><span data-stu-id="a11fe-222">Premium</span></span> |
| ---------- | ------------ | --------- |--------:|
| <span data-ttu-id="a11fe-223">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-223">Weekday</span></span>    | <span data-ttu-id="a11fe-224">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-224">morning rush</span></span> | <span data-ttu-id="a11fe-225">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-225">inbound</span></span>   | <span data-ttu-id="a11fe-226">x 2,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-226">x 2.00</span></span>  |
| <span data-ttu-id="a11fe-227">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-227">Weekday</span></span>    | <span data-ttu-id="a11fe-228">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-228">morning rush</span></span> | <span data-ttu-id="a11fe-229">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-229">outbound</span></span>  | <span data-ttu-id="a11fe-230">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-230">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-231">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-231">Weekday</span></span>    | <span data-ttu-id="a11fe-232">Дневное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-232">daytime</span></span>      | <span data-ttu-id="a11fe-233">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-233">inbound</span></span>   | <span data-ttu-id="a11fe-234">x 1,50</span><span class="sxs-lookup"><span data-stu-id="a11fe-234">x 1.50</span></span>  |
| <span data-ttu-id="a11fe-235">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-235">Weekday</span></span>    | <span data-ttu-id="a11fe-236">Дневное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-236">daytime</span></span>      | <span data-ttu-id="a11fe-237">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-237">outbound</span></span>  | <span data-ttu-id="a11fe-238">x 1,50</span><span class="sxs-lookup"><span data-stu-id="a11fe-238">x 1.50</span></span>  |
| <span data-ttu-id="a11fe-239">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-239">Weekday</span></span>    | <span data-ttu-id="a11fe-240">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-240">evening rush</span></span> | <span data-ttu-id="a11fe-241">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-241">inbound</span></span>   | <span data-ttu-id="a11fe-242">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-242">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-243">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-243">Weekday</span></span>    | <span data-ttu-id="a11fe-244">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-244">evening rush</span></span> | <span data-ttu-id="a11fe-245">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-245">outbound</span></span>  | <span data-ttu-id="a11fe-246">x 2,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-246">x 2.00</span></span>  |
| <span data-ttu-id="a11fe-247">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-247">Weekday</span></span>    | <span data-ttu-id="a11fe-248">Ночное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-248">overnight</span></span>    | <span data-ttu-id="a11fe-249">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-249">inbound</span></span>   | <span data-ttu-id="a11fe-250">x 0,75</span><span class="sxs-lookup"><span data-stu-id="a11fe-250">x 0.75</span></span>  |
| <span data-ttu-id="a11fe-251">День недели</span><span class="sxs-lookup"><span data-stu-id="a11fe-251">Weekday</span></span>    | <span data-ttu-id="a11fe-252">Ночное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-252">overnight</span></span>    | <span data-ttu-id="a11fe-253">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-253">outbound</span></span>  | <span data-ttu-id="a11fe-254">x 0,75</span><span class="sxs-lookup"><span data-stu-id="a11fe-254">x 0.75</span></span>  |
| <span data-ttu-id="a11fe-255">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-255">Weekend</span></span>    | <span data-ttu-id="a11fe-256">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-256">morning rush</span></span> | <span data-ttu-id="a11fe-257">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-257">inbound</span></span>   | <span data-ttu-id="a11fe-258">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-258">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-259">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-259">Weekend</span></span>    | <span data-ttu-id="a11fe-260">Утренний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-260">morning rush</span></span> | <span data-ttu-id="a11fe-261">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-261">outbound</span></span>  | <span data-ttu-id="a11fe-262">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-262">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-263">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-263">Weekend</span></span>    | <span data-ttu-id="a11fe-264">Дневное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-264">daytime</span></span>      | <span data-ttu-id="a11fe-265">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-265">inbound</span></span>   | <span data-ttu-id="a11fe-266">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-266">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-267">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-267">Weekend</span></span>    | <span data-ttu-id="a11fe-268">Дневное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-268">daytime</span></span>      | <span data-ttu-id="a11fe-269">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-269">outbound</span></span>  | <span data-ttu-id="a11fe-270">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-270">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-271">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-271">Weekend</span></span>    | <span data-ttu-id="a11fe-272">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-272">evening rush</span></span> | <span data-ttu-id="a11fe-273">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-273">inbound</span></span>   | <span data-ttu-id="a11fe-274">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-274">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-275">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-275">Weekend</span></span>    | <span data-ttu-id="a11fe-276">Вечерний час пик</span><span class="sxs-lookup"><span data-stu-id="a11fe-276">evening rush</span></span> | <span data-ttu-id="a11fe-277">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-277">outbound</span></span>  | <span data-ttu-id="a11fe-278">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-278">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-279">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-279">Weekend</span></span>    | <span data-ttu-id="a11fe-280">Ночное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-280">overnight</span></span>    | <span data-ttu-id="a11fe-281">Въезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-281">inbound</span></span>   | <span data-ttu-id="a11fe-282">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-282">x 1.00</span></span>  |
| <span data-ttu-id="a11fe-283">Выходные</span><span class="sxs-lookup"><span data-stu-id="a11fe-283">Weekend</span></span>    | <span data-ttu-id="a11fe-284">Ночное время</span><span class="sxs-lookup"><span data-stu-id="a11fe-284">overnight</span></span>    | <span data-ttu-id="a11fe-285">Выезд</span><span class="sxs-lookup"><span data-stu-id="a11fe-285">outbound</span></span>  | <span data-ttu-id="a11fe-286">x 1,00</span><span class="sxs-lookup"><span data-stu-id="a11fe-286">x 1.00</span></span>  |

<span data-ttu-id="a11fe-287">Для этих трех переменных существует 16 разных комбинаций.</span><span class="sxs-lookup"><span data-stu-id="a11fe-287">There are 16 different combinations of the three variables.</span></span> <span data-ttu-id="a11fe-288">Комбинируя некоторые условия, вы упростите окончательное выражение switch.</span><span class="sxs-lookup"><span data-stu-id="a11fe-288">By combining some of the conditions, you'll simplify the final switch expression.</span></span>

<span data-ttu-id="a11fe-289">В системе для сбора платы структура <xref:System.DateTime> используется для определения времени сбора.</span><span class="sxs-lookup"><span data-stu-id="a11fe-289">The system that collects the tools uses a <xref:System.DateTime> structure for the time when the toll was collection.</span></span> <span data-ttu-id="a11fe-290">Выполните методы-члены, которые создают переменные из предыдущей таблицы.</span><span class="sxs-lookup"><span data-stu-id="a11fe-290">Build member methods that create the variables from the preceding table.</span></span>  <span data-ttu-id="a11fe-291">В следующей функции для сопоставления шаблонов используется выражение switch, позволяющее определить, что <xref:System.DateTime> соответствует выходным или рабочим дням недели:</span><span class="sxs-lookup"><span data-stu-id="a11fe-291">The following function uses as pattern matching switch expression to express whether a <xref:System.DateTime> represents a weekend or a weekday:</span></span>

```csharp
private static bool IsWeekDay(DateTime timeOfToll) =>
    timeOfToll.DayOfWeek switch
    {
        DayOfWeek.Monday    => true,
        DayOfWeek.Tuesday   => true,
        DayOfWeek.Wednesday => true,
        DayOfWeek.Thursday  => true,
        DayOfWeek.Friday    => true,
        DayOfWeek.Saturday  => false,
        DayOfWeek.Sunday    => false
    };
```

<span data-ttu-id="a11fe-292">Этот метод работает, но фрагменты кода повторяются.</span><span class="sxs-lookup"><span data-stu-id="a11fe-292">That method works, but it's repetitious.</span></span> <span data-ttu-id="a11fe-293">Вы можете упростить его, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a11fe-293">You can simplify it, as shown in the following code:</span></span>

[!code-csharp[IsWeekDay](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#IsWeekDay)]

<span data-ttu-id="a11fe-294">Затем добавьте такую же функцию, чтобы создать временные интервалы:</span><span class="sxs-lookup"><span data-stu-id="a11fe-294">Next, add a similar function to categorize the time into the blocks:</span></span>

[!code-csharp[GetTimeBand](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#GetTimeBand)]

<span data-ttu-id="a11fe-295">В предыдущем методе сопоставление шаблонов не применяется.</span><span class="sxs-lookup"><span data-stu-id="a11fe-295">The previous method doesn't use pattern matching.</span></span> <span data-ttu-id="a11fe-296">В этом случае проще использовать каскад операторов `if`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-296">It's clearer using a familiar cascade of `if` statements.</span></span> <span data-ttu-id="a11fe-297">Чтобы преобразовать каждый диапазон времени в дискретную величину, добавьте закрытое перечисление `enum`.</span><span class="sxs-lookup"><span data-stu-id="a11fe-297">You do add a private `enum` to convert each range of time to a discrete value.</span></span>

<span data-ttu-id="a11fe-298">Создав эти методы, можно использовать другое выражение `switch` с **шаблоном кортежа** для вычисления премиум-цены.</span><span class="sxs-lookup"><span data-stu-id="a11fe-298">After you create those methods, you can use another `switch` expression with the **tuple pattern** to calculate the pricing premium.</span></span> <span data-ttu-id="a11fe-299">Вы можете записать выражение `switch` сразу со всеми 16 вариантами:</span><span class="sxs-lookup"><span data-stu-id="a11fe-299">You could build a `switch` expression with all 16 arms:</span></span>

[!code-csharp[FullTuplePattern](~/samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#TuplePatternOne)]

<span data-ttu-id="a11fe-300">Код выше работает, но его можно упростить.</span><span class="sxs-lookup"><span data-stu-id="a11fe-300">The above code works, but it can be simplified.</span></span> <span data-ttu-id="a11fe-301">Плата для всех восьми сочетаний в выходные дни одинаковая.</span><span class="sxs-lookup"><span data-stu-id="a11fe-301">All eight combinations for the weekend have the same toll.</span></span> <span data-ttu-id="a11fe-302">Вы можете заменить все восемь вариантов одной строкой кода:</span><span class="sxs-lookup"><span data-stu-id="a11fe-302">You can replace all eight with the following one line:</span></span>

```csharp
(false, _, _) => 1.0m,
```

<span data-ttu-id="a11fe-303">Для входящего и исходящего трафика используется одинаковый множитель в дневное и ночное время в рабочие дни.</span><span class="sxs-lookup"><span data-stu-id="a11fe-303">Both inbound and outbound traffic have the same multiplier during the weekday daytime and overnight hours.</span></span> <span data-ttu-id="a11fe-304">Эти четыре случая можно заменить следующими двумя строками:</span><span class="sxs-lookup"><span data-stu-id="a11fe-304">Those four switch arms can be replaced with the follow two lines:</span></span>

```csharp
(true, TimeBand.Overnight, _) => 0.75m,
(true, TimeBand.Daytime, _)   => 1.5m,
```

<span data-ttu-id="a11fe-305">После этих двух изменений код должен выглядеть как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="a11fe-305">The code should look like the following code after those two changes:</span></span>

```csharp
public decimal PeakTimePremium(DateTime timeOfToll, bool inbound) =>
    (IsWeekDay(timeOfToll), GetTimeBand(timeOfToll), inbound) switch
    {
        (true, TimeBand.MorningRush, true)  => 2.00m,
        (true, TimeBand.MorningRush, false) => 1.00m,
        (true, TimeBand.Daytime,     _)     => 1.50m,
        (true, TimeBand.EveningRush, true)  => 1.00m,
        (true, TimeBand.EveningRush, false) => 2.00m,
        (true, TimeBand.Overnight,   _)     => 0.75m,
        (false, _,                   _)     => 1.00m,
    };
```

<span data-ttu-id="a11fe-306">Наконец, вы можете удалить два часа пик, за которые взимается обычная плата.</span><span class="sxs-lookup"><span data-stu-id="a11fe-306">Finally, you can remove the two rush hour times that pay the regular price.</span></span> <span data-ttu-id="a11fe-307">Удалив эти ветви, можно заменить `false` пустой переменной (`_`) в последней ветви switch.</span><span class="sxs-lookup"><span data-stu-id="a11fe-307">Once you remove those arms, you can replace the `false` with a discard (`_`) in the final switch arm.</span></span> <span data-ttu-id="a11fe-308">У вас получится следующий законченный метод:</span><span class="sxs-lookup"><span data-stu-id="a11fe-308">You'll have the following finished method:</span></span>

[!code-csharp[SimplifiedTuplePattern](../../../samples/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#FinalTuplePattern)]

<span data-ttu-id="a11fe-309">В этом примере продемонстрировано одно из преимуществ сопоставления шаблонов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-309">This example highlights one of the advantages of pattern matching.</span></span> <span data-ttu-id="a11fe-310">Ветви шаблона вычисляются по порядку.</span><span class="sxs-lookup"><span data-stu-id="a11fe-310">The pattern branches are evaluated in order.</span></span> <span data-ttu-id="a11fe-311">Если поменять их местами так, чтобы начальная ветвь обрабатывала один из последних случаев, компилятор выдает предупреждение.</span><span class="sxs-lookup"><span data-stu-id="a11fe-311">If you rearrange them so that an earlier branch handles one of your later cases, the compiler warns you.</span></span> <span data-ttu-id="a11fe-312">Эти правила языка облегчают реализацию упрощений, выполненных выше, и гарантируют, что код не изменится.</span><span class="sxs-lookup"><span data-stu-id="a11fe-312">Those language rules made it easier to do the preceding simplifications with confidence that the code didn't change.</span></span>

<span data-ttu-id="a11fe-313">Сопоставление шаблонов предусматривает естественный синтаксис для реализации решений, отличающихся от созданных с использованием объектно-ориентированных методов.</span><span class="sxs-lookup"><span data-stu-id="a11fe-313">Pattern matching provides a natural syntax to implement different solutions than you'd create if you used object-oriented techniques.</span></span> <span data-ttu-id="a11fe-314">В облаке данные и функции хранятся рядом.</span><span class="sxs-lookup"><span data-stu-id="a11fe-314">The cloud is causing data and functionality to live apart.</span></span> <span data-ttu-id="a11fe-315">*Форма* данных и *операции* в облаке не обязательно описываются вместе.</span><span class="sxs-lookup"><span data-stu-id="a11fe-315">The *shape* of the data and the *operations* on it aren't necessarily described together.</span></span> <span data-ttu-id="a11fe-316">В этом руководстве показано, что существующие данные можно использовать совершенно иначе, чем в первоначальной функции.</span><span class="sxs-lookup"><span data-stu-id="a11fe-316">In this tutorial, you consumed existing data in entirely different ways from its original function.</span></span> <span data-ttu-id="a11fe-317">Сопоставление шаблонов позволяет создавать функции с использованием нужных типов, хотя их не удается расширить.</span><span class="sxs-lookup"><span data-stu-id="a11fe-317">Pattern matching gave you the ability to write functionality that over those types, even though you couldn't extend them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a11fe-318">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a11fe-318">Next steps</span></span>

<span data-ttu-id="a11fe-319">Скачать готовый код можно из репозитория GitHub [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished).</span><span class="sxs-lookup"><span data-stu-id="a11fe-319">You can download the finished code from the [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished) GitHub repository.</span></span> <span data-ttu-id="a11fe-320">Изучите шаблоны самостоятельно и используйте эту методику во время написания кода.</span><span class="sxs-lookup"><span data-stu-id="a11fe-320">Explore patterns on your own and add this technique into your regular coding activities.</span></span> <span data-ttu-id="a11fe-321">Это позволит вам подойти к решению проблем с другой стороны, чтобы создавать новые функции.</span><span class="sxs-lookup"><span data-stu-id="a11fe-321">Learning these techniques gives you another way to approach problems and create new functionality.</span></span>