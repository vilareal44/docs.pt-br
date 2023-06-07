---
title: Eventos LINQ to XML (C#)
description: Adicione LINQ to XML eventos em C# a uma instância de qualquer XObject. O manipulador de eventos recebe eventos quando a árvore XML para esse XObject é modificada.
ms.date: 07/20/2015
ms.assetid: ce7de951-cba7-4870-9962-733eb01cd680
ms.openlocfilehash: 576b0a5d0472bddd66e01d3bef8f3affa1c9458b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165417"
---
# <a name="linq-to-xml-events-c"></a>Eventos LINQ to XML (C#)
Eventos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permitem que você seja notificado quando uma árvore XML é modificada.  
  
 Você pode adicionar eventos a uma instância de qualquer <xref:System.Xml.Linq.XObject>. O manipulador de eventos em receberá eventos para alterações ao <xref:System.Xml.Linq.XObject> e a qualquer um dos seus descendentes. Por exemplo, você pode adicionar um manipulador de eventos à raiz da árvore, e trata todas as alterações na árvore do manipulador de eventos.  
  
 Para exemplos de eventos de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], consulte <xref:System.Xml.Linq.XObject.Changing> e <xref:System.Xml.Linq.XObject.Changed>.  
  
## <a name="types-and-events"></a>Tipos e eventos  
 Você usa os seguintes tipos ao trabalhar com eventos:  
  
|Tipo|DESCRIÇÃO|  
|----------|-----------------|  
|<xref:System.Xml.Linq.XObjectChange>|Especifica o tipo de evento quando um evento é gerado para <xref:System.Xml.Linq.XObject>.|  
|<xref:System.Xml.Linq.XObjectChangeEventArgs>|Fornece dados para os eventos de <xref:System.Xml.Linq.XObject.Changing> e de <xref:System.Xml.Linq.XObject.Changed> .|  
  
 Os seguintes eventos são gerados quando você altera uma árvore XML:  
  
|Evento|Descrição|  
|-----------|-----------------|  
|<xref:System.Xml.Linq.XObject.Changing>|Ocorre antes deste <xref:System.Xml.Linq.XObject> ou alguns dos seus descendentes são indo alterar.|  
|<xref:System.Xml.Linq.XObject.Changed>|Ocorre quando <xref:System.Xml.Linq.XObject> alterar ou alguns dos seus descendentes alterado.|  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 Os eventos são úteis quando você deseja manter algumas informações aggregate em uma árvore XML. Por exemplo, você pode querer mantém um total de fatura que é a soma das linhas de item de fatura. Este exemplo usa eventos para manter o total de todos os elementos filho no elemento complexo `Items`.  
  
### <a name="code"></a>Código  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("Total", "0"),  
    new XElement("Items")  
);  
XElement total = root.Element("Total");  
XElement items = root.Element("Items");  
items.Changed += (object sender, XObjectChangeEventArgs cea) =>  
{  
    switch (cea.ObjectChange)  
    {  
        case XObjectChange.Add:  
            if (sender is XElement)  
                total.Value = ((int)total + (int)(XElement)sender).ToString();  
            if (sender is XText)  
                total.Value = ((int)total + (int)((XText)sender).Parent).ToString();  
            break;  
        case XObjectChange.Remove:  
            if (sender is XElement)  
                total.Value = ((int)total - (int)(XElement)sender).ToString();  
            if (sender is XText)  
                total.Value = ((int)total - Int32.Parse(((XText)sender).Value)).ToString();  
            break;  
    }  
    Console.WriteLine("Changed {0} {1}",  
      sender.GetType().ToString(), cea.ObjectChange.ToString());  
};  
items.SetElementValue("Item1", 25);  
items.SetElementValue("Item2", 50);  
items.SetElementValue("Item2", 75);  
items.SetElementValue("Item3", 133);  
items.SetElementValue("Item1", null);  
items.SetElementValue("Item4", 100);  
Console.WriteLine("Total:{0}", (int)total);  
Console.WriteLine(root);  
```  
  
### <a name="comments"></a>Comentários  
 Este código produz a seguinte saída:  
  
```output  
Changed System.Xml.Linq.XElement Add  
Changed System.Xml.Linq.XElement Add  
Changed System.Xml.Linq.XText Remove  
Changed System.Xml.Linq.XText Add  
Changed System.Xml.Linq.XElement Add  
Changed System.Xml.Linq.XElement Remove  
Changed System.Xml.Linq.XElement Add  
Total:308  
<Root>  
  <Total>308</Total>  
  <Items>  
    <Item2>75</Item2>  
    <Item3>133</Item3>  
    <Item4>100</Item4>  
  </Items>  
</Root>  
```  
  