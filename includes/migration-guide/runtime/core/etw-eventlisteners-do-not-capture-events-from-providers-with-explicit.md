---
ms.openlocfilehash: eab476a1d3f275e851e5af4198c30b60ad0c17b8
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67858839"
---
### <a name="etw-eventlisteners-do-not-capture-events-from-providers-with-explicit-keywords-like-the-tpl-provider"></a>EventListeners трассировки событий Windows не выполняют запись событий от поставщиков с явными ключевыми словами (например, от поставщика TPL)

|   |   |
|---|---|
|Подробные сведения|EventListeners трассировки событий Windows с пустой маской ключевого слова неправильно записывают события от поставщиков с явными ключевыми словами. В платформе .NET Framework 4.5 поставщик TPL начал предоставлять явные ключевые слова и привел к возникновению этой проблемы. В .NET Framework 4.6 EventListeners были обновлены, чтобы больше не вызывать эту проблему.|
|Предложение|Чтобы обойти эту проблему, замените вызовы <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)> вызовами к перегрузке EnableEvents, которая явно задает маску &quot;любые ключевые слова&quot;: <code>EnableEvents(eventSource, level, unchecked((EventKeywords)0xFFFFffffFFFFffff))</code>. Кроме того, эта проблема была устранена в .NET Framework 4.6 и может быть решена путем обновления до этой версии платформы .NET Framework.|
|Область|Пограничный случай|
|Версия|4.5|
|Тип|Среда выполнения|
|Затронутые API|<ul><li><xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)?displayProperty=nameWithType></li></ul>|

