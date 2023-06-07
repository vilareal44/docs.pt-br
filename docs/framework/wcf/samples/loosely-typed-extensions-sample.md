---
title: Exemplo de extensões tipadas vagamente
ms.date: 03/30/2017
ms.assetid: 56ce265b-8163-4b85-98e7-7692a12c4357
ms.openlocfilehash: 0a08ca19e5e6bff7223d45726617d2c2163ca3df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591859"
---
# <a name="loosely-typed-extensions-sample"></a>Exemplo de extensões tipadas vagamente
O modelo de objeto de distribuição fornece suporte avançado para trabalhar com dados de extensão — informações que estão presentes na representação XML de um feed de agregação, mas não são explicitamente expostos por classes como <xref:System.ServiceModel.Syndication.SyndicationFeed> e <xref:System.ServiceModel.Syndication.SyndicationItem> . Este exemplo ilustra as técnicas básicas para trabalhar com dados de extensão.  
  
 O exemplo usa a <xref:System.ServiceModel.Syndication.SyndicationFeed> classe para os fins do exemplo. No entanto, os padrões demonstrados neste exemplo podem ser usados com todas as classes de distribuição que dão suporte a dados de extensão:  
  
 <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
 <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
 <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
 <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
## <a name="sample-xml"></a>XML de exemplo  
 Para referência, o documento XML a seguir é usado neste exemplo.  
  
```xml  
<?xml version="1.0" encoding="IBM437"?>  
<feed myAttribute="someValue" xmlns="http://www.w3.org/2005/Atom">  
  <title type="text"></title>  
  <id>uuid:8f60c7b3-a3c0-4de7-a642-2165d77ce3c1;id=1</id>  
  <updated>2007-09-07T22:15:34Z</updated>  
  <simpleString xmlns="">hello, world!</simpleString>  
  <simpleString xmlns="">another simple string</simpleString>  
  <DataContractExtension xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.d  
atacontract.org/2004/07/Microsoft.Syndication.Samples">  
    <Key>X</Key>  
    <Value>4</Value>  
  </DataContractExtension>  
  <XmlSerializerExtension xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://ww  
w.w3.org/2001/XMLSchema" xmlns="">  
    <Key>Y</Key>  
    <Value>8</Value>  
  </XmlSerializerExtension>  
  <xElementExtension xmlns="">  
    <Key attr1="someValue">Z</Key>  
    <Value attr1="someValue">15</Value>  
  </xElementExtension>  
</feed>  
```  
  
 Este documento contém as seguintes partes de dados de extensão:  
  
- O atributo `myAttribute` do elemento `<feed>`.  
  
- Elemento `<simpleString>`.  
  
- Elemento `<DataContractExtension>`.  
  
- Elemento `<XmlSerializerExtension>`.  
  
- Elemento `<xElementExtension>`.  
  
## <a name="writing-extension-data"></a>Gravando dados de extensão  
 As extensões de atributo são criadas adicionando entradas à <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> coleção, conforme mostrado no código de exemplo a seguir.  
  
```csharp  
//Attribute extensions are stored in a dictionary indexed by
// XmlQualifiedName  
feed.AttributeExtensions.Add(new XmlQualifiedName("myAttribute", ""), "someValue");  
```  
  
 As extensões de elemento são criadas adicionando entradas à <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> coleção. Essas extensões podem ser por valores básicos, como cadeias de caracteres, serializações XML de objetos .NET Framework ou nós XML codificados manualmente.  
  
 O código de exemplo a seguir cria um elemento de extensão chamado `simpleString` .  
  
```csharp  
feed.ElementExtensions.Add("simpleString", "", "hello, world!");  
```  
  
 O namespace XML para esse elemento é o namespace vazio ("") e seu valor é um nó de texto que contém a cadeia de caracteres "Olá, mundo!".  
  
 Uma maneira de criar extensões de elemento complexas que consistem em muitos elementos aninhados é usar as APIs de .NET Framework para serialização (tanto <xref:System.Runtime.Serialization.DataContractSerializer> quanto as <xref:System.Xml.Serialization.XmlSerializer> têm suporte), conforme mostrado nos exemplos a seguir.  
  
```csharp  
feed.ElementExtensions.Add( new DataContractExtension() { Key = "X", Value = 4 } );  
feed.ElementExtensions.Add( new XmlSerializerExtension { Key = "Y", Value = 8 }, new XmlSerializer( typeof( XmlSerializerExtension ) ) );  
```  
  
 Neste exemplo, `DataContractExtension` e `XmlSerializerExtension` são tipos personalizados escritos para uso com um serializador.  
  
 A <xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection> classe também pode ser usada para criar extensões de elemento de uma <xref:System.Xml.XmlReader> instância do. Isso permite uma fácil integração com APIs de processamento XML, como <xref:System.Xml.Linq.XElement> mostrado no código de exemplo a seguir.  
  
```csharp  
feed.ElementExtensions.Add(new XElement("xElementExtension",  
        new XElement("Key", new XAttribute("attr1", "someValue"), "Z"),  
        new XElement("Value", new XAttribute("attr1", "someValue"),
        "15")).CreateReader());  
```  
  
## <a name="reading-extension-data"></a>Lendo dados de extensão  
 Os valores para extensões de atributo podem ser obtidos pesquisando o atributo na <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> coleção por seu <xref:System.Xml.XmlQualifiedName> , conforme mostrado no código de exemplo a seguir.  
  
```csharp  
Console.WriteLine( feed.AttributeExtensions[ new XmlQualifiedName( "myAttribute", "" )]);  
```  
  
 As extensões de elemento são acessadas usando o `ReadElementExtensions<T>` método.  
  
```csharp  
foreach( string s in feed2.ElementExtensions.ReadElementExtensions<string>("simpleString", ""))  
{  
    Console.WriteLine(s);  
}  
  
foreach (DataContractExtension dce in feed2.ElementExtensions.ReadElementExtensions<DataContractExtension>("DataContractExtension",  
"http://schemas.datacontract.org/2004/07/SyndicationExtensions"))  
{  
    Console.WriteLine(dce.ToString());  
}  
  
foreach (XmlSerializerExtension xse in feed2.ElementExtensions.ReadElementExtensions<XmlSerializerExtension>("XmlSerializerExtension", "", new XmlSerializer(typeof(XmlSerializerExtension))))  
{  
    Console.WriteLine(xse.ToString());  
}  
```  
  
 Também é possível obter um `XmlReader` em extensões de elemento individuais usando o <xref:System.ServiceModel.Syndication.SyndicationElementExtension.GetReader> método.  
  
```csharp  
foreach (SyndicationElementExtension extension in feed2.ElementExtensions.Where<SyndicationElementExtension>(x => x.OuterName == "xElementExtension"))  
{  
    XNode xelement = XElement.ReadFrom(extension.GetReader());  
    Console.WriteLine(xelement.ToString());  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Syndication\LooselyTypedExtensions`  
  
## <a name="see-also"></a>Consulte também

- [Extensões com rigidez de tipos](strongly-typed-extensions-sample.md)
- [Sindicalização do WCF](../feature-details/wcf-syndication.md)
