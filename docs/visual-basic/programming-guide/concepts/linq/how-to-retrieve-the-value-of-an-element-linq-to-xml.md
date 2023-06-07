---
title: 'Como: recuperar o valor de um elemento (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 76b9b2a5-b3ba-49da-ba74-82100e1bd21c
ms.openlocfilehash: b8ff44416f0fbb590119af7e11714e449185d977
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397785"
---
# <a name="how-to-retrieve-the-value-of-an-element-linq-to-xml-visual-basic"></a>Como recuperar o valor de um elemento (LINQ to XML) (Visual Basic)
Este tópico mostra como obter o valor de elementos. Há duas maneiras principais de fazer isso. Uma maneira é converter <xref:System.Xml.Linq.XElement> ou <xref:System.Xml.Linq.XAttribute> para o tipo desejado. O operador de conversão explícita converte o conteúdo do elemento ou do atributo no tipo especificado e o atribui à sua variável. Outra opção é usar a propriedade <xref:System.Xml.Linq.XElement.Value%2A?displayProperty=nameWithType> ou a propriedade <xref:System.Xml.Linq.XAttribute.Value%2A?displayProperty=nameWithType>.  
  
 Com Visual Basic, a melhor abordagem é usar a propriedade <xref:System.Xml.Linq.XElement.Value%2A?displayProperty=nameWithType>.  
  
## <a name="example"></a>Exemplo  
 Para recuperar o valor de um elemento, basta converter o objeto <xref:System.Xml.Linq.XElement> no tipo desejado. Você sempre pode converter um elemento em uma cadeia de caracteres, como a seguir:  
  
```vb  
Dim e As XElement = <StringElement>abcde</StringElement>  
Console.WriteLine(e)  
Console.WriteLine("Value of e:" & e.Value)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<StringElement>abcde</StringElement>  
Value of e:abcde  
```  
  
## <a name="example"></a>Exemplo  
 Você também pode converter elementos em tipos que não sejam cadeias de caracteres. Por exemplo, se você tiver um elemento que contenha um número inteiro, poderá convertê-lo em `int`, como mostrado no código a seguir:  
  
```vb  
Dim e As XElement = <Age>44</Age>  
Console.WriteLine(e)  
Console.WriteLine("Value of e:" & CInt(e))  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Age>44</Age>  
Value of e:44  
```  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] fornece operadores de conversão explícita para os seguintes tipos de dados: `string`, `bool`, `bool?`, `int`, `int?`, `uint`, `uint?`, `long`, `long?`, `ulong`, `ulong?`, `float`, `float?`, `double`, `double?`, `decimal`, `decimal?`, `DateTime`, `DateTime?`, `TimeSpan`, `TimeSpan?`, `GUID` e `GUID?`.  
  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] fornece os mesmos operadores de conversão para objetos <xref:System.Xml.Linq.XAttribute>.  
  
## <a name="example"></a>Exemplo  
 É possível usar a propriedade <xref:System.Xml.Linq.XElement.Value%2A> para recuperar o conteúdo de um elemento:  
  
```vb  
Dim e As XElement = <StringElement>abcde</StringElement>  
Console.WriteLine(e)  
Console.WriteLine("Value of e:" & e.Value)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<StringElement>abcde</StringElement>  
Value of e:abcde  
```  
  
## <a name="example"></a>Exemplo  
 Às vezes, você tenta recuperar o valor de um elemento mesmo quando não tem certeza de que ele existe. Nesse caso, quando você atribui o elemento convertido a um tipo anulável ( `string` ou um dos tipos de valor anulável no .NET Framework), se o elemento não existir, a variável atribuída será definida apenas como `Nothing` . O código a seguir mostra que quando o elemento pode ou não existir, é mais fácil de usar a conversão do que usar a propriedade <xref:System.Xml.Linq.XElement.Value%2A>.  
  
```vb  
Dim root As XElement = <Root>  
                           <Child1>child 1 content</Child1>  
                           <Child2>2</Child2>  
                       </Root>  
  
' The following assignments show why it is easier to use  
' casting when the element might or might not exist.  
  
Dim c1 As String = CStr(root.Element("Child1"))  
Console.WriteLine("c1:{0}", IIf(c1 Is Nothing, "element does not exist", c1))  
  
Dim c2 As Nullable(Of Integer) = CType(root.Element("Child2"), Nullable(Of Integer))  
Console.WriteLine("c2:{0}", IIf(Not (c2.HasValue), "element does not exist", c2.ToString()))  
  
Dim c3 As String = CStr(root.Element("Child3"))  
Console.WriteLine("c3:{0}", IIf(c3 Is Nothing, "element does not exist", c3))  
  
Dim c4 As Nullable(Of Integer) = CType(root.Element("Child4"), Nullable(Of Integer))  
Console.WriteLine("c4:{0}", IIf(Not (c4.HasValue), "element does not exist", c4.ToString()))  
  
Console.WriteLine()  
  
' The following assignments show the required code when using  
' the Value property when the attribute might or might not exist.  
' Notice that this is more difficult than the casting approach.  
  
Dim e1 As XElement = root.Element("Child1")  
Dim v1 As String  
If (e1 Is Nothing) Then  
    v1 = Nothing  
Else  
    v1 = e1.Value  
End If  
Console.WriteLine("v1:{0}", IIf(v1 Is Nothing, "element does not exist", v1))  
  
Dim e2 As XElement = root.Element("Child2")  
Dim v2 As Nullable(Of Integer)  
If (e2 Is Nothing) Then  
    v2 = Nothing  
Else  
    v2 = e2.Value  
End If  
Console.WriteLine("v2:{0}", IIf(Not (v2.HasValue), "element does not exist", v2))  
  
Dim e3 As XElement = root.Element("Child3")  
Dim v3 As String  
If (e3 Is Nothing) Then  
    v3 = Nothing  
Else  
    v3 = e3.Value  
End If  
Console.WriteLine("v3:{0}", IIf(v3 Is Nothing, "element does not exist", v3))  
  
Dim e4 As XElement = root.Element("Child4")  
Dim v4 As Nullable(Of Integer)  
If (e4 Is Nothing) Then  
    v4 = Nothing  
Else  
    v4 = e4.Value  
End If  
Console.WriteLine("v4:{0}", IIf(Not (v4.HasValue), "element does not exist", v4))  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
c1:child 1 content  
c2:2  
c3:element does not exist  
c4:element does not exist  
  
v1:child 1 content  
v2:2  
v3:element does not exist  
v4:element does not exist  
```  
  
 Em geral, você pode criar um código mais simples ao usar a conversão para recuperar o conteúdo de elementos e atributos.  
  
## <a name="see-also"></a>Confira também

- [Eixos LINQ to XML (Visual Basic)](linq-to-xml-axes.md)
