---
title: Практическое руководство. Привязка элементов управления Windows Forms к значениям DBNull
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BindingSource component [Windows Forms], binding to DBNull values
- examples [Windows Forms], BindingSource component
- controls [Windows Forms], binding to DBNull values
ms.assetid: 96494e6f-5f40-4f83-af97-bbd7192c2af8
ms.openlocfilehash: 8abce5d69a7cece6528e0c9d8728de0e0acd9305
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65591424"
---
# <a name="how-to-bind-windows-forms-controls-to-dbnull-database-values"></a>Практическое руководство. Привязка элементов управления Windows Forms к значениям DBNull
Если элементы управления Windows Forms привязаны к источнику данных и источник данных возвращает значение <xref:System.DBNull>, соответствующее значение можно заменить без обработки, форматирования или анализа событий. Свойство <xref:System.Windows.Forms.Binding.NullValue%2A> преобразует <xref:System.DBNull> в указанный объект при форматировании или анализе значений источника данных.  
  
## <a name="example"></a>Пример  
 В примере ниже демонстрируется привязка значения <xref:System.DBNull> в двух разных случаях. В первом случае показано, как задать <xref:System.Windows.Forms.Binding.NullValue%2A> для свойства строки; во втором — как задать <xref:System.Windows.Forms.Binding.NullValue%2A> для свойства изображения.  
  
 [!code-csharp[System.Windows.Forms.BindingDBNull#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BindingDBNull/CS/form1.cs#1)]
 [!code-vb[System.Windows.Forms.BindingDBNull#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BindingDBNull/VB/form1.vb#1)]  
  
 Типы связанного свойства и свойства <xref:System.Windows.Forms.Binding.NullValue%2A> должны быть одинаковыми, иначе произойдет ошибка и следующие значения <xref:System.Windows.Forms.Binding.NullValue%2A> обрабатываться не будут. В этом случае исключение не создается.  
  
## <a name="compiling-the-code"></a>Компиляция кода  
 Для этого примера требуются:  
  
- ссылки на сборки System, System.Data, System.Drawing и System.Windows.Forms.  
  
## <a name="see-also"></a>См. также

- [Компонент BindingSource](bindingsource-component.md)
- [Практическое руководство. Обработка ошибок и исключений, происходящих при выполнении привязки данных](how-to-handle-errors-and-exceptions-that-occur-with-databinding.md)
- [Практическое руководство. Привязка элемента управления Windows Forms к типу](how-to-bind-a-windows-forms-control-to-a-type.md)
