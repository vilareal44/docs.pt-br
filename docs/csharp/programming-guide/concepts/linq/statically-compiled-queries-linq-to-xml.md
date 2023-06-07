---
title: Consultas estaticamente compiladas (LINQ to XML) (C#)
description: Saiba mais sobre consultas compiladas estaticamente em LINQ to XML em C# e como elas diferem das consultas XPath, que devem ser interpretadas em tempo de execução.
ms.date: 07/20/2015
ms.assetid: 3bf558fe-0705-479d-86d4-00188f5fcf9c
ms.openlocfilehash: cd2e6a6507311d5fc17215a22c70bd0449292b6f
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302302"
---
# <a name="statically-compiled-queries-linq-to-xml-c"></a>Consultas estaticamente compiladas (LINQ to XML) (C#)
Um de desempenho mais importante beneficia LINQ to XML, diferentemente de <xref:System.Xml.XmlDocument>, é que as consultas em LINQ to XML são compiladas estaticamente, enquanto as consultas XPath devem ser interpretado em tempo de execução. Esse recurso é interna a LINQ to XML, portanto você não precisa executar etapas adicionais para aproveitá-lo, mas é útil entender a diferença ao escolher entre as duas tecnologias. Este tópico explica a diferença.  
  
## <a name="statically-compiled-queries-vs-xpath"></a>Consultas compiladas estaticamente versus XPath  
 O exemplo a seguir mostra como obter os elementos descendentes com um nome especificado e, com um atributo com um valor especificado.  
  
 A seguir está a expressão XPath equivalente:`//Address[@Type='Shipping']`
  
```csharp  
XDocument po = XDocument.Load("PurchaseOrders.xml");  
  
IEnumerable<XElement> list1 =  
    from el in po.Descendants("Address")  
    where (string)el.Attribute("Type") == "Shipping"  
    select el;  
  
foreach (XElement el in list1)  
    Console.WriteLine(el);  
```  
  
 A expressão de consulta nesse exemplo é reescrita pelo compilador a sintaxe da consulta com base em método. O exemplo a seguir, que é escrito na sintaxe da consulta com base em método, gerenciar os mesmos resultados que anterior:  
  
```csharp  
XDocument po = XDocument.Load("PurchaseOrders.xml");  
  
IEnumerable<XElement> list1 =  
    po  
    .Descendants("Address")  
    .Where(el => (string)el.Attribute("Type") == "Shipping");  
  
foreach (XElement el in list1)  
    Console.WriteLine(el);  
```  
  
 O método de <xref:System.Linq.Enumerable.Where%2A> é um método de extensão. Para obter mais informações, consulte [Métodos de extensão](../../classes-and-structs/extension-methods.md). Porque <xref:System.Linq.Enumerable.Where%2A> é um método de extensão, a consulta anterior é criada como se ele foi gravado como segue:  
  
```csharp  
XDocument po = XDocument.Load("PurchaseOrders.xml");  
  
IEnumerable<XElement> list1 =  
    System.Linq.Enumerable.Where(  
        po.Descendants("Address"),  
        el => (string)el.Attribute("Type") == "Shipping");  
  
foreach (XElement el in list1)  
    Console.WriteLine(el);  
```  
  
 Este exemplo gerencia exatamente os mesmos resultados que os dois exemplos anteriores. Isso ilustra o fato de que as consultas são compiladas efetivamente estaticamente associados em chamadas de método. Isso, combinado com a semântica de execução adiada de iteradores, melhora o desempenho. Para obter mais informações sobre a semântica de execução adiada dos iteradores, consulte [Execução adiada e avaliação lenta em LINQ to XML (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md).  
  
> [!NOTE]
> Esses exemplos são representante de código que o compilador pode escrever. A implementação real pode diferir um pouco desses exemplos, mas o desempenho será o mesmo ou semelhante a esses exemplos.  
  
## <a name="executing-xpath-expressions-with-xmldocument"></a>Executando com expressões XPath XmlDocument  
 O exemplo a seguir usa <xref:System.Xml.XmlDocument> para executar os mesmos resultados que os exemplos anteriores:  
  
```csharp  
XmlReader reader = XmlReader.Create("PurchaseOrders.xml");  
XmlDocument doc = new XmlDocument();  
doc.Load(reader);  
XmlNodeList nl = doc.SelectNodes(".//Address[@Type='Shipping']");  
foreach (XmlNode n in nl)  
    Console.WriteLine(n.OuterXml);  
reader.Close();  
```  
  
 Esta consulta retorna as mesmas saída que os exemplos que usam LINQ to XML; a única diferença é que LINQ to XML recua XML impresso, enquanto <xref:System.Xml.XmlDocument> não.  
  
 No entanto, a abordagem de <xref:System.Xml.XmlDocument> geralmente não executa bem como LINQ to XML, porque o método de <xref:System.Xml.XmlNode.SelectNodes%2A> deve fazer o seguinte internamente todas as vezes nele é chamado:  
  
- Analisa a cadeia de caracteres que contém a expressão XPath, quebrando a cadeia de caracteres em tokens.  
  
- Valida os tokens para certificar-se de que a expressão XPath é válido.  
  
- Converte a expressão em uma árvore de expressão interna.  
  
- Itera através de nós, selecionando adequadamente os nós para o conjunto de resultados com base na avaliação da expressão.  
  
 Este é significativamente mais do que o trabalho feito pela consulta correspondente LINQ to XML. A diferença desempenho específico variam para tipos diferentes de consultas, mas em geral LINQ to XML consultas menos trabalho e, portanto, executam melhor do que as expressões XPath de avaliação usando <xref:System.Xml.XmlDocument>.  
