---
title: Запись сведений о схеме набора данных как XSD
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 4e530831-695e-49ff-8f0b-e5b0c526b8eb
ms.openlocfilehash: f86e9100489ddf35d8ef5f98e386306a7dbfd4ed
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70784180"
---
# <a name="writing-dataset-schema-information-as-xsd"></a>Запись сведений о схеме набора данных как XSD
Можно записать схему набора данных <xref:System.Data.DataSet> в виде схемы на языке XSD, чтобы можно было передать ее в XML-документ с взаимосвязанными данными или без них. XML-схема может быть записана в файл, в поток, <xref:System.Xml.XmlWriter>в или строку; она полезна для создания строго типизированного **набора данных**. Дополнительные сведения о строго типизированных объектах DataSet см. в разделе [типизированные наборы](typed-datasets.md) **данных** .  
  
 Можно указать способ представления столбца таблицы в XML-схеме с помощью свойства <xref:System.Data.DataColumn> **ColumnMapping** объекта. Дополнительные сведения см. в разделе «Сопоставление столбцов с XML-элементами, атрибутами и текстом» [статьи запись содержимого набора данных в виде XML-данных](writing-dataset-contents-as-xml-data.md).  
  
 Для записи схемы **набора данных** в виде XML-схемы в файл, поток или **XmlWriter**используйте метод **вритексмлсчема** **набора данных**. **Вритексмлсчема** принимает один параметр, который задает назначение результирующей схемы XML. В следующих примерах кода показано, как записать XML-схему **набора данных** в файл, передав строку, содержащую имя файла и <xref:System.IO.StreamWriter> объект.  
  
```vb  
dataSet.WriteXmlSchema("Customers.xsd")  
```  
  
```csharp  
dataSet.WriteXmlSchema("Customers.xsd");  
```  
  
```vb  
Dim writer As System.IO.StreamWriter = New System.IO.StreamWriter("Customers.xsd")  
dataSet.WriteXmlSchema(writer)  
writer.Close()  
```  
  
```csharp  
System.IO.StreamWriter writer = new System.IO.StreamWriter("Customers.xsd");  
dataSet.WriteXmlSchema(writer);  
writer.Close();  
```  
  
 Чтобы получить схему **набора данных** и записать ее в виде строки XML-схемы, **используйте метод GetString** , как показано в следующем примере.  
  
```vb  
Dim schemaString As String = dataSet.GetXmlSchema()  
```  
  
```csharp  
string schemaString = dataSet.GetXmlSchema();  
```  
  
## <a name="see-also"></a>См. также

- [Использование XML в наборах данных](using-xml-in-a-dataset.md)
- [Запись содержимого DataSet как данных XML](writing-dataset-contents-as-xml-data.md)
- [Типизированные наборы данных](typed-datasets.md)
- [Наборы данных, таблицы данных и объекты DataView](index.md)
- [Общие сведения об ADO.NET](../ado-net-overview.md)
