---
title: Comparação XPath e de LINQ to XML
description: Saiba mais sobre as semelhanças e diferenças na funcionalidade entre XPath e LINQ to XML em C#. Você pode usar ambos para consultar uma árvore XML.
ms.date: 07/20/2015
dev_langs:
- csharp
- vb
ms.assetid: 87d361b1-daa9-4fd4-a53a-cbfa40111ad3
ms.openlocfilehash: e2f34903b20a53dea6e5ff4858d4fadaebd9c37a
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104010"
---
# <a name="comparison-of-xpath-and-linq-to-xml"></a>Comparação XPath e de LINQ to XML
O XPath e o LINQ to XML fornecem alguma funcionalidade semelhante. Ambos podem ser usados para ver uma árvore XML, retornando resultados como uma coleção de elementos, uma coleção de atributos, uma coleção de nós, ou o valor de um elemento ou de um atributo. No entanto, também há algumas diferenças.  
  
## <a name="differences-between-xpath-and-linq-to-xml"></a>Diferenças entre o XPath e o LINQ to XML  
 O XPath não permite a projeção de novos tipos. Somente é possível retornar coleções de nós de árvore, enquanto o LINQ to XML pode executar uma consulta e projetar um grafo de objeto ou uma árvore XML em uma nova forma. Consultas do LINQ to XML abrangem muito mais funcionalidade e são muito mais avançadas do que expressões XPath.  
  
 As expressões XPath existem no isolamento dentro de uma cadeia de caracteres. O compilador do C# não pode ajudar a analisar em tempo de compilação a expressão XPath. Por outro lado, as consultas do LINQ to XML são analisadas e compiladas pelo compilador do C#. O compilador pode capturar muitos erros de consulta.  
  
 Os resultados XPath não são altamente digitados. Em um número das circunstâncias, o resultado para avaliar uma expressão XPath é um objeto, e até o desenvolvedor para determinar o tipo apropriado e conforme necessário para converter o resultado. Por outro lado, as projeções de uma consulta do LINQ to XML são fortemente tipadas.  
  
## <a name="result-ordering"></a>Ordenação de resultado  
 O XPath 1,0 estados de recomendação que uma coleção que é o resultado de avaliar uma expressão XPath é não ordenada.  
  
 Entretanto, ao fazer iterações por uma coleção retornada por um método do eixo XPath do LINQ to XML, os nós na coleção são retornados na ordem do documento. Este é o caso mesmo quando acessando os eixos XPath onde os predicados são expressos em termos de ordem inversa de documento, como `preceding` e `preceding-sibling`.  
  
 Por outro lado, a maioria dos eixos do LINQ to XML retorna coleções na ordem de documento, mas por dois deles, <xref:System.Xml.Linq.XNode.Ancestors%2A> e <xref:System.Xml.Linq.XElement.AncestorsAndSelf%2A>, retornam coleções na ordem inversa do documento. A tabela a seguir enumera os eixos, e indica a ordem de coleção para cada:  
  
|O eixo LINQ to XML|Ordenando|  
|----------------------|--------------|  
|XContainer.DescendantNodes|Ordem de documento|  
|XContainer.Descendants|Ordem de documento|  
|XContainer.Elements|Ordem de documento|  
|XContainer.Nodes|Ordem de documento|  
|XContainer.NodesAfterSelf|Ordem de documento|  
|XContainer.NodesBeforeSelf|Ordem de documento|  
|XElement.AncestorsAndSelf|Ordem inversa de documento|  
|XElement.Attributes|Ordem de documento|  
|XElement.DescendantNodesAndSelf|Ordem de documento|  
|XElement.DescendantsAndSelf|Ordem de documento|  
|XNode.Ancestors|Ordem inversa de documento|  
|XNode.ElementsAfterSelf|Ordem de documento|  
|XNode.ElementsBeforeSelf|Ordem de documento|  
|XNode.NodesAfterSelf|Ordem de documento|  
|XNode.NodesBeforeSelf|Ordem de documento|  
  
## <a name="positional-predicates"></a>Predicados posicionais  
 Em uma expressão XPath, os predicados posicionais são expressos em termos de pedido de documento para muitos eixos, mas expressos na ordem inversa de documento para os eixos invertidos, que são `preceding`, `preceding-sibling`, `ancestor`, e `ancestor-or-self`. Por exemplo, a expressão XPath `preceding-sibling::*[1]` retorna imediatamente antes o irmão. Este é o caso mesmo que o conjunto de resultados final é apresentado na ordem de documento.  
  
 Por outro lado, todos os predicados posicionais em LINQ to XML sempre são expressos em termos de pedido do eixo. Por exemplo, `anElement.ElementsBeforeSelf().ElementAt(0)` retorna o primeiro elemento filho do pai do elemento consultado, não irmão anterior imediato. Outro exemplo: `anElement.Ancestors().ElementAt(0)` retorna o elemento pai.  
  
 Se você desejar localizar imediatamente o elemento precedente no LINQ to XML, escreva a expressão a seguir:  
  
```csharp
ElementsBeforeSelf().Last()
```
  
```vb
ElementsBeforeSelf().Last()
```
  
## <a name="performance-differences"></a>Diferenças de desempenho  
 As consultas XPath que usam a funcionalidade XPath no LINQ to XML não serão tão bem executadas como as consultas do LINQ to XML.  
  
## <a name="comparison-of-composition"></a>Comparação de composição  
 A composição de uma consulta do LINQ to XML é um pouco paralela à composição de uma expressão XPath, embora muito diferente na sintaxe.  
  
 Por exemplo, se você tiver um elemento em uma variável chamada `customers`, e você desejar localizar um elemento de neto chamado `CompanyName` em todos os elementos filho chamados `Customer`, escreva uma expressão XPath como segue:  
  
```csharp  
customers.XPathSelectElements("./Customer/CompanyName")
```  
  
```vb
customers.XPathSelectElements("./Customer/CompanyName")
```

 A consulta do LINQ to XML equivalente é:  
  
```csharp  
customers.Elements("Customer").Elements("CompanyName")
```  
  
```vb
customers.Elements("Customer").Elements("CompanyName")
```  

 Há paralelas semelhantes para cada um dos eixos XPath.  
  
|O eixo XPath|O eixo LINQ to XML|  
|----------------|----------------------|  
|o eixo filho (padrão)|<xref:System.Xml.Linq.XContainer.Elements%2A?displayProperty=nameWithType>|  
|Pai (.).|<xref:System.Xml.Linq.XObject.Parent%2A?displayProperty=nameWithType>|  
|o eixo de atributo (@)|<xref:System.Xml.Linq.XElement.Attribute%2A?displayProperty=nameWithType><br /><br /> ou<br /><br /> <xref:System.Xml.Linq.XElement.Attributes%2A?displayProperty=nameWithType>|  
|o eixo de ancestral|<xref:System.Xml.Linq.XNode.Ancestors%2A?displayProperty=nameWithType>|  
|o eixo de antepassado-ou- auto|<xref:System.Xml.Linq.XElement.AncestorsAndSelf%2A?displayProperty=nameWithType>|  
|o eixo descendente (/)|<xref:System.Xml.Linq.XContainer.Descendants%2A?displayProperty=nameWithType><br /><br /> ou<br /><br /> <xref:System.Xml.Linq.XContainer.DescendantNodes%2A?displayProperty=nameWithType>|  
|descendente-ou-auto|<xref:System.Xml.Linq.XElement.DescendantsAndSelf%2A?displayProperty=nameWithType><br /><br /> ou<br /><br /> <xref:System.Xml.Linq.XElement.DescendantNodesAndSelf%2A?displayProperty=nameWithType>|  
|seguir-irmão|<xref:System.Xml.Linq.XNode.ElementsAfterSelf%2A?displayProperty=nameWithType><br /><br /> ou<br /><br /> <xref:System.Xml.Linq.XNode.NodesAfterSelf%2A?displayProperty=nameWithType>|  
|acima-irmão|<xref:System.Xml.Linq.XNode.ElementsBeforeSelf%2A?displayProperty=nameWithType><br /><br /> ou<br /><br /> <xref:System.Xml.Linq.XNode.NodesBeforeSelf%2A?displayProperty=nameWithType>|  
|após|Nenhum equivalente direto.|  
|precedência|Nenhum equivalente direto.|  
  