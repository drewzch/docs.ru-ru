---
title: Кэширование в приложениях платформы .NET Framework
ms.date: 03/30/2017
helpviewer_keywords:
- ASP.NET caching
- caching [.NET Framework]
- caching [ASP.NET]
ms.assetid: c4b47ee0-4b82-4124-9bce-818088385e34
ms.openlocfilehash: 2a0d138151722a76133da45c166c51d7f3bb0a31
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2019
ms.locfileid: "74428200"
---
# <a name="caching-in-net-framework-applications"></a>Кэширование в приложениях платформы .NET Framework
Кэширование позволяет хранить данные в памяти для быстрого доступа. При повторном доступе к данным приложения могут получать их из кэша вместо извлечения из исходного источника. Это может повысить производительность и масштабируемость. Кроме того, кэширование обеспечивает доступность данных при временной недоступности источника данных.  
  
 Платформа .NET Framework предоставляет возможность кэширования, которую можно использовать для улучшения производительности и масштабируемости как серверных, так и клиентских приложений Windows, включая ASP.NET.  
  
> [!NOTE]
> В .NET Framework 3,5 и более ранних версиях ASP.NET предоставил реализацию кэша в памяти в пространстве имен <xref:System.Web.Caching>. В предыдущих версиях .NET Framework кэширование было доступно только в пространстве имен <xref:System.Web> и, следовательно, требовало зависимости от классов ASP.NET. В платформе .NET Framework 4 пространство имен <xref:System.Runtime.Caching> содержит интерфейсы API, предназначенные как для веб-приложений, так и для приложений, не связанных с Интернетом.  
  
## <a name="caching-data"></a>Кэширование данных  
 Информацию можно кэшировать с помощью классов в пространстве имен <xref:System.Runtime.Caching>. Классы кэширования в нем предоставляют перечисленные ниже возможности.  
  
- Абстрактные типы, которые образуют основу для создания пользовательских реализаций кэша.  
  
- Конкретная реализация кэша объектов в памяти.  
  
 Абстрактный базовый класс кэширования (<xref:System.Runtime.Caching.ObjectCache>) определяет следующие задачи кэширования:  
  
- создание записей кэша и управление ими;  
  
- указание сведений об окончании срока действия и вытеснении;  
  
- активация событий, создаваемых в ответ на изменение записей кэша.  
  
 Класс <xref:System.Runtime.Caching.MemoryCache> является реализацией в памяти кэша объектов, представленного классом <xref:System.Runtime.Caching.ObjectCache>. Класс <xref:System.Runtime.Caching.MemoryCache> можно использовать для большинства задач кэширования.  
  
> [!NOTE]
> Класс <xref:System.Runtime.Caching.MemoryCache> моделируется на основе объекта кэша ASP.NET, определенного в пространстве имен <xref:System.Web.Caching>. Следовательно, внутренняя логика кэширования подобна логике, предоставляемой в более ранних версиях ASP.NET.  
  
 Пример использования кэширования в приложении WPF см. в разделе [Пошаговое руководство. Кэширование данных приложения WPF](../wpf/advanced/walkthrough-caching-application-data-in-a-wpf-application.md).  
  
## <a name="caching-in-aspnet-applications"></a>Кэширование в приложениях ASP.NET  
 Классы кэширования в пространстве имен <xref:System.Runtime.Caching> предоставляют функциональные возможности кэширования данных в ASP.NET.  
  
> [!NOTE]
> Если приложение предназначено для .NET Framework 3,5 или более ранней версии, необходимо использовать классы кэширования, определенные в пространстве имен <xref:System.Web.Caching>. Дополнительные сведения см. в разделе [Общие сведения о кэшировании в ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/ms178597(v=vs.100)).  
  
> [!NOTE]
> При разработке новых приложений рекомендуется использовать класс <xref:System.Runtime.Caching.MemoryCache>. Интерфейс API, предоставленный в пространстве имен <xref:System.Runtime.Caching>, подобен API, предоставленному в пространстве имен <xref:System.Web.Caching.Cache>. Следовательно, этот API будет знаком вам, если вы использовали кэширование в более ранних версиях ASP.NET. Пример использования кэширования в приложениях ASP.NET см. в разделе [Пошаговое руководство. Кэширование данных приложения в ASP.NET](https://docs.microsoft.com/previous-versions/ff477235(v=vs.100)).  
  
### <a name="output-caching"></a>Кэширование выводимых данных  
 Для ручного кэширования данных приложения можно использовать класс <xref:System.Runtime.Caching.MemoryCache> в ASP.NET. ASP.NET также поддерживает кэширование выходных данных, при котором в памяти сохраняются созданные выходные данные страниц, элементов управления и HTTP-ответов. Кэширование выходных данных можно настроить декларативно на веб-странице ASP.NET или с помощью параметров в файле Web.config. Дополнительные сведения см. в разделе [Элемент outputCache для элемента caching (схема параметров ASP.NET)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms228124(v=vs.100)).  
  
 ASP.NET позволяет расширить кэширование выходных данных путем создания пользовательских поставщиков кэша выходных данных. С помощью пользовательских поставщиков можно хранить кэшированное содержимое на других запоминающих устройствах, таких как диски, облачные хранилища и распределенные модули кэширования. Для создания пользовательского поставщика кэша выходных данных необходимо создать класс, производный от класса <xref:System.Web.Caching.OutputCacheProvider>, и настроить для приложения использование пользовательского поставщика кэша выходных данных.  
  
## <a name="caching-in-wcf-rest-services"></a>Кэширование в службах WCF REST  
 Для служб WCF REST платформа .NET Framework позволяет использовать преимущества декларативного кэширования выходных данных, доступного в ASP.NET. Это дает возможность кэшировать ответы операций службы WCF REST. Когда пользователь отправляет запрос HTTP GET в службу, для которой настроено кэширование, ASP.NET отправляет обратно кэшированный ответ, а метод службы при этом не вызывается. По истечении срока действия кэша при следующей отправке пользователем запроса HTTP GET будет вызван метод службы, и ответ будет снова кэширован.  
  
 Платформа .NET Framework позволяет также реализовать условное кэширование HTTP GET. В сценариях REST условный HTTP-запрос GET часто используется службами для реализации интеллектуального HTTP-кэширования, которое описано в [спецификации протокола HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616.html). Дополнительные сведения см. в разделе [Поддержка кэширования для веб-служб HTTP WCF](../wcf/feature-details/caching-support-for-wcf-web-http-services.md).  
  
## <a name="extending-caching-in-the-net-framework"></a>Расширение кэширования в платформе .NET Framework  
 Кэширование в платформе .NET Framework предусматривает возможность расширения. Класс <xref:System.Runtime.Caching.ObjectCache> позволяет создавать пользовательскую реализацию кэша. Этот класс предоставляет члены, доступные всем управляемым приложениям, включая Windows Forms, Windows Presentation Foundation (WPF) и Windows Communications Foundation (WCF). С помощью пользовательской реализации можно создать класс кэша, использующий другой механизм хранения, или обеспечить точный контроль над операциями кэша.  
  
 Для расширения кэширования можно выполнить указанные ниже действия.  
  
- Создайте пользовательский класс, производный от класса <xref:System.Runtime.Caching.ObjectCache>, а затем предоставьте в нем пользовательскую реализацию кэша.  
  
- Создайте класс, производный от класса <xref:System.Runtime.Caching.MemoryCache>, и настройте или расширьте его. Пример того, как это сделать, см. в записи блога [Кэширование данных приложений с помощью нескольких объектов кэша в приложении ASP.NET](https://blogs.msdn.microsoft.com/aspnetue/2010/03/22/caching-application-data-by-using-multiple-cache-objects-in-an-asp-net-application/).  
  
- Создайте класс, производный от класса <xref:System.Web.Caching.OutputCacheProvider>, и настройте для приложения использование пользовательского поставщика кэша выходных данных.  
  
 Дополнительные сведения см. в записи [Расширенное кэширование выходных данных в ASP.NET 4 (серия, посвященная VS 2010 и .NET 4.0)](https://weblogs.asp.net/scottgu/extensible-output-caching-with-asp-net-4-vs-2010-and-net-4-0-series) блога Скотта Гатри (Scott Guthrie).  
  
## <a name="see-also"></a>См. также:

- <xref:System.Runtime.Caching.ObjectCache>
- <xref:System.Runtime.Caching.MemoryCache>
- [Пошаговое руководство. Кэширование данных приложения WPF](../wpf/advanced/walkthrough-caching-application-data-in-a-wpf-application.md)
- [Пошаговое руководство. Кэширование данных приложения в ASP.NET](https://docs.microsoft.com/previous-versions/ff477235(v=vs.100))
