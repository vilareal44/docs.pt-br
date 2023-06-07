---
title: Como projetar um grafo de objeto (C#)
description: Saiba como projetar ou popular um grafo de objeto do XML em C#. O código de exemplo popula um grafo de objeto com classes de um arquivo XML de exemplo.
ms.date: 07/20/2015
ms.assetid: 293d15d5-3eaf-48de-9a02-3e13cb117b5b
ms.openlocfilehash: 5c2ea0e06d9a46a1b19236eae021a2a153fea1d4
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104619"
---
# <a name="how-to-project-an-object-graph-c"></a>Como projetar um grafo de objeto (C#)
Este tópico ilustra como projeto, ou preencher, um grafo de objeto XML.  
  
## <a name="example"></a>Exemplo  
 O código a seguir preenche um grafo de objeto com as classes `Address`, `PurchaseOrder` e `PurchaseOrderItem` do documento XML de [Arquivo XML de exemplo: pedido de compra típico (LINQ to XML)](./sample-xml-file-typical-purchase-order-linq-to-xml-1.md).  
  
```csharp  
class Address  
{  
    public enum AddressUse  
    {  
        Shipping,  
        Billing,  
    }  
  
    private AddressUse addressType;  
    private string name;  
    private string street;  
    private string city;  
    private string state;  
    private string zip;  
    private string country;  
  
    public AddressUse AddressType {  
        get { return addressType; } set { addressType = value; }  
    }  
  
    public string Name {  
        get { return name; } set { name = value; }  
    }  
  
    public string Street {  
        get { return street; } set { street = value; }  
    }  
  
    public string City {  
        get { return city; } set { city = value; }  
    }  
  
    public string State {  
        get { return state; } set { state = value; }  
    }  
  
    public string Zip {  
        get { return zip; } set { zip = value; }  
    }  
  
    public string Country {  
        get { return country; } set { country = value; }  
    }  
  
    public override string ToString()  
    {  
        StringBuilder sb = new StringBuilder();
        sb.Append($"Type: {(addressType == AddressUse.Shipping ? "Shipping" : "Billing")}\n");
        sb.Append($"Name: {name}\n");
        sb.Append($"Street: {street}\n");
        sb.Append($"City: {city}\n");
        sb.Append($"State: {state}\n");
        sb.Append($"Zip: {zip}\n");
        sb.Append($"Country: {country}\n");
        return sb.ToString();
    }  
}  
  
class PurchaseOrderItem  
{  
    private string partNumber;  
    private string productName;  
    private int quantity;  
    private Decimal usPrice;  
    private string comment;  
    private DateTime shipDate;  
  
    public string PartNumber {  
        get { return partNumber; } set { partNumber = value; }  
    }  
  
    public string ProductName {  
        get { return productName; } set { productName = value; }  
    }  
  
    public int Quantity {  
        get { return quantity; } set { quantity = value; }  
    }  
  
    public Decimal USPrice {  
        get { return usPrice; } set { usPrice = value; }  
    }  
  
    public string Comment {  
        get { return comment; } set { comment = value; }  
    }  
  
    public DateTime ShipDate {  
        get { return shipDate; } set { shipDate = value; }  
    }  
  
    public override string ToString()  
    {  
        StringBuilder sb = new StringBuilder();
        sb.Append($"PartNumber: {partNumber}\n");
        sb.Append($"ProductName: {productName}\n");
        sb.Append($"Quantity: {quantity}\n");
        sb.Append($"USPrice: {usPrice}\n");
        if (comment != null)
            sb.Append($"Comment: {comment}\n");
        if (shipDate != DateTime.MinValue)
            sb.Append($"ShipDate: {shipDate:d}\n");
        return sb.ToString();
    }  
}  
  
class PurchaseOrder  
{  
    private string purchaseOrderNumber;  
    private DateTime orderDate;  
    private string comment;  
    private List<Address> addresses;  
    private List<PurchaseOrderItem> items;  
  
    public string PurchaseOrderNumber {  
        get { return purchaseOrderNumber; } set { purchaseOrderNumber = value; }  
    }  
  
    public DateTime OrderDate {  
        get { return orderDate; } set { orderDate = value; }  
    }  
  
    public string Comment {  
        get { return comment; } set { comment = value; }  
    }  
  
    public List<Address> Addresses {  
        get { return addresses; } set { addresses = value; }  
    }  
  
    public List<PurchaseOrderItem> Items {  
        get { return items; } set { items = value; }  
    }  
  
    public override string ToString()  
    {  
        StringBuilder sb = new StringBuilder();
        sb.Append($"PurchaseOrderNumber: {purchaseOrderNumber}\n");
        sb.Append($"OrderDate: {orderDate:d}\n");
        sb.Append("\n");
        sb.Append("Addresses\n");
        sb.Append("=====\n");
        foreach (Address address in addresses)
        {
            sb.Append(address);
            sb.Append("\n");
        }
        sb.Append("Items\n");
        sb.Append("=====\n");
        foreach (PurchaseOrderItem item in items)
        {
            sb.Append(item);
            sb.Append("\n");
        }
        return sb.ToString();
    }  
}  
  
class Program {  
    public static void Main()  
    {  
        XElement po = XElement.Load("PurchaseOrder.xml");  
        PurchaseOrder purchaseOrder = new PurchaseOrder {  
            PurchaseOrderNumber = (string)po.Attribute("PurchaseOrderNumber"),  
            OrderDate = (DateTime)po.Attribute("OrderDate"),  
            Addresses = (  
                            from a in po.Elements("Address")  
                            select new Address {  
                                AddressType = ((string)a.Attribute("Type") == "Shipping") ?  
                                    Address.AddressUse.Shipping :
                                    Address.AddressUse.Billing,  
                                Name = (string)a.Element("Name"),  
                                Street = (string)a.Element("Street"),  
                                City = (string)a.Element("City"),  
                                State = (string)a.Element("State"),  
                                Zip = (string)a.Element("Zip"),  
                                Country = (string)a.Element("Country")  
                            }  
                        ).ToList(),  
            Items = (  
                        from i in po.Element("Items").Elements("Item")  
                        select new PurchaseOrderItem {  
                            PartNumber = (string)i.Attribute("PartNumber"),  
                            ProductName = (string)i.Element("ProductName"),  
                            Quantity = (int)i.Element("Quantity"),  
                            USPrice = (Decimal)i.Element("USPrice"),  
                            Comment = (string)i.Element("Comment"),  
                            ShipDate = (i.Element("ShipDate") != null) ?  
                                (DateTime)i.Element("ShipDate") :  
                                DateTime.MinValue  
                        }  
                    ).ToList()  
        };  
        Console.WriteLine(purchaseOrder);  
    }  
}  
```  
  
 Neste exemplo, o resultado da consulta LINQ é retornado como um <xref:System.Collections.Generic.IEnumerable%601> de `PurchaseOrderItem` . Os itens na classe de `PurchaseOrder` são do tipo <xref:System.Collections.Generic.IEnumerable%601> de `PurchaseOrderItem`. O código usa o método de extensão de <xref:System.Linq.Enumerable.ToList%2A> para criar uma coleção de <xref:System.Collections.Generic.List%601> dos resultados da consulta.  
  
 O exemplo produz a seguinte saída:  
  
```output  
PurchaseOrderNumber: 99503  
OrderDate: 10/20/1999  
  
Addresses  
=====  
Type: Shipping  
Name: Ellen Adams  
Street: 123 Maple Street  
City: Mill Valley  
State: CA  
Zip: 10999  
Country: USA  
  
Type: Billing  
Name: Tai Yee  
Street: 8 Oak Avenue  
City: Old Town  
State: PA  
Zip: 95819  
Country: USA  
  
Items  
=====  
PartNumber: 872-AA  
ProductName: Lawnmower  
Quantity: 1  
USPrice: 148.95  
Comment: Confirm this is electric  
  
PartNumber: 926-AA  
ProductName: Baby Monitor  
Quantity: 2  
USPrice: 39.98  
ShipDate: 5/21/1999  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq.Enumerable.Select%2A>
- <xref:System.Linq.Enumerable.ToList%2A>
