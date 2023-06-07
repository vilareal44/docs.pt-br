---
title: Como localizar elementos filho com base na posição (XPath-LINQ to XML) (C#)
description: Saiba como localizar elementos filho com base na posição usando uma expressão XPath. Examine um exemplo de código que usa um arquivo XML de exemplo.
ms.date: 07/20/2015
ms.assetid: e35bb269-ec86-4c96-8321-12491a0eb2c3
ms.openlocfilehash: 2603d3ac94ace645bde1ce85a43a43af7321014e
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301665"
---
# <a name="how-to-find-child-elements-based-on-position-xpath-linq-to-xml-c"></a>Como localizar elementos filho com base na posição (XPath-LINQ to XML) (C#)
Às vezes você deseja localizar elementos com base em sua posição. Você pode desejar localizar o segundo elemento, ou você talvez queira localizar o terceiro através do quinto elemento.  
  
 A expressão XPath é:  
  
 `Test[position() >= 2 and position() <= 4]`  
  
 Há duas abordagens para escrever esta consulta de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] de uma maneira lazy. Você pode usar os operadores de <xref:System.Linq.Enumerable.Skip%2A> e de <xref:System.Linq.Enumerable.Take%2A> , ou você pode usar a sobrecarga de <xref:System.Linq.Enumerable.Where%2A> que usa um índice. Quando você usa a sobrecarga de <xref:System.Linq.Enumerable.Where%2A> , você usa uma expressão lambda que leva dois argumentos. O exemplo a seguir mostra os dois métodos de selecione a posição de functionamento base.  
  
## <a name="example"></a>Exemplo  
 Este exemplo localiza o segundo a quarto elemento de `Test` . O resultado é uma coleção de elementos.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: configuração de teste (LINQ to XML)](./sample-xml-file-test-configuration-linq-to-xml.md).  
  
```csharp  
XElement testCfg = XElement.Load("TestConfig.xml");  
  
// LINQ to XML query  
IEnumerable<XElement> list1 =  
    testCfg  
    .Elements("Test")  
    .Skip(1)  
    .Take(3);  
  
// LINQ to XML query  
IEnumerable<XElement> list2 =  
    testCfg  
    .Elements("Test")  
    .Where((el, idx) => idx >= 1 && idx <= 3);  
  
// XPath expression  
IEnumerable<XElement> list3 =  
  testCfg.XPathSelectElements("Test[position() >= 2 and position() <= 4]");  
  
if (list1.Count() == list2.Count() &&  
    list1.Count() == list3.Count() &&  
    list1.Intersect(list2).Count() == list1.Count() &&  
    list1.Intersect(list3).Count() == list1.Count())  
    Console.WriteLine("Results are identical");  
else  
    Console.WriteLine("Results differ");  
foreach (XElement el in list1)  
    Console.WriteLine(el);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
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
