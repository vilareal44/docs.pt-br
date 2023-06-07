---
title: Manutenção de pares nome-valor
ms.date: 07/20/2015
ms.assetid: 57ac2072-d9f5-432b-84f0-a889c62fd813
ms.openlocfilehash: 1e30f010311dda393f65b1424e56f5b5ad014963
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84389211"
---
# <a name="maintaining-namevalue-pairs-visual-basic"></a>Mantendo pares de nome/valor (Visual Basic)
Muitos aplicativos devem manter informações que são melhor armazenadas como pares de valor/nome. Essas informações podem ser informações de configuração ou configurações globais. O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] contém alguns métodos que facilitam o trabalho de manter um conjunto de pares de valor/nome. É possível manter informações como atributos ou como um conjunto de elementos filho.  
  
 Uma das diferenças entre manter as informações como atributos ou como elementos filho é que os atributos possuem a restrição de que pode existir apenas um atributo com um nome específico para um elemento. Essa limitação não se aplica aos elementos filho.  
  
## <a name="setattributevalue-and-setelementvalue"></a>SetAttributeValue e SetElementValue  
 Os dois métodos que facilitam o trabalho de manter pares de valor/nome são <xref:System.Xml.Linq.XElement.SetAttributeValue%2A> e <xref:System.Xml.Linq.XElement.SetElementValue%2A>. Esses dois métodos possuem semântica semelhante.  
  
 <xref:System.Xml.Linq.XElement.SetAttributeValue%2A> pode adicionar, modificar ou remover atributos de um elemento.  
  
- Ao designar <xref:System.Xml.Linq.XElement.SetAttributeValue%2A> com o nome de um atributo inexistente, o método cria um novo atributo e o adiciona ao elemento especificado.  
  
- Ao designar <xref:System.Xml.Linq.XElement.SetAttributeValue%2A> com o nome de um atributo existente e algum conteúdo especificado, o conteúdo do atributo é substituído pelo conteúdo especificado.  
  
- Ao designar <xref:System.Xml.Linq.XElement.SetAttributeValue%2A> com o nome de um atributo existente e especificar nulo em relação ao conteúdo, o atributo é removido de seu pai.  
  
 <xref:System.Xml.Linq.XElement.SetElementValue%2A> pode adicionar, modificar, ou remover elementos filho de um elemento.  
  
- Ao designar <xref:System.Xml.Linq.XElement.SetElementValue%2A> com o nome de um elemento filho que não existe, o método cria um novo elemento e o adiciona ao elemento especificado.  
  
- Ao designar <xref:System.Xml.Linq.XElement.SetElementValue%2A> com o nome de um elemento existente e algum conteúdo especificado, o conteúdo do elemento é substituído pelo conteúdo especificado.  
  
- Ao designar <xref:System.Xml.Linq.XElement.SetElementValue%2A> com o nome de um elemento existente e especificar nulo em relação ao conteúdo, o elemento é removido de seu pai.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um elemento sem atributos. Usa o método <xref:System.Xml.Linq.XElement.SetAttributeValue%2A> para criar e manter uma lista de pares de valor/nome.  
  
```vb  
' Create an element with no content.  
Dim root As XElement = <Root/>  
  
' Add a number of name/value pairs as attributes.  
root.SetAttributeValue("Top", 22)  
root.SetAttributeValue("Left", 20)  
root.SetAttributeValue("Bottom", 122)  
root.SetAttributeValue("Right", 300)  
root.SetAttributeValue("DefaultColor", "Color.Red")  
Console.WriteLine(root)  
  
' Replace the value of Top.  
root.SetAttributeValue("Top", 10)  
Console.WriteLine(root)  
  
' Remove DefaultColor.  
root.SetAttributeValue("DefaultColor", Nothing)  
Console.WriteLine(root)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root Top="22" Left="20" Bottom="122" Right="300" DefaultColor="Color.Red" />  
<Root Top="10" Left="20" Bottom="122" Right="300" DefaultColor="Color.Red" />  
<Root Top="10" Left="20" Bottom="122" Right="300" />  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um elemento sem elementos filho. Usa o método <xref:System.Xml.Linq.XElement.SetElementValue%2A> para criar e manter uma lista de pares de valor/nome.  
  
```vb  
' Create an element with no content.  
Dim root As XElement = <Root/>  
  
' Add a number of name/value pairs as elements.  
root.SetElementValue("Top", 22)  
root.SetElementValue("Left", 20)  
root.SetElementValue("Bottom", 122)  
root.SetElementValue("Right", 300)  
root.SetElementValue("DefaultColor", "Color.Red")  
Console.WriteLine(root)  
Console.WriteLine("----")  
  
' Replace the value of Top.  
root.SetElementValue("Top", 10)  
Console.WriteLine(root)  
Console.WriteLine("----")  
  
' Remove DefaultColor.  
root.SetElementValue("DefaultColor", Nothing)  
Console.WriteLine(root)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root>  
  <Top>22</Top>  
  <Left>20</Left>  
  <Bottom>122</Bottom>  
  <Right>300</Right>  
  <DefaultColor>Color.Red</DefaultColor>  
</Root>
----  
<Root>  
  <Top>10</Top>  
  <Left>20</Left>  
  <Bottom>122</Bottom>  
  <Right>300</Right>  
  <DefaultColor>Color.Red</DefaultColor>  
</Root>
----
<Root>  
  <Top>10</Top>  
  <Left>20</Left>  
  <Bottom>122</Bottom>  
  <Right>300</Right>  
</Root>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement.SetAttributeValue%2A>
- <xref:System.Xml.Linq.XElement.SetElementValue%2A>
- [Modificando árvores XML (LINQ to XML) (Visual Basic)](modifying-xml-trees-linq-to-xml.md)
