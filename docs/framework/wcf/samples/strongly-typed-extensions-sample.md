---
title: Exemplo de extensões com rigidez de tipos
ms.date: 03/30/2017
ms.assetid: 02220f11-1a83-441c-9e5a-85f9a9367572
ms.openlocfilehash: e8c3bf202a1fb76d383f0a3fe15084d19a1d51fb
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600874"
---
# <a name="strongly-typed-extensions-sample"></a>Exemplo de extensões com rigidez de tipos

O exemplo usa a <xref:System.ServiceModel.Syndication.SyndicationFeed> classe para os fins do exemplo. No entanto, os padrões demonstrados neste exemplo podem ser usados com todas as classes de distribuição que dão suporte a dados de extensão.  
  
 O modelo de objeto de distribuição ( <xref:System.ServiceModel.Syndication.SyndicationFeed> , <xref:System.ServiceModel.Syndication.SyndicationItem> , e classes relacionadas) dá suporte ao acesso de tipo inflexível para dados de extensão usando as <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> Propriedades e. Este exemplo mostra como fornecer acesso com rigidez de tipos a dados de extensão implementando classes derivadas personalizadas do e disponibilizando <xref:System.ServiceModel.Syndication.SyndicationFeed> <xref:System.ServiceModel.Syndication.SyndicationItem> determinadas extensões específicas do aplicativo como propriedades fortemente tipadas.  
  
 Por exemplo, este exemplo mostra como implementar um elemento de extensão definido na RFC de extensões de Threading Atom propostas. Isso é apenas para fins de demonstração e este exemplo não se destina a ser uma implementação completa da especificação proposta.  
  
## <a name="sample-xml"></a>XML de exemplo  
 O exemplo de XML a seguir mostra uma entrada Atom 1,0 com um `<in-reply-to>` elemento de extensão adicional.  
  
```xml  
<entry>  
    <id>tag:example.org,2005:1,2</id>  
    <title type="text">Another response to the original</title>  
    <summary type="text">  
         This is a response to the original entry</summary>  
    <updated>2006-03-01T12:12:13Z</updated>  
    <link href="http://www.example.org/entries/1/2" />  
    <in-reply-to p3:ref="tag:example.org,2005:1"
                 p3:href="http://www.example.org/entries/1"
                 p3:type="application/xhtml+xml"
                 xmlns:p3="http://contoso.org/syndication/thread/1.0"
                 xmlns="http://contoso.org/syndication/thread/1.0">  
      <anotherElement xmlns="http://www.w3.org/2005/Atom">  
                     Some more data</anotherElement>  
      <aDifferentElement xmlns="http://www.w3.org/2005/Atom">  
                     Even more data</aDifferentElement>  
    </in-reply-to>  
</entry>  
```  
  
 O `<in-reply-to>` elemento especifica três atributos necessários ( `ref` `type` e `href` ), enquanto também permite a presença de atributos de extensão adicionais e elementos de extensão.  
  
## <a name="modeling-the-in-reply-to-element"></a>Modelando o elemento in-reply-to  
 Neste exemplo, o `<in-reply-to>` elemento é modelado como CLR que implementa <xref:System.Xml.Serialization.IXmlSerializable> , que permite seu uso com o <xref:System.Runtime.Serialization.DataContractSerializer> . Ele também implementa alguns métodos e propriedades para acessar os dados do elemento, conforme mostrado no código de exemplo a seguir.  
  
```csharp  
[XmlRoot(ElementName = "in-reply-to", Namespace = "http://contoso.org/syndication/thread/1.0")]  
public class InReplyToElement : IXmlSerializable  
{  
    internal const string ElementName = "in-reply-to";  
    internal const string NsUri =
                  "http://contoso.org/syndication/thread/1.0";  
    private Dictionary<XmlQualifiedName, string> extensionAttributes;  
    private Collection<XElement> extensionElements;  
  
    public InReplyToElement()  
    {  
        this.extensionElements = new Collection<XElement>();  
        this.extensionAttributes = new Dictionary<XmlQualifiedName,
                                                          string>();  
    }  
  
    public Dictionary<XmlQualifiedName, string> AttributeExtensions  
    {  
        get { return this.extensionAttributes; }  
    }  
  
    public Collection<XElement> ElementExtensions  
    {  
        get { return this.extensionElements; }  
    }  
  
    public Uri Href  
    { get; set; }  
  
    public string MediaType  
    { get; set; }  
  
    public string Ref  
    { get; set; }  
  
    public Uri Source  
    { get; set; }  
}  
```  
  
 A `InReplyToElement` classe implementa propriedades para o atributo necessário ( `HRef` , `MediaType` , e `Source` ), bem como coleções para manter <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> e <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> .  
  
 A `InReplyToElement` classe implementa a <xref:System.Xml.Serialization.IXmlSerializable> interface, que permite o controle direto sobre como as instâncias de objeto são lidas e gravadas em XML. O `ReadXml` método primeiro lê os valores para as `Ref` `HRef` Propriedades,, e `Source` `MediaType` do <xref:System.Xml.XmlReader> passado para ele. Todos os atributos desconhecidos são armazenados na <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> coleção. Quando todos os atributos foram lidos, <xref:System.Xml.XmlReader.ReadStartElement> é chamado para avançar o leitor para o próximo elemento. Como o elemento modelado por essa classe não tem filhos necessários, os elementos filho são armazenados em buffer em `XElement` instâncias e armazenadas na <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> coleção, conforme mostrado no código a seguir.  
  
```csharp
public void ReadXml(System.Xml.XmlReader reader)  
{  
    bool isEmpty = reader.IsEmptyElement;  
  
    if (reader.HasAttributes)  
    {  
        for (int i = 0; i < reader.AttributeCount; i++)  
        {  
            reader.MoveToNextAttribute();  
  
            if (reader.NamespaceURI == "")  
            {  
                if (reader.LocalName == "ref")  
                {  
                    this.Ref = reader.Value;  
                }  
                else if (reader.LocalName == "href")  
                {  
                    this.Href = new Uri(reader.Value);  
                }  
                else if (reader.LocalName == "source")  
                {  
                    this.Source = new Uri(reader.Value);  
                }  
                else if (reader.LocalName == "type")  
                {  
                    this.MediaType = reader.Value;  
                }  
                else  
                {  
                    this.AttributeExtensions.Add(new
                                 XmlQualifiedName(reader.LocalName,
                                 reader.NamespaceURI),
                                 reader.Value);  
                }  
            }  
        }  
    }  
  
    reader.ReadStartElement();  
  
    if (!isEmpty)  
    {  
        while (reader.IsStartElement())  
        {  
            ElementExtensions.Add(  
                  (XElement) XElement.ReadFrom(reader));  
        }  
        reader.ReadEndElement();  
    }  
}  
```  
  
 No `WriteXml` , o `InReplyToElement` método primeiro escreve os valores das `Ref` Propriedades,, `HRef` `Source` e `MediaType` como atributos XML ( `WriteXml` não é responsável por escrever o próprio elemento externo real, como feito pelo chamador de `WriteXml` ). Ele também grava o conteúdo do <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> e <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> para o gravador, conforme mostrado no código a seguir.  
  
```csharp
public void WriteXml(System.Xml.XmlWriter writer)  
{  
    if (this.Ref != null)  
    {  
        writer.WriteAttributeString("ref", InReplyToElement.NsUri,
                                            this.Ref);  
    }  
    if (this.Href != null)  
    {  
        writer.WriteAttributeString("href", InReplyToElement.NsUri,
                                                this.Href.ToString());  
    }  
    if (this.Source != null)  
    {  
        writer.WriteAttributeString("source", InReplyToElement.NsUri,
                                              this.Source.ToString());  
    }  
    if (this.MediaType != null)  
    {  
        writer.WriteAttributeString("type", InReplyToElement.NsUri,
                                                    this.MediaType);  
    }  
  
    foreach (KeyValuePair<XmlQualifiedName, string> kvp in
                                             this.AttributeExtensions)  
    {  
        writer.WriteAttributeString(kvp.Key.Name, kvp.Key.Namespace,
                                                   kvp.Value);  
    }  
  
    foreach (XElement element in this.ElementExtensions)  
    {  
        element.WriteTo(writer);  
    }  
}  
```  
  
## <a name="threadedfeed-and-threadeditem"></a>ThreadedFeed e ThreadedItem  
 No exemplo, `SyndicationItems` com `InReplyTo` extensões são modeladas pela `ThreadedItem` classe. Da mesma forma, a `ThreadedFeed` classe é uma `SyndicationFeed` cujos itens são todas as instâncias de `ThreadedItem` .  
  
 A `ThreadedFeed` classe é herdada de `SyndicationFeed` e substitui `OnCreateItem` para retornar um `ThreadedItem` . Ele também implementa um método para acessar a `Items` coleção como `ThreadedItems` , conforme mostrado no código a seguir.  
  
```csharp
public class ThreadedFeed : SyndicationFeed  
{  
    public ThreadedFeed()  
    {  
    }  
  
    public IEnumerable<ThreadedItem> ThreadedItems  
    {  
        get  
        {  
            return this.Items.Cast<ThreadedItem>();  
        }  
    }  
  
    protected override SyndicationItem CreateItem()  
    {  
        return new ThreadedItem();  
    }  
}  
```  
  
 A classe `ThreadedItem` herda de `SyndicationItem` e faz `InReplyToElement` como uma propriedade fortemente tipada. Isso fornece acesso de programação conveniente aos `InReplyTo` dados de extensão. Ele também implementa `TryParseElement` e `WriteElementExtensions` para ler e gravar seus dados de extensão, conforme mostrado no código a seguir.  
  
```csharp
public class ThreadedItem : SyndicationItem  
{  
    private InReplyToElement inReplyTo;  
    // Constructors  
        public ThreadedItem()  
        {  
            inReplyTo = new InReplyToElement();  
        }  
  
        public ThreadedItem(string title, string content, Uri itemAlternateLink, string id, DateTimeOffset lastUpdatedTime) : base(title, content, itemAlternateLink, id, lastUpdatedTime)  
        {  
            inReplyTo = new InReplyToElement();  
        }  
  
    public InReplyToElement InReplyTo  
    {  
        get { return this.inReplyTo; }  
    }  
  
    protected override bool TryParseElement(  
                        System.Xml.XmlReader reader,
                        string version)  
    {  
        if (version == SyndicationVersions.Atom10 &&  
            reader.NamespaceURI == InReplyToElement.NsUri &&  
            reader.LocalName == InReplyToElement.ElementName)  
        {  
            this.inReplyTo = new InReplyToElement();  
  
            this.InReplyTo.ReadXml(reader);  
  
            return true;  
        }  
        else  
        {  
            return base.TryParseElement(reader, version);  
        }  
    }  
  
    protected override void WriteElementExtensions(XmlWriter writer,
                                                 string version)  
    {  
        if (this.InReplyTo != null &&
                     version == SyndicationVersions.Atom10)  
        {  
            writer.WriteStartElement(InReplyToElement.ElementName,
                                           InReplyToElement.NsUri);  
            this.InReplyTo.WriteXml(writer);  
            writer.WriteEndElement();  
        }  
  
        base.WriteElementExtensions(writer, version);  
    }  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Syndication\StronglyTypedExtensions`  
