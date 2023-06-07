---
title: Como projetar um novo tipo (LINQ to XML) (C#)
description: Saiba como criar consultas em LINQ to XML em C# que retornam IEnumerable <T> de tipos, além de XElement, String ou int, que são descritas em outros exemplos.
ms.date: 07/20/2015
ms.assetid: 48145cf9-1e0b-4e73-bbfd-28fc04800dc4
ms.openlocfilehash: 013ea852a64b77c04ac583b4d9b71e8006cd4976
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104647"
---
# <a name="how-to-project-a-new-type-linq-to-xml-c"></a>Como projetar um novo tipo (LINQ to XML) (C#)

Outros exemplos nesta seção mostraram consultas que os resultados de retorno como <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XElement>, <xref:System.Collections.Generic.IEnumerable%601> de `string`, e <xref:System.Collections.Generic.IEnumerable%601> de `int`. Esses são tipos comuns de resultado, mas não são adequados para cada cenário. Em muitos casos você desejará suas consultas para retornar <xref:System.Collections.Generic.IEnumerable%601> de qualquer outro tipo.

## <a name="example"></a>Exemplo

Este exemplo mostra como instanciar objetos na cláusula `select` . O código define primeiro uma nova classe com um construtor, e altera a declaração de `select` de modo que a expressão é uma nova instância da nova classe.

Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](./sample-xml-file-typical-purchase-order-linq-to-xml-1.md).

```csharp
class NameQty
{
    public string name;
    public int qty;
    public NameQty(string n, int q)
    {
        name = n;
        qty = q;
    }
};

class Program {
    public static void Main()
    {
        XElement po = XElement.Load("PurchaseOrder.xml");
  
        IEnumerable<NameQty> nqList =
            from n in po.Descendants("Item")
            select new NameQty(
                    (string)n.Element("ProductName"),
                    (int)n.Element("Quantity")
                );
  
        foreach (NameQty n in nqList)
            Console.WriteLine(n.name + ":" + n.qty);
    }
}
```

Este exemplo usa o <xref:System.Xml.Linq.XContainer.Element%2A> método que foi introduzido no tópico [como recuperar um único elemento filho (LINQ to XML) (C#)](how-to-retrieve-a-single-child-element-linq-to-xml.md). Também usa conversões para recuperar os valores dos elementos que são retornados pelo método de <xref:System.Xml.Linq.XContainer.Element%2A> .  

Esse exemplo gera a saída a seguir:

```output
Lawnmower:1
Baby Monitor:2
```
