---
title: Como unir duas coleções (LINQ to XML) (C#)
description: Este exemplo de C# une os elementos em LINQ to XML a outros elementos e gera um novo documento XML.
ms.date: 07/20/2015
ms.assetid: 7b817ede-911a-4cff-9dd3-639c3fc228c9
ms.openlocfilehash: 10792ed4907e778b41821c9b32574bd8fc0ab35f
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104991"
---
# <a name="how-to-join-two-collections-linq-to-xml-c"></a>Como unir duas coleções (LINQ to XML) (C#)

Um elemento ou atributo em um documento XML, algumas vezes, pode fazer referência a outro elemento ou atributo. Por exemplo, o documento XML [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](./sample-xml-file-customers-and-orders-linq-to-xml-2.md) contém uma lista de clientes e uma lista de pedidos. Cada elemento `Customer` contém um atributo `CustomerID`. Cada elemento `Order` contém um atributo `CustomerID`. O elemento `CustomerID` em cada pedido faz referência ao atributo `CustomerID` em um cliente.

O tópico [Arquivo XSD de exemplo: clientes e pedidos](./sample-xsd-file-customers-and-orders1.md) contém um XSD que pode ser usado para validar esse documento. Usa os recursos de XSD `xs:key` e `xs:keyref` para estabelecer que o atributo `CustomerID` do elemento `Customer` é uma chave, e para estabelecer uma relação entre o elemento `CustomerID` em cada elemento `Order`, e o atributo `CustomerID` em cada elemento `Customer`.

Com o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], você pode tirar proveito dessa relação usando a cláusula `join`.

Como não há índice disponível, essa junção terá um desempenho insatisfatório em tempo de execução.

Para obter informações mais detalhadas sobre `join`, consulte [Operações de junção (C#)](./join-operations.md).

## <a name="example"></a>Exemplo

O exemplo a seguir une os elementos `Customer` aos elementos `Order` e gera um novo documento XML que inclui o elemento `CompanyName` nos pedidos.

Antes de executar a consulta, o exemplo valida que o documento está de acordo com o esquema em [Arquivo XSD de exemplo: clientes e pedidos](./sample-xsd-file-customers-and-orders1.md). Isso garante que a cláusula join sempre funcionará.

Esta consulta retorna primeiro todos os elementos `Customer` e, em seguida, une-os aos elementos `Order`. A consulta seleciona apenas os pedidos de clientes com um `CustomerID` maior que "K". Em seguida, projeta um novo elemento `Order` que contém as informações do cliente dentro de cada pedido.

Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](./sample-xml-file-customers-and-orders-linq-to-xml-2.md).

Este exemplo usa o seguinte esquema XSD: [Arquivo XSD de exemplo: clientes e pedidos](./sample-xsd-file-customers-and-orders1.md).

Unir dessa maneira não terá um bom desempenho. As junções são executadas por meio de uma pesquisa linear. Não há nenhuma tabela de hash ou índice para ajudar no desempenho.

```csharp
XmlSchemaSet schemas = new XmlSchemaSet();
schemas.Add("", "CustomersOrders.xsd");

Console.Write("Attempting to validate, ");
XDocument custOrdDoc = XDocument.Load("CustomersOrders.xml");

bool errors = false;
custOrdDoc.Validate(schemas, (o, e) =>
                     {
                         Console.WriteLine("{0}", e.Message);
                         errors = true;
                     });
Console.WriteLine("custOrdDoc {0}", errors ? "did not validate" : "validated");

if (!errors)
{
    // Join customers and orders, and create a new XML document with
    // a different shape.

    // The new document contains orders only for customers with a
    // CustomerID > 'K'
    XElement custOrd = custOrdDoc.Element("Root");
    XElement newCustOrd = new XElement("Root",
        from c in custOrd.Element("Customers").Elements("Customer")
        join o in custOrd.Element("Orders").Elements("Order")
                   on (string)c.Attribute("CustomerID") equals
                      (string)o.Element("CustomerID")
        where ((string)c.Attribute("CustomerID")).CompareTo("K") > 0
        select new XElement("Order",
            new XElement("CustomerID", (string)c.Attribute("CustomerID")),
            new XElement("CompanyName", (string)c.Element("CompanyName")),
            new XElement("ContactName", (string)c.Element("ContactName")),
            new XElement("EmployeeID", (string)o.Element("EmployeeID")),
            new XElement("OrderDate", (DateTime)o.Element("OrderDate"))
        )
    );
    Console.WriteLine(newCustOrd);
}
```

Este código produz a seguinte saída:

```output
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
