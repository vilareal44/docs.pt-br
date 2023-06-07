---
title: Eventos LINQ to XML
ms.date: 07/20/2015
ms.assetid: 34923928-b99c-4004-956e-38f6db25e910
ms.openlocfilehash: d00f6f1b2a14ac73c1bcd4a1f74b9714ca304da3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84368810"
---
# <a name="linq-to-xml-events-visual-basic"></a>Eventos de LINQ to XML (Visual Basic)
Eventos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permitem que você seja notificado quando uma árvore XML é modificada.  
  
 Você pode adicionar eventos a uma instância de qualquer <xref:System.Xml.Linq.XObject>. O manipulador de eventos em receberá eventos para alterações ao <xref:System.Xml.Linq.XObject> e a qualquer um dos seus descendentes. Por exemplo, você pode adicionar um manipulador de eventos à raiz da árvore, e trata todas as alterações na árvore do manipulador de eventos.  
  
 Para exemplos de eventos de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], consulte <xref:System.Xml.Linq.XObject.Changing> e <xref:System.Xml.Linq.XObject.Changed>.  
  
## <a name="types-and-events"></a>Tipos e eventos  
 Você usa os seguintes tipos ao trabalhar com eventos:  
  
|Tipo|Description|  
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
  
```vb  
Module Module1  
    Dim WithEvents items As XElement = Nothing  
    Dim total As XElement = Nothing  
    Dim root As XElement = _  
            <Root>  
                <Total>0</Total>  
                <Items></Items>  
            </Root>  
  
    Private Sub XObjectChanged( _  
            ByVal sender As Object, _  
            ByVal cea As XObjectChangeEventArgs) _  
            Handles items.Changed  
        Select Case cea.ObjectChange  
            Case XObjectChange.Add  
                If sender.GetType() Is GetType(XElement) Then  
                    total.Value = CStr(CInt(total.Value) + _  
                            CInt((DirectCast(sender, XElement)).Value))  
                End If  
                If sender.GetType() Is GetType(XText) Then  
                    total.Value = CStr(CInt(total.Value) + _  
                            CInt((DirectCast(sender, XText)).Value))  
                End If  
            Case XObjectChange.Remove  
                If sender.GetType() Is GetType(XElement) Then  
                    total.Value = CStr(CInt(total.Value) - _  
                            CInt((DirectCast(sender, XElement)).Value))  
                End If  
                If sender.GetType() Is GetType(XText) Then  
                    total.Value = CStr(CInt(total.Value) - _  
                            CInt((DirectCast(sender, XText)).Value))  
                End If  
        End Select  
        Console.WriteLine("Changed {0} {1}", _  
                            sender.GetType().ToString(), _  
                            cea.ObjectChange.ToString())  
    End Sub  
  
    Sub Main()  
        total = root.<Total>(0)  
        items = root.<Items>(0)  
        items.SetElementValue("Item1", 25)  
        items.SetElementValue("Item2", 50)  
        items.SetElementValue("Item2", 75)  
        items.SetElementValue("Item3", 133)  
        items.SetElementValue("Item1", Nothing)  
        items.SetElementValue("Item4", 100)  
        Console.WriteLine("Total:{0}", CInt(total))  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
### <a name="comments"></a>Comentários  
 Esse código gera a seguinte saída:  
  
```console  
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
  
## <a name="see-also"></a>Confira também

- [Programação de LINQ to XML avançada (Visual Basic)](advanced-linq-to-xml-programming.md)
