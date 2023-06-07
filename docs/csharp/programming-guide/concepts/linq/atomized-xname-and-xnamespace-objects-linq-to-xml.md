---
title: Objetos XName e XNamespace atomizados (LINQ to XML) (C#)
description: Saiba mais sobre os objetos XName e XNamespace Atom e como eles fornecem benefícios de desempenho para consultas.
ms.date: 07/20/2015
ms.assetid: a5b21433-b49d-415c-b00e-bcbfb0d267d7
ms.openlocfilehash: 305a233adab5c860b5f9509ae25ccc5d633e7957
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104273"
---
# <a name="atomized-xname-and-xnamespace-objects-linq-to-xml-c"></a>Objetos XName e XNamespace atomizados (LINQ to XML) (C#)
Os objetos <xref:System.Xml.Linq.XName> e <xref:System.Xml.Linq.XNamespace> são *atomizados*, isto é, se eles contêm o mesmo nome qualificado, referem-se ao mesmo objeto. Este benefícios de desempenho das para consultas: Quando você compara dois nomes atomizados para igualdade, o linguagem intermediária subjacente só precisa determinar se o ponto de duas referências para o mesmo objeto. O código subjacente não tem que fazer as comparações de cadeias de caracteres, que poderiam demoradas.  
  
## <a name="atomization-semantics"></a>Semântica de atomização  
 A atomização significa que se dois objetos de <xref:System.Xml.Linq.XName> têm o mesmo nome local, e estão no mesmo namespace, compartilham a mesma instância. Da mesma forma, se dois objetos de <xref:System.Xml.Linq.XNamespace> têm o mesmo URI de namespace, compartilham a mesma instância.  
  
 Para que uma classe permite objetos atomizados, o construtor para a classe deve ser particular, não público. Isso ocorre porque se foi o construtor público, você pode criar um objeto não atomizado. As classes de <xref:System.Xml.Linq.XName> e de <xref:System.Xml.Linq.XNamespace> implementam um operador de conversão implícita para converter uma cadeia de caracteres em <xref:System.Xml.Linq.XName> ou em <xref:System.Xml.Linq.XNamespace>. Isso é como você obtém uma instância desses objetos. Você não pode obter uma instância usando um construtor, porque o construtor é inacessível.  
  
 <xref:System.Xml.Linq.XName> e <xref:System.Xml.Linq.XNamespace> também implementam os operadores de igualdade e desigualdade de, para determinar se dois objetos que estão sendo comparados são referências para a mesma instância.  
  
## <a name="example"></a>Exemplo  
 O código a seguir cria alguns objetos de <xref:System.Xml.Linq.XElement> e demonstrar-los que os nomes idênticos compartilham a mesma instância.  
  
```csharp  
XElement r1 = new XElement("Root", "data1");  
XElement r2 = XElement.Parse("<Root>data2</Root>");  
  
if ((object)r1.Name == (object)r2.Name)  
    Console.WriteLine("r1 and r2 have names that refer to the same instance.");  
else  
    Console.WriteLine("Different");  
  
XName n = "Root";  
  
if ((object)n == (object)r1.Name)  
    Console.WriteLine("The name of r1 and the name in 'n' refer to the same instance.");  
else  
    Console.WriteLine("Different");  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
r1 and r2 have names that refer to the same instance.  
The name of r1 and the name in 'n' refer to the same instance.  
```  
  
 Como mencionado anteriormente, a vantagem de objetos atomizados é que quando você usa um dos métodos do eixo que recebem <xref:System.Xml.Linq.XName> como um parâmetro, o método do eixo só precisa determinar que dois nomes referenciam a mesma instância para selecionar os elementos desejados.  
  
 O exemplo a seguir <xref:System.Xml.Linq.XName> passa a chamada de método <xref:System.Xml.Linq.XContainer.Descendants%2A> , que tem em melhor desempenho devido ao padrão de atomização.  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("C1", 1),  
    new XElement("Z1",  
        new XElement("C1", 2),  
        new XElement("C1", 1)  
    )  
);  
  
var query = from e in root.Descendants("C1")  
            where (int)e == 1  
            select e;  
  
foreach (var z in query)  
    Console.WriteLine(z);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<C1>1</C1>  
<C1>1</C1>  
```  
