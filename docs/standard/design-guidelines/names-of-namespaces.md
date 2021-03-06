---
title: Имена пространств имен.
ms.date: 10/22/2008
helpviewer_keywords:
- names [.NET Framework], conflicts
- names [.NET Framework], namespaces
- type names, conflicts
- namespaces [.NET Framework], names
- names [.NET Framework], type names
ms.assetid: a49058d2-0276-43a7-9502-04adddf857b2
ms.openlocfilehash: 0678f695e3ae7c40660031862c9073a21fd72491
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709235"
---
# <a name="names-of-namespaces"></a>Имена пространств имен.
Как и в случае с другими рекомендациями по именованию, цель при именовании пространств имен заключается в создании достаточной ясности для программиста, использующего платформу для немедленного знания содержимого пространства имен. Следующий шаблон задает общее правило для именования пространств имен.  
  
 `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`  
  
 Примеры:  
  
 `Fabrikam.Math`  
 `Litware.Security`  
  
 **✓ DO** префикса пространства имен названия название компании, чтобы предотвратить пространства имен из разных компаний с одинаковым именем.  
  
 **✓ DO** название стабильной, независимые от версии продукта, используемое на втором уровне пространства имен.  
  
 **X DO NOT** использовать организационные иерархии как основу для имен в иерархии пространств имен, так как имена групп в организации, как правило краткосрочны. Упорядочите иерархию пространств имен вокруг групп связанных технологий.  
  
 **✓ DO** использовать PascalCasing и компонентов в отдельном пространстве имен с точками (например, `Microsoft.Office.PowerPoint`). Если ваша торговая марка использует нетрадиционный регистр, следует соблюдать регистр, заданный в торговой марке, даже если он отличается от обычного регистра пространства имен.  
  
 **✓ CONSIDER** с помощью имена во множественном числе пространств имен, где это применимо.  
  
 Например, используйте `System.Collections` вместо `System.Collection`. Однако названия и акронимы торговых марок являются исключениями из этого правила. Например, используйте `System.IO` вместо `System.IOs`.  
  
 **X DO NOT** использовать то же имя для пространства имен и типа в этом пространстве имен.  
  
 Например, не используйте `Debug` в качестве имени пространства имен, а затем предоставьте класс с именем `Debug` в том же пространстве имен. Для некоторых компиляторов такие типы должны быть полными.  
  
### <a name="namespaces-and-type-name-conflicts"></a>Конфликты пространств имен и имен типов  
 **X DO NOT** ввести имена универсальных типов, таких как `Element`, `Node`, `Log`, и `Message`.  
  
 Очень высокая вероятность того, что это приводит к конфликтам имен типов в распространенных сценариях. Необходимо уточнить имена универсальных типов (`FormElement`, `XmlNode`, `EventLog`, `SoapMessage`).  
  
 Существуют определенные рекомендации по предотвращению конфликтов имен типов для различных категорий пространств имен.  
  
- **Пространства имен модели приложения**  
  
     Пространства имен, принадлежащие одной модели приложения, часто используются вместе, но они практически никогда не используются с пространствами имен других моделей приложений. Например, пространство имен <xref:System.Windows.Forms?displayProperty=nameWithType> редко используется совместно с пространством имен <xref:System.Web.UI?displayProperty=nameWithType>. Ниже приведен список хорошо известных групп пространств имен модели приложений.  
  
     `System.Windows*`   
     `System.Web.UI*`  
  
     **X DO NOT** дать одинаковые имена, чтобы типы в пространствах имен в пределах одной модели приложения.  
  
     Например, не добавляйте тип с именем `Page` в пространство имен <xref:System.Web.UI.Adapters?displayProperty=nameWithType>, так как пространство имен <xref:System.Web.UI?displayProperty=nameWithType> уже содержит тип с именем `Page`.  
  
- **Пространства имен инфраструктуры**  
  
     Эта группа содержит пространства имен, которые редко импортируются во время разработки общих приложений. Например, `.Design` пространства имен используются главным образом при разработке средств программирования. Предотвращение конфликтов с типами в этих пространствах имен не является критически важным.  
  
- **Основные пространства имен**  
  
     К основным пространствам имен относятся все пространства имен `System`, за исключением пространств имен моделей приложений и пространств имен инфраструктуры. К основным пространствам имен относятся, среди прочих, `System`, `System.IO`, `System.Xml`и `System.Net`.  
  
     **X DO NOT** предоставляют типы имен, которые могли бы конфликтовать с любым типом из основных пространств имен.  
  
     Например, никогда не используйте `Stream` в качестве имени типа. Это приведет к конфликту с <xref:System.IO.Stream?displayProperty=nameWithType>, очень часто используемым типом.  
  
- **Группы пространств имен технологий**  
  
     Эта категория включает все пространства имен с одними и теми же первыми двумя узлами пространства имен `(<Company>.<Technology>*`), например `Microsoft.Build.Utilities` и `Microsoft.Build.Tasks`. Важно, чтобы типы, принадлежащие одной технологии, не конфликтовали друг с другом.  
  
     **X DO NOT** назначить имена типов, которые могли бы конфликтовать с другими типами внутри одной технологии.  
  
     **X DO NOT** вводит конфликтов имен типов между типами из пространств имен технологии и пространстве имен модели приложения (если компонент не предназначен для использования с моделью приложения).  
  
 *Части © 2005, 2009 Корпорация Майкрософт. Все права защищены.*  
  
 *Перепечатано с разрешения Pearson Education, Inc. из книги [Инфраструктура программных проектов. Соглашения, идиомы и шаблоны для многократно используемых библиотек .NET (2-е издание)](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619), авторы: Кржиштоф Цвалина (Krzysztof Cwalina) и Брэд Абрамс (Brad Abrams). Книга опубликована 22 октября 2008 г. издательством Addison-Wesley Professional в рамках серии, посвященной разработке для Microsoft Windows.*  
  
## <a name="see-also"></a>См. также:

- [Рекомендации по проектированию на основе Framework](../../../docs/standard/design-guidelines/index.md)
- [Правила именования](../../../docs/standard/design-guidelines/naming-guidelines.md)
