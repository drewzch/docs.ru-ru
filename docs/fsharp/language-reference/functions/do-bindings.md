---
title: Привязки do
description: Узнайте, как F# привязка do используется для выполнения кода без определения функции или значения.
ms.date: 05/16/2016
ms.openlocfilehash: f98f523296bfaceeda35d4861eafbfeaa5a60c32
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630542"
---
# <a name="do-bindings"></a>Привязки do

Привязка `do` используется для выполнения кода без определения функции или значения. Кроме того, привязки `do` можно использовать в классах см [привязки `do` в классах](../members/do-bindings-in-classes.md).

## <a name="syntax"></a>Синтаксис

```fsharp
[ attributes ]
[ do ]expression
```

## <a name="remarks"></a>Примечания

Используйте привязки `do`, если требуется выполнить код независимо от определения функции или значения. Выражение в привязке `do` должно возвращать значение `unit`. Код в привязке `do` верхнего уровня выполняется при инициализации модуля. Ключевое слово `do` является необязательным.

Атрибуты могут быть применены к привязке `do` верхнего уровня. Например, если программа использует COM-взаимодействия, может потребоваться применить в программе атрибут `STAThread`. Это можно сделать с помощью атрибута привязки `do`, как показано в следующем коде.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet201.fs)]

## <a name="see-also"></a>См. также

- [Справочник по языку F#](../index.md)
- [Функции](index.md)
