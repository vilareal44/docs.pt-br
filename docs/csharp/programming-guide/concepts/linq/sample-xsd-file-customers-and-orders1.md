---
title: 'Arquivo de exemplo XSD: Clientes e ordens'
description: Esse arquivo XSD é usado em vários exemplos na documentação do LINQ to XML. É uma definição de esquema para um arquivo XML de exemplo que contém clientes e pedidos.
ms.date: 07/20/2015
ms.assetid: ef9911a3-7ac4-44fd-b36e-a0c0ad0a157d
ms.openlocfilehash: 2fd4ce6ebc4c9b5d222ac86fe8dc6746d9126da0
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302419"
---
# <a name="sample-xsd-file-customers-and-orders"></a>Arquivo de exemplo XSD: Clientes e ordens
O arquivo XSD a seguir é usado em vários exemplos na documentação do [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]. Este arquivo contém uma definição de esquema para o [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](./sample-xml-file-customers-and-orders-linq-to-xml-2.md). O esquema usa os recursos `xs:key` e `xs:keyref` de XSD para estabelecer que o atributo `CustomerID` do elemento `Customer` é uma chave e para estabelecer uma relação entre o elemento `CustomerID` em cada elemento `Order` e o atributo `CustomerID` em cada elemento `Customer`.  
  
 Para obter um exemplo de como escrever consultas LINQ que aproveitam essa relação usando a `Join` cláusula, consulte [como unir duas coleções (LINQ to XML) (C#)](./how-to-join-two-collections-linq-to-xml.md).  
  
## <a name="customersordersxsd"></a>CustomersOrders.xsd  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:element name='Root'>  
    <xs:complexType>  
      <xs:sequence>  
        <xs:element name='Customers'>  
          <xs:complexType>  
            <xs:sequence>  
              <xs:element name='Customer' type='CustomerType' minOccurs='0' maxOccurs='unbounded' />  
            </xs:sequence>  
          </xs:complexType>  
        </xs:element>  
        <xs:element name='Orders'>  
          <xs:complexType>  
            <xs:sequence>  
              <xs:element name='Order' type='OrderType' minOccurs='0' maxOccurs='unbounded' />  
            </xs:sequence>  
          </xs:complexType>  
        </xs:element>  
      </xs:sequence>  
    </xs:complexType>  
    <xs:key name='CustomerIDKey'>  
      <xs:selector xpath='Customers/Customer'/>  
      <xs:field xpath='@CustomerID'/>  
    </xs:key>  
    <xs:keyref name='CustomerIDKeyRef' refer='CustomerIDKey'>  
      <xs:selector xpath='Orders/Order'/>  
      <xs:field xpath='CustomerID'/>  
    </xs:keyref>  
  </xs:element>  
  <xs:complexType name='CustomerType'>  
    <xs:sequence>  
      <xs:element name='CompanyName' type='xs:string'/>  
      <xs:element name='ContactName' type='xs:string'/>  
      <xs:element name='ContactTitle' type='xs:string'/>  
      <xs:element name='Phone' type='xs:string'/>  
      <xs:element name='Fax' minOccurs='0' type='xs:string'/>  
      <xs:element name='FullAddress' type='AddressType'/>  
    </xs:sequence>  
    <xs:attribute name='CustomerID' type='xs:token'/>  
  </xs:complexType>  
  <xs:complexType name='AddressType'>  
    <xs:sequence>  
      <xs:element name='Address' type='xs:string'/>  
      <xs:element name='City' type='xs:string'/>  
      <xs:element name='Region' type='xs:string'/>  
      <xs:element name='PostalCode' type='xs:string' />  
      <xs:element name='Country' type='xs:string'/>  
    </xs:sequence>  
    <xs:attribute name='CustomerID' type='xs:token'/>  
  </xs:complexType>  
  <xs:complexType name='OrderType'>  
    <xs:sequence>  
      <xs:element name='CustomerID' type='xs:token'/>  
      <xs:element name='EmployeeID' type='xs:token'/>  
      <xs:element name='OrderDate' type='xs:dateTime'/>  
      <xs:element name='RequiredDate' type='xs:dateTime'/>  
      <xs:element name='ShipInfo' type='ShipInfoType'/>  
    </xs:sequence>  
  </xs:complexType>  
  <xs:complexType name='ShipInfoType'>  
    <xs:sequence>  
      <xs:element name='ShipVia' type='xs:integer'/>  
      <xs:element name='Freight' type='xs:decimal'/>  
      <xs:element name='ShipName' type='xs:string'/>  
      <xs:element name='ShipAddress' type='xs:string'/>  
      <xs:element name='ShipCity' type='xs:string'/>  
      <xs:element name='ShipRegion' type='xs:string'/>  
      <xs:element name='ShipPostalCode' type='xs:string'/>  
      <xs:element name='ShipCountry' type='xs:string'/>  
    </xs:sequence>  
    <xs:attribute name='ShippedDate' type='xs:dateTime'/>  
  </xs:complexType>  
</xs:schema>  
```  
