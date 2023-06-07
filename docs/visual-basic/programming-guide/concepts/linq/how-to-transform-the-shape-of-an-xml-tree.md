---
title: 'Como: transformar a forma de uma árvore XML'
ms.date: 07/20/2015
ms.assetid: 84b60854-48b2-452c-87f2-77d53e1d653a
ms.openlocfilehash: 90fa23df09972eb76154dc47ce0a025e85a12ea3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397655"
---
# <a name="how-to-transform-the-shape-of-an-xml-tree-visual-basic"></a>Como transformar a forma de uma árvore XML (Visual Basic)
A *forma* de um documento XML refere-se aos seus nomes de elemento, nomes de atributo e às características de sua hierarquia.  
  
 Às vezes você precisará alterar a forma de um documento XML. Por exemplo, você pode ter que enviar um documento XML existente para outro sistema que requer nomes diferentes de elementos e atributos. Você pode examinar o documento, excluir e renomeando elementos conforme necessário, mas usando funcionais resultados de compilação em um código mais legível e mais sustentável. Para obter mais informações sobre a construção funcional, consulte [construção funcional (LINQ to XML) (Visual Basic)](functional-construction-linq-to-xml.md).  
  
 O primeiro exemplo altera a organização de documento XML. Mover elementos complexos de um local na árvore para outro.  
  
 O segundo exemplo neste tópico cria um documento XML com uma forma diferente do documento de origem. Altera a caixa de nomes de elemento, renomear alguns elementos, e permitir que alguns elementos da árvore de origem fora da árvore transformada.  
  
## <a name="example"></a>Exemplo  
 As seguintes alterações de código a forma de um arquivo XML usando expressões inseridas de consulta.  
  
 O documento XML de origem neste exemplo contém um elemento de `Customers` no elemento de `Root` que contém todos os clientes. Também contém um elemento de `Orders` no elemento de `Root` que contém todos os pedidos. Este exemplo cria uma nova árvore XML em que os pedidos para cada cliente estão contidos em um elemento de `Orders` dentro do elemento de `Customer` . O documento original também contém um elemento de `CustomerID` no elemento de `Order` ; este elemento será removido do documento reformulado.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](sample-xml-file-customers-and-orders-linq-to-xml.md).  
  
```vb  
Dim co As XElement = XElement.Load("CustomersOrders.xml")  
Dim newCustOrd = _  
    <Root>  
        <%= From cust In co.<Customers>.<Customer> _  
            Select _  
            <Customer>  
                <%= cust.Attributes() %>  
                <%= cust.Elements() %>  
                <Orders>  
                    <%= From ord In co.<Orders>.<Order> _  
                        Where ord.<CustomerID>.Value = cust.@CustomerID _  
                        Select _  
                        <Order>  
                            <%= ord.Attributes() %>  
                            <%= ord.<EmployeeID> %>  
                            <%= ord.<OrderDate> %>  
                            <%= ord.<RequiredDate> %>  
                            <%= ord.<ShipInfo> %>  
                        </Order> _  
                    %>  
                </Orders>  
            </Customer> _  
        %>  
    </Root>  
Console.WriteLine(newCustOrd)  
```  
  
 Esse código gera a seguinte saída:  
  
```xml  
<Root>  
<Customer CustomerID="GREAL">  
  <CompanyName>Great Lakes Food Market</CompanyName>  
  <ContactName>Howard Snyder</ContactName>  
  <ContactTitle>Marketing Manager</ContactTitle>  
  <Phone>(503) 555-7555</Phone>  
  <FullAddress>  
    <Address>2732 Baker Blvd.</Address>  
    <City>Eugene</City>  
    <Region>OR</Region>  
    <PostalCode>97403</PostalCode>  
    <Country>USA</Country>  
  </FullAddress>  
  <Orders />  
</Customer>  
<Customer CustomerID="HUNGC">  
  <CompanyName>Hungry Coyote Import Store</CompanyName>  
  <ContactName>Yoshi Latimer</ContactName>  
  <ContactTitle>Sales Representative</ContactTitle>  
  <Phone>(503) 555-6874</Phone>  
  <Fax>(503) 555-2376</Fax>  
  <FullAddress>  
    <Address>City Center Plaza 516 Main St.</Address>  
    <City>Elgin</City>  
    <Region>OR</Region>  
    <PostalCode>97827</PostalCode>  
    <Country>USA</Country>  
  </FullAddress>  
  <Orders />  
</Customer>  
...
</Root>
```  
  
## <a name="example"></a>Exemplo  
 Este exemplo renomeia alguns elementos e converte alguns atributos para elementos.  
  
 O código chama `ConvertAddress`, que retorna uma lista de objetos <xref:System.Xml.Linq.XElement> . O argumento para o método é uma consulta que determina o elemento complexo de `Address` onde o atributo de `Type` tem um valor de `"Shipping"`.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](sample-xml-file-typical-purchase-order-linq-to-xml.md).  
  
```vb  
Function ConvertAddress(ByVal add As XElement) As IEnumerable(Of XElement)  
    Dim fragment = New List(Of XElement)  
    fragment.Add(<NAME><%= add.<Name>.Value %></NAME>)  
    fragment.Add(<STREET><%= add.<Street>.Value %></STREET>)  
    fragment.Add(<CITY><%= add.<City>.Value %></CITY>)  
    fragment.Add(<ST><%= add.<State>.Value %></ST>)  
    fragment.Add(<POSTALCODE><%= add.<Zip>.Value %></POSTALCODE>)  
    fragment.Add(<COUNTRY><%= add.<Country>.Value %></COUNTRY>)  
    Return fragment  
End Function  
  
Sub Main()  
    Dim po As XElement = XElement.Load("PurchaseOrder.xml")  
    Dim newPo As XElement = _  
        <PO>  
            <ID><%= po.@PurchaseOrderNumber %></ID>  
            <DATE><%= CDate(po.@OrderDate) %></DATE>  
            <%= _  
                ConvertAddress( _  
                (From el In po.<Address> _  
                Where el.@Type = "Shipping" _  
                Select el) _  
                .First() _  
                ) _  
            %>  
        </PO>  
    Console.WriteLine(newPo)  
End Sub  
```  
  
 Esse código gera a seguinte saída:  
  
```xml  
<PO>  
  <ID>99503</ID>  
  <DATE>1999-10-20T00:00:00</DATE>  
  <NAME>Ellen Adams</NAME>  
  <STREET>123 Maple Street</STREET>  
  <CITY>Mill Valley</CITY>  
  <ST>CA</ST>  
  <POSTALCODE>10999</POSTALCODE>  
  <COUNTRY>USA</COUNTRY>  
</PO>  
```  
  
## <a name="see-also"></a>Confira também

- [Projeções e transformações (LINQ to XML) (Visual Basic)](projections-and-transformations-linq-to-xml.md)
