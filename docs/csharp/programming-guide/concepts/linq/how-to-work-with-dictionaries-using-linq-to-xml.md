---
title: Como trabalhar com dicionários usando LINQ to XML (C#)
description: Saiba como trabalhar com dicionários usando LINQ to XML. Veja exemplos convertendo dicionários em XML e XML de volta para outras estruturas de dados.
ms.date: 07/20/2015
ms.assetid: 57bcefe3-8433-4d3b-935a-511c9bcbdfa8
ms.openlocfilehash: bdba7a2b3dfc16fab1e239ac804c317dfefb7d9e
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302614"
---
# <a name="how-to-work-with-dictionaries-using-linq-to-xml-c"></a>Como trabalhar com dicionários usando LINQ to XML (C#)
É conveniente converter variedades de estruturas de dados para XML, e XML de volta para outras estruturas de dados. Este tópico mostra uma implementação específica dessa abordagem geral convertendo <xref:System.Collections.Generic.Dictionary%602> a XML e verso.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa um formulário de compilação funcional em que uma consulta projetos novos objetos de <xref:System.Xml.Linq.XElement> e a coleção resultante é passada como um argumento para o construtor do objeto de <xref:System.Xml.Linq.XElement> raiz.  
  
```csharp  
Dictionary<string, string> dict = new Dictionary<string, string>();  
dict.Add("Child1", "Value1");  
dict.Add("Child2", "Value2");  
dict.Add("Child3", "Value3");  
dict.Add("Child4", "Value4");  
XElement root = new XElement("Root",  
    from keyValue in dict  
    select new XElement(keyValue.Key, keyValue.Value)  
);  
Console.WriteLine(root);  
```  
  
 Este código produz a seguinte saída:  
  
```xml  
<Root>  
  <Child1>Value1</Child1>  
  <Child2>Value2</Child2>  
  <Child3>Value3</Child3>  
  <Child4>Value4</Child4>  
</Root>  
```  
  
## <a name="example"></a>Exemplo  
 O código a seguir cria um dicionário XML.  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("Child1", "Value1"),  
    new XElement("Child2", "Value2"),  
    new XElement("Child3", "Value3"),  
    new XElement("Child4", "Value4")  
);  
  
Dictionary<string, string> dict = new Dictionary<string, string>();  
foreach (XElement el in root.Elements())  
    dict.Add(el.Name.LocalName, el.Value);  
foreach (string str in dict.Keys)  
    Console.WriteLine("{0}:{1}", str, dict[str]);  
```  
  
 Este código produz a seguinte saída:  
  
```output  
Child1:Value1  
Child2:Value2  
Child3:Value3  
Child4:Value4  
```  
