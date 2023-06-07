---
title: 'Como: localizar elementos filho com base na posição (XPath-LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 6831e1db-5e97-444f-a7a1-d0a87104b005
ms.openlocfilehash: d6dd1150ae3e4ad586e476b777b1f7d47d60c261
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405255"
---
# <a name="how-to-find-child-elements-based-on-position-xpath-linq-to-xml-visual-basic"></a>Como localizar elementos filho com base na posição (XPath-LINQ to XML) (Visual Basic)
Às vezes você deseja localizar elementos com base em sua posição. Você pode desejar localizar o segundo elemento, ou você talvez queira localizar o terceiro através do quinto elemento.  
  
 A expressão XPath é:  
  
 `Test[position() >= 2 and position() <= 4]`  
  
 Há duas abordagens para escrever esta consulta de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] de uma maneira lazy. Você pode usar os operadores de <xref:System.Linq.Enumerable.Skip%2A> e de <xref:System.Linq.Enumerable.Take%2A> , ou você pode usar a sobrecarga de <xref:System.Linq.Enumerable.Where%2A> que usa um índice. Quando você usa a sobrecarga de <xref:System.Linq.Enumerable.Where%2A> , você usa uma expressão lambda que leva dois argumentos. O exemplo a seguir mostra os dois métodos de selecione a posição de functionamento base.  
  
## <a name="example"></a>Exemplo  
 Este exemplo localiza o segundo a quarto elemento de `Test` . O resultado é uma coleção de elementos.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: configuração de teste (LINQ to XML)](sample-xml-file-test-configuration-linq-to-xml.md).  
  
```vb  
Dim testCfg As XElement = XElement.Load("TestConfig.xml")  
  
' LINQ to XML query  
Dim list1 As IEnumerable(Of XElement) = _  
    testCfg.Elements("Test").Skip(1).Take(3)  
  
'LINQ to XML query  
Dim list2 As IEnumerable(Of XElement) = _  
    testCfg.Elements("Test"). _  
    Where(Function(ByVal el, ByVal idx) idx >= 1 And idx <= 3)  
  
' XPath expression  
Dim list3 As IEnumerable(Of XElement) = _  
    testCfg.XPathSelectElements("Test[position() >= 2 and position() <= 4]")  
  
If list1.Count() = list2.Count() And _  
       list1.Count() = list3.Count() And _  
       list1.Intersect(list2).Count() = list1.Count() And _  
       list1.Intersect(list3).Count() = list1.Count() Then  
  
    Console.WriteLine("Results are identical")  
Else  
    Console.WriteLine("Results differ")  
End If  
  
For Each el As XElement In list1  
    Console.WriteLine(el)  
Next  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
Results are identical  
<Test TestId="0002" TestType="CMD">  
  <Name>Find succeeding characters</Name>  
  <CommandLine>Examp2.EXE</CommandLine>  
  <Input>abc</Input>  
  <Output>def</Output>  
</Test>  
<Test TestId="0003" TestType="GUI">  
  <Name>Convert multiple numbers to strings</Name>  
  <CommandLine>Examp2.EXE /Verbose</CommandLine>  
  <Input>123</Input>  
  <Output>One Two Three</Output>  
</Test>  
<Test TestId="0004" TestType="GUI">  
  <Name>Find correlated key</Name>  
  <CommandLine>Examp3.EXE</CommandLine>  
  <Input>a1</Input>  
  <Output>b1</Output>  
</Test>  
```  
  
## <a name="see-also"></a>Confira também

- [LINQ to XML para usuários do XPath (Visual Basic)](linq-to-xml-for-xpath-users.md)
