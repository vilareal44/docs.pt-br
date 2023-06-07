---
title: Consultando um XDocument vs. consultando um XElement (C#)
description: Saiba mais sobre as diferenças entre consultar um XDocument e consultar um XElement. Examine os exemplos de código que demonstram essas diferenças.
ms.date: 07/20/2015
ms.assetid: 46221ff5-62ee-4de8-93ba-66465facb5c1
ms.openlocfilehash: 0c81768f06148308a639f96f4041e464b24edd33
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87300300"
---
# <a name="querying-an-xdocument-vs-querying-an-xelement-c"></a>Consultando um XDocument vs. consultando um XElement (C#)
Ao carregar um documento por meio do <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType>, você observará que precisa escrever consultas um pouco diferentes do que ao carregar por meio do <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>.  
  
## <a name="comparison-of-xdocumentload-and-xelementload"></a>Comparação de XDocument.Load e de XElement.Load  
 Quando você carrega um documento XML em um <xref:System.Xml.Linq.XElement> por meio do <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>, o <xref:System.Xml.Linq.XElement> na raiz da árvore XML contém o elemento raiz do documento carregado. No entanto, quando você carrega o mesmo documento XML em um <xref:System.Xml.Linq.XDocument> por meio do <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType>, a raiz da árvore é um nó de <xref:System.Xml.Linq.XDocument>, e o elemento raiz do documento carregado é o nó filho <xref:System.Xml.Linq.XElement> permitido do <xref:System.Xml.Linq.XDocument>. Os eixos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] operam em relação ao nó raiz.  
  
 Este primeiro exemplo carrega uma árvore XML usando <xref:System.Xml.Linq.XElement.Load%2A>. Em seguida, ele consulta os elementos filho da raiz da árvore.  
  
```csharp  
// Create a simple document and write it to a file  
File.WriteAllText("Test.xml", @"<Root>  
    <Child1>1</Child1>  
    <Child2>2</Child2>  
    <Child3>3</Child3>  
</Root>");  
  
Console.WriteLine("Querying tree loaded with XElement.Load");  
Console.WriteLine("----");  
XElement doc = XElement.Load("Test.xml");  
IEnumerable<XElement> childList =  
    from el in doc.Elements()  
    select el;  
foreach (XElement e in childList)  
    Console.WriteLine(e);  
```  
  
 Como esperado, Este exemplo gera a seguinte saída:  
  
```output  
Querying tree loaded with XElement.Load  
----  
<Child1>1</Child1>  
<Child2>2</Child2>  
<Child3>3</Child3>  
```  
  
 O exemplo a seguir é o mesmo que o anterior, com a exceção de que a árvore XML é carregada em um <xref:System.Xml.Linq.XDocument> em vez de em um <xref:System.Xml.Linq.XElement>.  
  
```csharp  
// Create a simple document and write it to a file  
File.WriteAllText("Test.xml", @"<Root>  
    <Child1>1</Child1>  
    <Child2>2</Child2>  
    <Child3>3</Child3>  
</Root>");  
  
Console.WriteLine("Querying tree loaded with XDocument.Load");  
Console.WriteLine("----");  
XDocument doc = XDocument.Load("Test.xml");  
IEnumerable<XElement> childList =  
    from el in doc.Elements()  
    select el;  
foreach (XElement e in childList)  
    Console.WriteLine(e);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
Querying tree loaded with XDocument.Load  
----  
<Root>  
  <Child1>1</Child1>  
  <Child2>2</Child2>  
  <Child3>3</Child3>  
</Root>  
```  
  
 Observe que a mesma consulta retornou o nó `Root` em vez dos três nós filho.  
  
 Uma abordagem para lidar com isso é usar a propriedade <xref:System.Xml.Linq.XDocument.Root%2A> antes de acessar os métodos de eixos, da seguinte maneira:  
  
```csharp  
// Create a simple document and write it to a file  
File.WriteAllText("Test.xml", @"<Root>  
    <Child1>1</Child1>  
    <Child2>2</Child2>  
    <Child3>3</Child3>  
</Root>");  
  
Console.WriteLine("Querying tree loaded with XDocument.Load");  
Console.WriteLine("----");  
XDocument doc = XDocument.Load("Test.xml");  
IEnumerable<XElement> childList =  
    from el in doc.Root.Elements()  
    select el;  
foreach (XElement e in childList)  
    Console.WriteLine(e);  
```  
  
 Essa consulta agora executa da mesma maneira que a consulta na árvore enraizada em <xref:System.Xml.Linq.XElement>. O exemplo produz a seguinte saída:  
  
```output  
Querying tree loaded with XDocument.Load  
----  
<Child1>1</Child1>  
<Child2>2</Child2>  
<Child3>3</Child3>  
```  
  