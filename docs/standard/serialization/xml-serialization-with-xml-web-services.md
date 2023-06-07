---
title: Serialização XML com Serviços Web XML
description: Saiba mais sobre a serialização de XML como o mecanismo de transporte usado na arquitetura de serviços Web XML. A serialização XML é executada pela classe XmlSerializer.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML Web services, XML serialization
- XML serialization, XML Web services
- SOAP, XML serialization
- asmx files
- serialization, SOAP
- XML serialization, attributes
- attributes [.NET Framework], XML serialization
- .asmx files
- encoded XML serialization
- literal XML serialization
- serialization, attributes
ms.assetid: a416192f-8102-458e-bc0a-0b8f3f784da9
ms.openlocfilehash: b03c25f745df9aa4afe44075506983cb14ed3da7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84288947"
---
# <a name="xml-serialization-with-xml-web-services"></a>Serialização XML com Serviços Web XML
A serialização XML é o mecanismo de transporte subjacente utilizado na arquitetura de serviços Web XML, executado pela classe <xref:System.Xml.Serialization.XmlSerializer>. Para controlar o XML gerado por um serviço Web XML, aplique os atributos listados em [Atributos que controlam a serialização XML](attributes-that-control-xml-serialization.md) e [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md) às classes, aos valores retornados, aos parâmetros e aos campos de um arquivo usado para criar um serviço Web XML (.asmx). Para obter mais informações sobre como criar um serviço Web XML, consulte [XML Web Services Using ASP.net](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ba0z6a33(v=vs.100)).  
  
## <a name="literal-and-encoded-styles"></a>Estilos literais e codificados  
 O XML gerado por um serviço Web XML pode ser formatado em um de dois modos, seja literal ou codificado, conforme explicado em [Personalizando a formatação da mensagem SOAP](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dkwy2d72(v=vs.100)). Portanto, há dois conjuntos de atributos que controlam a serialização XML. Os atributos listados em [Atributos que controlam a serialização XML](attributes-that-control-xml-serialization.md) foram projetados para controlar o XML de estilo literal. Os atributos listados em [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md) controlam o estilo codificado. Aplicando seletivamente esses atributos, você pode personalizar um aplicativo para retornar qualquer um ou ambos os estilos. Além disso, esses atributos podem ser aplicados (conforme apropriado) a valores e parâmetros de retorno.  
  
### <a name="example-of-using-both-styles"></a>Exemplo de como usar os dois estilos  
 Quando estiver criando um serviço Web XML, você pode usar os dois conjuntos de atributos nos métodos. No exemplo de código a seguir, a classe denominada `MyService` contém dois métodos de serviço Web XML, `MyLiteralMethod` e `MyEncodedMethod`. Os dois métodos executam a mesma função: retornando uma instância da classe `Order`. Na classe `Order`, os atributos <xref:System.Xml.Serialization.XmlTypeAttribute> e <xref:System.Xml.Serialization.SoapTypeAttribute> são aplicados ao campo `OrderID` e ambos têm sua propriedade `ElementName` definida com valores diferentes.  
  
 Para executar o exemplo, cole o código em um arquivo com uma extensão .asmx e coloque o arquivo em um diretório virtual gerenciado pelo IIS (Serviços de Informações da Internet). Em um navegador HTML, como o Internet Explorer, digite o nome do computador, o diretório virtual e o arquivo.  
  
```vb  
<%@ WebService Language="VB" Class="MyService" %>  
Imports System  
Imports System.Web.Services  
Imports System.Web.Services.Protocols  
Imports System.Xml.Serialization  
Public Class Order  
    ' Both types of attributes can be applied. Depending on which type  
    ' the method used, either one will affect the call.  
    <SoapElement(ElementName:= "EncodedOrderID"), _  
    XmlElement(ElementName:= "LiteralOrderID")> _  
    public OrderID As String  
End Class  
  
Public Class MyService  
    <WebMethod, SoapDocumentMethod> _  
    public Function MyLiteralMethod() As Order
        Dim myOrder As Order = New Order()  
        return myOrder  
    End Function  
    <WebMethod, SoapRpcMethod> _  
    public Function MyEncodedMethod() As Order
        Dim myOrder As Order = New Order()  
        return myOrder  
    End Function  
End Class  
```  
  
```csharp  
<%@ WebService Language="C#" Class="MyService" %>  
using System;  
using System.Web.Services;  
using System.Web.Services.Protocols;  
using System.Xml.Serialization;  
public class Order {  
    // Both types of attributes can be applied. Depending on which type  
    // the method used, either one will affect the call.  
    [SoapElement(ElementName = "EncodedOrderID")]  
    [XmlElement(ElementName = "LiteralOrderID")]  
    public String OrderID;  
}  
public class MyService {  
    [WebMethod][SoapDocumentMethod]  
    public Order MyLiteralMethod(){  
        Order myOrder = new Order();  
        return myOrder;  
    }  
    [WebMethod][SoapRpcMethod]  
    public Order MyEncodedMethod(){  
        Order myOrder = new Order();  
        return myOrder;  
    }  
}  
```  
  
 O exemplo de código a seguir chama `MyLiteralMethod`. O nome do elemento é alterado para "LiteralOrderID".  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethodResponse xmlns="http://tempuri.org/">  
            <MyLiteralMethodResult>  
                <LiteralOrderID>string</LiteralOrderID>  
            </MyLiteralMethodResult>  
        </MyLiteralMethodResponse>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 O exemplo de código a seguir chama `MyEncodedMethod`. O nome do elemento é "EncodedOrderID".  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/" xmlns:tns="http://tempuri.org/" xmlns:types="http://tempuri.org/encodedTypes" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body soap:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">  
        <tns:MyEncodedMethodResponse>  
            <MyEncodedMethodResult href="#id1" />  
        </tns:MyEncodedMethodResponse>  
        <types:Order id="id1" xsi:type="types:Order">  
            <EncodedOrderID xsi:type="xsd:string">string</EncodedOrderID>  
        </types:Order>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="applying-attributes-to-return-values"></a>Aplicando atributos a valores de retorno  
 Você também pode aplicar atributos a valores de retorno para controlar o namespace, o nome do elemento e assim por diante. O exemplo de código a seguir aplica o atributo `XmlElementAttribute` ao valor de retorno do método `MyLiteralMethod`. Fazer isso permite controlar o namespace e o nome do elemento.  
  
```vb  
<WebMethod, SoapDocumentMethod> _  
public Function MyLiteralMethod() As _  
<XmlElement(Namespace:="http://www.cohowinery.com", _  
ElementName:= "BookOrder")> _  
Order
    Dim myOrder As Order = New Order()  
    return myOrder  
End Function  
```  
  
```csharp  
[return: XmlElement(Namespace = "http://www.cohowinery.com",  
ElementName = "BookOrder")]  
[WebMethod][SoapDocumentMethod]  
public Order MyLiteralMethod(){  
    Order myOrder = new Order();  
    return myOrder;  
}  
```  
  
 Quando chamado, o código retorna XML semelhante ao seguinte.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethodResponse xmlns="http://tempuri.org/">  
            <BookOrder xmlns="http://www.cohowinery.com">  
                <LiteralOrderID>string</LiteralOrderID>  
            </BookOrder>  
        </MyLiteralMethodResponse>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="attributes-applied-to-parameters"></a>Atributos aplicados a parâmetros  
 Você também pode aplicar atributos a parâmetros para especificar o namespace, o nome do elemento e assim por diante. O exemplo de código a seguir adiciona um parâmetro ao método `MyLiteralMethodResponse` e aplica o atributo `XmlAttributeAttribute` ao parâmetro. O nome e o namespace do elemento são definidos para o parâmetro.  
  
```vb  
<WebMethod, SoapDocumentMethod> _  
public Function MyLiteralMethod(<XmlElement _  
("MyOrderID", Namespace:="http://www.microsoft.com")>ID As String) As _  
<XmlElement(Namespace:="http://www.cohowinery.com", _  
ElementName:= "BookOrder")> _  
Order
    Dim myOrder As Order = New Order()  
    myOrder.OrderID = ID  
    return myOrder  
End Function  
```  
  
```csharp  
[return: XmlElement(Namespace = "http://www.cohowinery.com",  
ElementName = "BookOrder")]  
[WebMethod][SoapDocumentMethod]  
public Order MyLiteralMethod([XmlElement("MyOrderID",
Namespace="http://www.microsoft.com")] string ID){  
    Order myOrder = new Order();  
    myOrder.OrderID = ID;  
    return myOrder;  
}
```  
  
 A solicitação SOAP deve ser semelhante ao seguinte.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
    <soap:Body>  
        <MyLiteralMethod xmlns="http://tempuri.org/">  
            <MyOrderID xmlns="http://www.microsoft.com">string</MyOrderID>  
        </MyLiteralMethod>  
    </soap:Body>  
</soap:Envelope>  
```  
  
### <a name="applying-attributes-to-classes"></a>Aplicando atributos a classes  
 Se você precisar controlar o namespace dos elementos que são correlacionados a classes, poderá aplicar `XmlTypeAttribute`, `XmlRootAttribute` e `SoapTypeAttribute`, conforme apropriado. O exemplo de código a seguir aplica todos os três à classe `Order`.  
  
```vb  
<XmlType("BigBookService"), _  
SoapType("SoapBookService"), _  
XmlRoot("BookOrderForm")> _  
Public Class Order  
    ' Both types of attributes can be applied. Depending on which  
    ' the method used, either one will affect the call.  
    <SoapElement(ElementName:= "EncodedOrderID"), _  
    XmlElement(ElementName:= "LiteralOrderID")> _  
    public OrderID As String  
End Class  
```  
  
```csharp  
[XmlType("BigBooksService", Namespace = "http://www.cpandl.com")]  
[SoapType("SoapBookService")]  
[XmlRoot("BookOrderForm")]  
public class Order {  
    // Both types of attributes can be applied. Depending on which  
    // the method used, either one will affect the call.  
    [SoapElement(ElementName = "EncodedOrderID")]  
    [XmlElement(ElementName = "LiteralOrderID")]  
    public String OrderID;  
}  
```  
  
 Os resultados da aplicação de `XmlTypeAttribute` e de `SoapTypeAttribute` podem ser vistos quando você examina a descrição do serviço, conforme mostrado no exemplo de código a seguir.  
  
```xml
<s:element name="BookOrderForm" type="s0:BigBookService" />
<s:complexType name="BigBookService">
  <s:sequence>
    <s:element minOccurs="0" maxOccurs="1" name="LiteralOrderID" type="s:string" />
  </s:sequence>

  <s:schema targetNamespace="http://tempuri.org/encodedTypes">
    <s:complexType name="SoapBookService">
      <s:sequence>
        <s:element minOccurs="1" maxOccurs="1" name="EncodedOrderID" type="s:string" />
      </s:sequence>
    </s:complexType>
  </s:schema>
</s:complexType>
```
  
 O efeito do `XmlRootAttribute` também pode ser visto nos resultados de HTTP GET e HTTP POST, da seguinte maneira.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<BookOrderForm xmlns="http://tempuri.org/">  
    <LiteralOrderID>string</LiteralOrderID>  
</BookOrderForm>  
```  
  
## <a name="see-also"></a>Confira também

- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md)
- [Como serializar um objeto como um fluxo XML codificado para SOAP](how-to-serialize-an-object-as-a-soap-encoded-xml-stream.md)
- [Como substituir a serialização XML de SOAP codificada](how-to-override-encoded-soap-xml-serialization.md)
- [Apresentando a serialização XML](introducing-xml-serialization.md)
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
