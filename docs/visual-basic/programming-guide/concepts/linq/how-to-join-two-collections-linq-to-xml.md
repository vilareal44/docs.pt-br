---
title: 'Como: unir duas coleções (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 5a5758d4-906b-4285-908d-5b930db192e6
ms.openlocfilehash: dc3cfd19d990fa81e00f4781cb15bf07eb9a80ea
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398045"
---
# <a name="how-to-join-two-collections-linq-to-xml-visual-basic"></a>Como unir duas coleções (LINQ to XML) (Visual Basic)
Um elemento ou atributo em um documento XML, algumas vezes, pode fazer referência a outro elemento ou atributo. Por exemplo, o documento XML [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](sample-xml-file-customers-and-orders-linq-to-xml.md) contém uma lista de clientes e uma lista de pedidos. Cada elemento `Customer` contém um atributo `CustomerID`. Cada elemento `Order` contém um atributo `CustomerID`. O elemento `CustomerID` em cada pedido faz referência ao atributo `CustomerID` em um cliente.  
  
 O tópico [Arquivo XSD de exemplo: clientes e pedidos](sample-xsd-file-customers-and-orders.md) contém um XSD que pode ser usado para validar esse documento. Usa os recursos de XSD `xs:key` e `xs:keyref` para estabelecer que o atributo `CustomerID` do elemento `Customer` é uma chave, e para estabelecer uma relação entre o elemento `CustomerID` em cada elemento `Order`, e o atributo `CustomerID` em cada elemento `Customer`.  
  
 Com o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], você pode tirar proveito dessa relação usando a cláusula `Join`.  
  
 Observe que como não existe nenhum índice disponível, essa junção terá um desempenho inadequado em runtime.  
  
 Para obter informações mais detalhadas sobre o `Join` , consulte [operações de junção (Visual Basic)](join-operations.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir une os elementos `Customer` aos elementos `Order` e gera um novo documento XML que inclui o elemento `CompanyName` nos pedidos.  
  
 Antes de executar a consulta, o exemplo valida que o documento está de acordo com o esquema em [Arquivo XSD de exemplo: clientes e pedidos](sample-xsd-file-customers-and-orders.md). Isso garante que a cláusula join sempre funcionará.  
  
 Esta consulta retorna primeiro todos os elementos `Customer` e, em seguida, une-os aos elementos `Order`. A consulta seleciona apenas os pedidos de clientes com um `CustomerID` maior que "K". Em seguida, projeta um novo elemento `Order` que contém as informações do cliente dentro de cada pedido.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](sample-xml-file-customers-and-orders-linq-to-xml.md).  
  
 Este exemplo usa o seguinte esquema XSD: [Arquivo XSD de exemplo: clientes e pedidos](sample-xsd-file-customers-and-orders.md).  
  
 Observe que a junção dessa forma não será muito bem-executada. As junções são executadas por meio de uma pesquisa linear. Não há nenhuma tabela de hash ou índice para ajudar no desempenho.  
  
```vb  
Public Class Program  
    Public Shared errors As Boolean = False  
  
    Public Shared Function LamValidEvent(ByVal o As Object, _  
                 ByVal e As ValidationEventArgs) As Boolean  
        Console.WriteLine("{0}", e.Message)  
        errors = True  
    End Function  
  
    Shared Sub Main()  
        Dim schemas As New XmlSchemaSet()  
        schemas.Add("", "CustomersOrders.xsd")  
  
        Console.Write("Attempting to validate, ")  
        Dim custOrdDoc As XDocument = XDocument.Load("CustomersOrders.xml")  
  
        custOrdDoc.Validate(schemas, Function(o, e) LamValidEvent(0, e))  
        If errors Then  
            Console.WriteLine("custOrdDoc did not validate")  
        Else  
            Console.WriteLine("custOrdDoc validated")  
        End If  
  
        If Not errors Then  
            'Join customers and orders, and create a new XML document with  
            ' a different shape.  
            'The new document contains orders only for customers with a  
            ' CustomerID > 'K'.  
            Dim custOrd As XElement = custOrdDoc.<Root>.FirstOrDefault  
            Dim newCustOrd As XElement = _  
                <Root>  
                    <%= From c In custOrd.<Customers>.<Customer> _  
                        Join o In custOrd.<Orders>.<Order> _  
                        On c.@CustomerID Equals o.<CustomerID>.Value _  
                        Where c.@CustomerID.CompareTo("K") > 0 _  
                        Select _  
                        <Order>  
                            <CustomerID><%= c.@CustomerID %></CustomerID>  
                            <%= c.<CompanyName> %>  
                            <%= c.<ContactName> %>  
                            <%= o.<EmployeeID> %>  
                            <%= o.<OrderDate> %>  
                        </Order> _  
                    %>  
                </Root>  
            Console.WriteLine(newCustOrd)  
        End If  
    End Sub  
End Class  
```  
  
 Esse código gera a seguinte saída:  
  
```console
Attempting to validate, custOrdDoc validated  
<Root>  
  <Order>  
    <CustomerID>LAZYK</CustomerID>  
    <CompanyName>Lazy K Kountry Store</CompanyName>  
    <ContactName>John Steel</ContactName>  
    <EmployeeID>1</EmployeeID>  
    <OrderDate>1997-03-21T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LAZYK</CustomerID>  
    <CompanyName>Lazy K Kountry Store</CompanyName>  
    <ContactName>John Steel</ContactName>  
    <EmployeeID>8</EmployeeID>  
    <OrderDate>1997-05-22T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>1</EmployeeID>  
    <OrderDate>1997-06-25T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>8</EmployeeID>  
    <OrderDate>1997-10-27T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>6</EmployeeID>  
    <OrderDate>1997-11-10T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>4</EmployeeID>  
    <OrderDate>1998-02-12T00:00:00</OrderDate>  
  </Order>  
</Root>  
```  
  
## <a name="see-also"></a>Confira também

- [Técnicas de consulta avançada (LINQ to XML) (Visual Basic)](advanced-query-techniques-linq-to-xml.md)
