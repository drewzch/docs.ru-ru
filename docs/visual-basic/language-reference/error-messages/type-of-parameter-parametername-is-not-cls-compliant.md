---
title: Тип параметра <parametername> не является CLS-совместимым
ms.date: 07/20/2015
f1_keywords:
- vbc40028
- bc40028
helpviewer_keywords:
- BC40028
ms.assetid: dfa1f6f9-bb88-44ad-b85f-149144363d41
ms.openlocfilehash: 1f671b75972642670e28b9e761a8174d02d39c4e
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/15/2019
ms.locfileid: "65642179"
---
# <a name="type-of-parameter-parametername-is-not-cls-compliant"></a>Тип параметра "\<имя_параметра >" не является CLS-совместимым
Процедура помечена как `<CLSCompliant(True)>` , но объявляет параметр с типом, который помечен как `<CLSCompliant(False)>`, не помечен или не определен, так как он является несовместимым типом.  
  
 Для соответствия требованиям, описанным в статье [Независимость от языка и независимые от языка компоненты](../../../standard/language-independence-and-language-independent-components.md) (CLS), процедура должна использовать только типы, соответствующие CLS. Это касается типов параметров, типа возвращаемого значения и типов всех локальных переменных.  
  
 Следующие типы данных Visual Basic не являются CLS-совместимыми:  
  
- [Тип данных SByte](../../../visual-basic/language-reference/data-types/sbyte-data-type.md)  
  
- [Тип данных UInteger](../../../visual-basic/language-reference/data-types/uinteger-data-type.md)  
  
- [Тип данных ULong](../../../visual-basic/language-reference/data-types/ulong-data-type.md)  
  
- [Тип данных UShort](../../../visual-basic/language-reference/data-types/ushort-data-type.md)  
  
 При применении атрибута <xref:System.CLSCompliantAttribute> к программному элементу вы задаете для параметра `isCompliant` атрибута значение `True` или `False` , чтобы указать совместимость или несовместимость. Для этого параметра нет значения по умолчанию, и вы должны предоставить значение.  
  
 Если вы не применяете <xref:System.CLSCompliantAttribute> к элементу, он считается несовместимым.  
  
 По умолчанию данное сообщение является предупреждением. Сведения о сокрытии предупреждений или обработке предупреждений как ошибок см. в разделе [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **Идентификатор ошибки:** BC40028  
  
## <a name="to-correct-this-error"></a>Исправление ошибки  
  
- Если процедура должна принимать параметр этого конкретного типа, удалите <xref:System.CLSCompliantAttribute>. Процедура не может соответствовать CLS.  
  
- Если процедура должна быть CLS-совместимым, измените тип этого параметра на ближайший CLS-совместимый тип. Например, вместо `UInteger` вы можете использовать `Integer` , если вам не нужен диапазон значений, превышающий 2 147 483 647. Если вам нужен расширенный диапазон, вы можете заменить `UInteger` на `Long`.  
  
- При взаимодействии с автоматизация или COM-объектами, имейте в виду, что некоторые типы имеют разные данные от длины в .NET Framework. Например, данные типа `int` часто являются 16-битными в других средах. Если вы принимаете 16-разрядное целое число из таких компонентов, объявите его как `Short` вместо `Integer` в управляемом коде Visual Basic.
