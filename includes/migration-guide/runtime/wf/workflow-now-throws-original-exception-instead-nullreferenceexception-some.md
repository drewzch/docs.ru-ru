---
ms.openlocfilehash: 6e5e90a4cfb862b3bfd74ac5a3715e97a736f598
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67802538"
---
### <a name="workflow-now-throws-original-exception-instead-of-nullreferenceexception-in-some-cases"></a>Рабочий процесс теперь создает исходное исключение вместо NullReferenceException, которое создавалось в некоторых случаях

|   |   |
|---|---|
|Подробные сведения|В .NET Framework 4.6.2 и более ранних версий в тех случаях, когда метод Execute действия рабочего процесса создает исключение со значением <code>null</code> для свойства <xref:System.Exception.Message>, среда выполнения рабочего процесса System.Activities создает исключение <xref:System.NullReferenceException?displayProperty=name>, маскируя исходное исключение. В версии .NET Framework 4.7 создается ранее маскированное исключение.|
|Предложение|Если в вашем коде реализуется обработка исключения <xref:System.NullReferenceException?displayProperty=name>, измените его для перехвата исключений, которые могут создаваться настраиваемыми действиями.|
|Область|Дополнительный номер|
|Версия|4.7|
|Тип|Среда выполнения|
|Затронутые API|<ul><li><xref:System.Activities.CodeActivity.Execute(System.Activities.CodeActivityContext)?displayProperty=nameWithType></li><li><xref:System.Activities.AsyncCodeActivity.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)?displayProperty=nameWithType></li><li><xref:System.Activities.AsyncCodeActivity%601.BeginExecute(System.Activities.AsyncCodeActivityContext,System.AsyncCallback,System.Object)?displayProperty=nameWithType></li><li><xref:System.Activities.WorkflowInvoker.Invoke?displayProperty=nameWithType></li></ul>|

