---
title: Миграция веб-служб ASP.NET на платформу WCF
ms.date: 03/30/2017
ms.assetid: 1adbb931-f0b1-47f3-9caf-169e4edc9907
ms.openlocfilehash: 52e0e499b5338e20377c14b598c045a5173df7d3
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69965340"
---
# <a name="migrating-aspnet-web-services-to-wcf"></a>Миграция веб-служб ASP.NET на платформу WCF
Платформа ASP.NET предоставляет библиотеки классов .NET Framework и средства для построения веб-служб, а также возможности для их размещения в службах Internet Information Services (IIS). Windows Communication Foundation (WCF) предоставляет библиотеки классов .NET Framework, средства и средства размещения для взаимодействия программных сущностей с помощью любых протоколов, включая те, которые используются веб-службами.  Миграция веб-служб ASP.NET в WCF позволяет приложениям использовать преимущества новых функций и улучшений, уникальных для WCF.  
  
 WCF имеет несколько важных преимуществ относительно веб-служб ASP.NET. Хотя средства веб-служб ASP.NET предназначены исключительно для создания веб-служб, WCF предоставляет средства, которые можно использовать, когда программные сущности должны быть связаны друг с другом. Это позволяет уменьшить число технологий, в которых должен разбираться разработчик, чтобы иметь возможность реализовывать различные сценарии взаимодействия между компонентами программного обеспечения, что, в свою очередь, сокращает стоимость необходимых для разработки ресурсов, а также время реализации проектов по разработке программного обеспечения.  
  
 Даже для проектов разработки веб-служб WCF поддерживает больше протоколов веб-служб, чем ASP.NET поддержка веб-служб. Эти дополнительные протоколы позволяют реализовывать более сложные решения, в том числе решения, использующие надежные сеансы и транзакции.  
  
 WCF поддерживает больше протоколов для передачи сообщений, чем веб-службы ASP.NET. Веб-службы ASP.NET поддерживают только отправку сообщений с помощью протокола передачи гипертекста (HTTP). WCF поддерживает отправку сообщений с помощью протокола HTTP, а также протокол TCP, именованные каналы и очередь сообщений Майкрософт (MSMQ). Более важно, что WCF можно расширить для поддержки дополнительных транспортных протоколов. Таким образом, программное обеспечение, разработанное с помощью WCF, можно адаптировать для совместной работы с более широким спектром других программных продуктов, тем самым увеличивая вероятность возврата инвестиций.  
  
 WCF предоставляет широкие возможности для развертывания приложений и управления ими, чем предоставляемые веб-службы ASP.NET. Помимо системы конфигурации, которая также имеет ASP.NET, WCF предлагает редактор конфигурации, трассировку действий от отправителей к получателям и обратно через любое количество посредников, средство просмотра трассировки, ведение журнала сообщений, большое количество счетчиков производительности и Поддержка инструментарий управления Windows (WMI).  
  
 Учитывая эти потенциальные преимущества WCF относительно веб-служб ASP.NET, если вы используете или планируете использовать веб-службы ASP.NET, у вас есть несколько вариантов:  
  
- Продолжайте использовать веб-службы ASP.NET и в вышеизложенном предложеннойе по WCF.  
  
- Используйте веб-службы ASP.NET с намерением внедрения WCF в будущем. В подразделах этого раздела объясняется, как максимально увеличить количество перспективных клиентов, чтобы они могли использовать новые приложения веб-службы ASP.NET вместе с будущими приложениями WCF. В подразделах этого раздела также объясняется, как создать новые веб-службы ASP.NET, чтобы упростить их миграцию в WCF. Тем не менее, если защита служб важна, или требуются гарантии надежности или транзакций, или если необходимо создать пользовательские средства управления, лучше использовать WCF. WCF предназначен для точного использования в таких сценариях.  
  
- Внедрять WCF для новой разработки, продолжая обслуживать существующие приложения веб-службы ASP.NET. Этот вариант с большой вероятностью может быть оптимальным. Это дает преимущества WCF, сохраняя затраты на изменение существующих приложений для его использования. В этом сценарии новые приложения WCF могут сосуществовать с существующими ASP.NET приложениями. Новые приложения WCF смогут использовать существующие веб-службы ASP.NET, и WCF можно использовать для программирования новых операционных возможностей в существующих ASP.NET приложениях с помощью режима совместимости WCF ASP.NET.  
  
- Внедрять WCF и переносить существующие приложения веб-службы ASP.NET в WCF. Этот вариант можно выбрать для улучшения существующих приложений с помощью функций, предоставляемых WCF, или для воспроизведения функциональных возможностей существующих веб-служб ASP.NET в новых, более эффективных приложениях WCF.  
  
> [!NOTE]
> Следует соблюдать осторожность при удалении службы WCF, размещенной в IIS 5. x и ASP.NET. Если служба WCF размещена в IIS 5. x, код службы может быть запрошен при удалении ASP.NET. При удалении ASP.NET в операционной системе, где запущены службы IIS 5. x и WCF, файл с расширением SVC считается текстовым файлом, а содержимое, включая любой исходный код, возвращается инициатору запроса.  
  
 В этом разделе подробно описываются эти возможности, сравниваются веб-службы ASP.NET с WCF и приводятся инструкции по переносу кода веб-служб ASP.NET в WCF.  
  
## <a name="see-also"></a>См. также

- [Предполагается принятие Windows Communication Foundation: Ускорение миграции в будущем](../../../../docs/framework/wcf/feature-details/anticipating-adopting-wcf-migration.md)
- [Предполагается принятие Windows Communication Foundation: Ускорение интеграции в будущем](../../../../docs/framework/wcf/feature-details/anticipating-adopting-the-wcf-easing-future-integration.md)
- [Переход на платформу Windows Communication Foundation](../../../../docs/framework/wcf/feature-details/adopting-wcf.md)
- [Сравнение веб-служб ASP.NET с веб-службами на основе WCF по назначению и используемым стандартам](../../../../docs/framework/wcf/feature-details/comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used.md)
- [Сравнение веб-служб ASP.NET с веб-службами на основе WCF по процессу разработки](../../../../docs/framework/wcf/feature-details/comparing-aspnet-web-services-to-wcf-based-on-development.md)
