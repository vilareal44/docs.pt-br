---
title: Como transformar a forma de uma árvore XML (C#)
description: Saiba como transformar a forma de uma árvore XML. A forma de uma árvore XML refere-se a seus nomes de elementos e atributos e suas características de hierarquia.
ms.date: 07/20/2015
ms.assetid: 93c5d426-dea2-4709-a991-60204de42e8f
ms.openlocfilehash: 4fa3cc18f235d061ae1778c177c4ac9b626f4b71
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302640"
---
# <a name="how-to-transform-the-shape-of-an-xml-tree-c"></a>Como transformar a forma de uma árvore XML (C#)
A *forma* de um documento XML refere-se aos seus nomes de elemento, nomes de atributo e às características de sua hierarquia.  
  
 Às vezes você precisará alterar a forma de um documento XML. Por exemplo, você pode ter que enviar um documento XML existente para outro sistema que requer nomes diferentes de elementos e atributos. Você pode examinar o documento, excluir e renomeando elementos conforme necessário, mas usando funcionais resultados de compilação em um código mais legível e mais sustentável. Para obter mais informações sobre a construção funcional, consulte [Construção funcional (LINQ to XML) (C#)](./functional-construction-linq-to-xml.md).  
  
 O primeiro exemplo altera a organização de documento XML. Mover elementos complexos de um local na árvore para outro.  
  
 O segundo exemplo neste tópico cria um documento XML com uma forma diferente do documento de origem. Altera a caixa de nomes de elemento, renomear alguns elementos, e permitir que alguns elementos da árvore de origem fora da árvore transformada.  
  
## <a name="example"></a>Exemplo  
 As seguintes alterações de código a forma de um arquivo XML usando expressões inseridas de consulta.  
  
 O documento XML de origem neste exemplo contém um elemento de `Customers` no elemento de `Root` que contém todos os clientes. Também contém um elemento de `Orders` no elemento de `Root` que contém todos os pedidos. Este exemplo cria uma nova árvore XML em que os pedidos para cada cliente estão contidos em um elemento de `Orders` dentro do elemento de `Customer` . O documento original também contém um elemento de `CustomerID` no elemento de `Order` ; este elemento será removido do documento reformulado.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](./sample-xml-file-customers-and-orders-linq-to-xml-2.md).  
  
```csharp  
XElement co = XElement.Load("CustomersOrders.xml");  
XElement newCustOrd =  
    new XElement("Root",  
        from cust in co.Element("Customers").Elements("Customer")  
        select new XElement("Customer",  
            cust.Attributes(),  
            cust.Elements(),  
            new XElement("Orders",  
                from ord in co.Element("Orders").Elements("Order")  
                where (string)ord.Element("CustomerID") == (string)cust.Attribute("CustomerID")  
                select new XElement("Order",  
                    ord.Attributes(),  
                    ord.Element("EmployeeID"),  
                    ord.Element("OrderDate"),  
                    ord.Element("RequiredDate"),  
                    ord.Element("ShipInfo")  
                )  
            )  
        )  
    );  
Console.WriteLine(newCustOrd);  
```  
  
 Este código produz a seguinte saída:  
  
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
  . . .  
```  
  
## <a name="example"></a>Exemplo  
 Este exemplo renomeia alguns elementos e converte alguns atributos para elementos.  
  
 O código chama `ConvertAddress`, que retorna uma lista de objetos <xref:System.Xml.Linq.XElement> . O argumento para o método é uma consulta que determina o elemento complexo de `Address` onde o atributo de `Type` tem um valor de `"Shipping"`.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](./sample-xml-file-typical-purchase-order-linq-to-xml-1.md).  
  
```csharp  
static IEnumerable<XElement> ConvertAddress(XElement add)  
{  
    List<XElement> fragment = new List<XElement>() {  
        new XElement("NAME", (string)add.Element("Name")),  
        new XElement("STREET", (string)add.Element("Street")),  
        new XElement("CITY", (string)add.Element("City")),  
        new XElement("ST", (string)add.Element("State")),  
        new XElement("POSTALCODE", (string)add.Element("Zip")),  
        new XElement("COUNTRY", (string)add.Element("Country"))  
    };  
    return fragment;  
}  
  
static void Main(string[] args)  
{  
    XElement po = XElement.Load("PurchaseOrder.xml");  
    XElement newPo = new XElement("PO",  
        new XElement("ID", (string)po.Attribute("PurchaseOrderNumber")),  
        new XElement("DATE", (DateTime)po.Attribute("OrderDate")),  
        ConvertAddress(  
            (from el in po.Elements("Address")  
            where (string)el.Attribute("Type") == "Shipping"  
            select el)  
            .First()  
        )  
    );  
    Console.WriteLine(newPo);  
}  
```  
  
 Este código produz a seguinte saída:  
  
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
