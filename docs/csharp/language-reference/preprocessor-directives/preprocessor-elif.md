---
title: '#elif. Справочник по C#'
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- '#elif'
helpviewer_keywords:
- '#elif directive [C#]'
ms.assetid: 731d78df-08e0-4d51-b8c8-f193c27de13f
ms.openlocfilehash: b04db4bd23a459043efec59b8ebf9d322defbcf7
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69608582"
---
# <a name="elif-c-reference"></a>#elif (Справочник по C#)
Директива `#elif` позволяет создать составную условную директиву. Выражение `#elif` будет вычисляться в том случае, если ни одна из предшествующих директив [#if](./preprocessor-if.md) или необязательных директив `#elif` после вычисления выражения не возвращает значение `true`. Если после вычисления выражения `#elif` возвращается значение `true`, компилятор вычисляет весь код между директивой `#elif` и следующей условной директивой. Например:  
  
```csharp
#define VC7  
//...  
#if debug  
    Console.WriteLine("Debug build");  
#elif VC7  
    Console.WriteLine("Visual Studio 7");  
#endif  
```  
  
 Для вычисления нескольких символов можно использовать операторы `==` (равенство), `!=` (неравенство), `&&` (И) и `||` (ИЛИ). Можно также группировать символы и операторы при помощи скобок.  
  
## <a name="remarks"></a>Примечания  
 Применение `#elif` эквивалентно следующему:  
  
```csharp
#else  
#if  
```  
  
 Директива `#elif` позволяет упростить код, поскольку для каждой директивы `#if` требуется отдельная директива [#endif](./preprocessor-endif.md), тогда как `#elif` можно использовать без соответствующей директивы `#endif`.  
  
 В разделе [#if](./preprocessor-if.md) приводится пример использования директивы `#elif`.  
  
## <a name="see-also"></a>См. также

- [Справочник по C#](../index.md)
- [Руководство по программированию на C#](../../programming-guide/index.md)
- [Директивы препроцессора C#](./index.md)
