---
title: Consultar um XDocument versus Consultar um XElement
ms.date: 07/20/2015
ms.assetid: 2d111f84-0ded-4cde-8d93-5440557a726d
ms.openlocfilehash: 3cd79c8f2cde101de43a9b9e983709e2e0d11814
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396292"
---
# <a name="querying-an-xdocument-vs-querying-an-xelement-visual-basic"></a>Consultando um XDocument versus consultando um XElement (Visual Basic)
Ao carregar um documento por meio do <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType>, você observará que precisa escrever consultas um pouco diferentes do que ao carregar por meio do <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>.  
  
## <a name="comparison-of-xdocumentload-and-xelementload"></a>Comparação de XDocument.Load e de XElement.Load  
 Quando você carrega um documento XML em um <xref:System.Xml.Linq.XElement> por meio do <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>, o <xref:System.Xml.Linq.XElement> na raiz da árvore XML contém o elemento raiz do documento carregado. No entanto, quando você carrega o mesmo documento XML em um <xref:System.Xml.Linq.XDocument> por meio do <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType>, a raiz da árvore é um nó de <xref:System.Xml.Linq.XDocument>, e o elemento raiz do documento carregado é o nó filho <xref:System.Xml.Linq.XElement> permitido do <xref:System.Xml.Linq.XDocument>. Os eixos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] operam em relação ao nó raiz.  
  
 Este primeiro exemplo carrega uma árvore XML usando <xref:System.Xml.Linq.XElement.Load%2A>. Em seguida, ele consulta os elementos filho da raiz da árvore.  
  
```vb  
' Create a simple document and  write it to a file  
File.WriteAllText("Test.xml", "<Root>" + Environment.NewLine + _  
    "    <Child1>1</Child1>" + Environment.NewLine + _  
    "    <Child2>2</Child2>" + Environment.NewLine + _  
    "    <Child3>3</Child3>" + Environment.NewLine + _  
    "</Root>")  
  
Console.WriteLine("Querying tree loaded with XElement.Load")  
Console.WriteLine("----")  
Dim doc As XElement = XElement.Load("Test.xml")  
Dim childList As IEnumerable(Of XElement) = _  
        From el In doc.Elements() _  
        Select el  
For Each e As XElement In childList  
    Console.WriteLine(e)  
Next  
```  
  
 Como esperado, Este exemplo gera a seguinte saída:  
  
```console
Querying tree loaded with XElement.Load  
----  
<Child1>1</Child1>  
<Child2>2</Child2>  
<Child3>3</Child3>  
```  
  
 O exemplo a seguir é o mesmo que o anterior, com a exceção de que a árvore XML é carregada em um <xref:System.Xml.Linq.XDocument> em vez de em um <xref:System.Xml.Linq.XElement>.  
  
```vb  
' Create a simple document and  write it to a file  
File.WriteAllText("Test.xml", "<Root>" + Environment.NewLine + _  
    "    <Child1>1</Child1>" + Environment.NewLine + _  
    "    <Child2>2</Child2>" + Environment.NewLine + _  
    "    <Child3>3</Child3>" + Environment.NewLine + _  
    "</Root>")  
  
Console.WriteLine("Querying tree loaded with XDocument.Load")  
Console.WriteLine("----")  
Dim doc As XDocument = XDocument.Load("Test.xml")  
Dim childList As IEnumerable(Of XElement) = _  
        From el In doc.Elements() _  
        Select el  
For Each e As XElement In childList  
    Console.WriteLine(e)  
Next  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console
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
  
```vb  
' Create a simple document and  write it to a file  
File.WriteAllText("Test.xml", "<Root>" + Environment.NewLine + _  
    "    <Child1>1</Child1>" + Environment.NewLine + _  
    "    <Child2>2</Child2>" + Environment.NewLine + _  
    "    <Child3>3</Child3>" + Environment.NewLine + _  
    "</Root>")  
  
Console.WriteLine("Querying tree loaded with XDocument.Load")  
Console.WriteLine("----")  
Dim doc As XDocument = XDocument.Load("Test.xml")  
Dim childList As IEnumerable(Of XElement) = _  
        From el In doc.Root.Elements() _  
        Select el  
For Each e As XElement In childList  
    Console.WriteLine(e)  
Next  
```  
  
 Essa consulta agora executa da mesma maneira que a consulta na árvore enraizada em <xref:System.Xml.Linq.XElement>. O exemplo produz a seguinte saída:  
  
```console
Querying tree loaded with XDocument.Load  
----  
<Child1>1</Child1>  
<Child2>2</Child2>  
<Child3>3</Child3>  
```  
  
## <a name="see-also"></a>Confira também

- [Consultas básicas (LINQ to XML) (Visual Basic)](basic-queries-linq-to-xml.md)
