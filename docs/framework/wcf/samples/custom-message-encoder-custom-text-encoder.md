---
title: 'Codificador de mensagem personalizado: Codificador de texto personalizado'
description: Use este exemplo para implementar um codificador de mensagem de texto personalizado usando o WCF. Esse codificador dá suporte a todas as codificações de caracteres com suporte da plataforma para interoperabilidade.
ms.date: 03/30/2017
ms.assetid: 68ff5c74-3d33-4b44-bcae-e1d2f5dea0de
ms.openlocfilehash: 88ddc79e6cc1df654aea851cedb0e60c6fbcd017
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246266"
---
# <a name="custom-message-encoder-custom-text-encoder"></a>Codificador de mensagem personalizado: Codificador de texto personalizado

Este exemplo demonstra como implementar um codificador de mensagem de texto personalizado usando o Windows Communication Foundation (WCF).

> [!WARNING]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Text`

O <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> do WCF dá suporte apenas às codificações Unicode UTF-8, UTF-16 e big-endian. O codificador de mensagem de texto personalizado neste exemplo dá suporte a todas as codificações de caracteres com suporte da plataforma que podem ser necessárias para interoperabilidade. O exemplo consiste em um programa de console do cliente (. exe), uma biblioteca de serviço (. dll) hospedada pelo Serviços de Informações da Internet (IIS) e uma biblioteca de codificador de mensagem de texto (. dll). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir). O cliente faz solicitações síncronas para uma determinada operação matemática e o serviço responde com o resultado. O cliente e o serviço usam o `CustomTextMessageEncoder` em vez do padrão <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> .

A implementação do codificador personalizado consiste em um alocador de codificador de mensagem, um codificador de mensagem, um elemento de ligação de codificação de mensagens e um manipulador de configuração e demonstra o seguinte:

- Criando um codificador personalizado e um alocador de codificador.

- Criando um elemento de associação para um codificador personalizado.

- Usando a configuração de associação personalizada para integrar elementos de associação personalizados.

- Desenvolver um manipulador de configuração personalizado para permitir a configuração de arquivo de um elemento de associação personalizado.

## <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Instale o ASP.NET 4,0 usando o comando a seguir.

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

3. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

## <a name="message-encoder-factory-and-the-message-encoder"></a>Alocador de codificador de mensagem e o codificador de mensagem

Quando o <xref:System.ServiceModel.ServiceHost> ou o canal do cliente é aberto, o componente de tempo de design `CustomTextMessageBindingElement` cria o `CustomTextMessageEncoderFactory` . A fábrica cria o `CustomTextMessageEncoder` . O codificador de mensagem opera no modo de streaming e no modo em buffer. Ele usa o <xref:System.Xml.XmlReader> e o <xref:System.Xml.XmlWriter> para ler e gravar as mensagens, respectivamente. Ao contrário dos leitores e gravadores XML otimizados do WCF que dão suporte apenas ao UTF-8, ao UTF-16 e ao Unicode big endian, esses leitores e gravadores dão suporte a todas as codificações com suporte da plataforma.

O exemplo de código a seguir mostra o CustomTextMessageEncoder.

```csharp
public class CustomTextMessageEncoder : MessageEncoder
{
    private CustomTextMessageEncoderFactory factory;
    private XmlWriterSettings writerSettings;
    private string contentType;

    public CustomTextMessageEncoder(CustomTextMessageEncoderFactory factory)
    {
        this.factory = factory;

        this.writerSettings = new XmlWriterSettings();
        this.writerSettings.Encoding = Encoding.GetEncoding(factory.CharSet);
        this.contentType = $"{this.factory.MediaType}; charset={this.writerSettings.Encoding.HeaderName}";
    }

    public override string ContentType
    {
        get
        {
            return this.contentType;
        }
    }

    public override string MediaType
    {
        get
        {
            return factory.MediaType;
        }
    }

    public override MessageVersion MessageVersion
    {
        get
        {
            return this.factory.MessageVersion;
        }
    }

    public override Message ReadMessage(ArraySegment<byte> buffer, BufferManager bufferManager, string contentType)
    {
        byte[] msgContents = new byte[buffer.Count];
        Array.Copy(buffer.Array, buffer.Offset, msgContents, 0, msgContents.Length);
        bufferManager.ReturnBuffer(buffer.Array);

        MemoryStream stream = new MemoryStream(msgContents);
        return ReadMessage(stream, int.MaxValue);
    }

    public override Message ReadMessage(Stream stream, int maxSizeOfHeaders, string contentType)
    {
        XmlReader reader = XmlReader.Create(stream);
        return Message.CreateMessage(reader, maxSizeOfHeaders, this.MessageVersion);
    }

    public override ArraySegment<byte> WriteMessage(Message message, int maxMessageSize, BufferManager bufferManager, int messageOffset)
    {
        MemoryStream stream = new MemoryStream();
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);
        message.WriteMessage(writer);
        writer.Close();

        byte[] messageBytes = stream.GetBuffer();
        int messageLength = (int)stream.Position;
        stream.Close();

        int totalLength = messageLength + messageOffset;
        byte[] totalBytes = bufferManager.TakeBuffer(totalLength);
        Array.Copy(messageBytes, 0, totalBytes, messageOffset, messageLength);

        ArraySegment<byte> byteArray = new ArraySegment<byte>(totalBytes, messageOffset, messageLength);
        return byteArray;
    }

    public override void WriteMessage(Message message, Stream stream)
    {
        XmlWriter writer = XmlWriter.Create(stream, this.writerSettings);
        message.WriteMessage(writer);
        writer.Close();
    }
}
```

O exemplo de código a seguir mostra como criar o alocador de codificador de mensagem.

```csharp
public class CustomTextMessageEncoderFactory : MessageEncoderFactory
{
    private MessageEncoder encoder;
    private MessageVersion version;
    private string mediaType;
    private string charSet;

    internal CustomTextMessageEncoderFactory(string mediaType, string charSet,
        MessageVersion version)
    {
        this.version = version;
        this.mediaType = mediaType;
        this.charSet = charSet;
        this.encoder = new CustomTextMessageEncoder(this);
    }

    public override MessageEncoder Encoder
    {
        get
        {
            return this.encoder;
        }
    }

    public override MessageVersion MessageVersion
    {
        get
        {
            return this.version;
        }
    }

    internal string MediaType
    {
        get
        {
            return this.mediaType;
        }
    }

    internal string CharSet
    {
        get
        {
            return this.charSet;
        }
    }
}
```

## <a name="message-encoding-binding-element"></a>Elemento de associação de codificação de mensagens

Os elementos de associação permitem a configuração da pilha de tempo de execução do WCF. Para usar o codificador de mensagem personalizado em um aplicativo WCF, é necessário um elemento de ligação que cria o alocador de codificador de mensagem com as configurações apropriadas no nível apropriado na pilha de tempo de execução.

O `CustomTextMessageBindingElement` deriva da <xref:System.ServiceModel.Channels.BindingElement> classe base e herda da <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> classe. Isso permite que outros componentes do WCF reconheçam esse elemento de associação como sendo um elemento de associação de codificação de mensagens. A implementação de <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A> retorna uma instância do alocador de codificador de mensagem correspondente com as configurações apropriadas.

O `CustomTextMessageBindingElement` expõe configurações para `MessageVersion` , `ContentType` e `Encoding` por meio de propriedades. O codificador dá suporte às versões Soap11Addressing e Soap12Addressing1. O padrão é Soap11Addressing1. O valor padrão de `ContentType` é "text/xml". A `Encoding` propriedade permite que você defina o valor da codificação de caracteres desejada. O cliente de exemplo e o serviço usam a codificação de caracteres ISO-8859-1 (Latino1), que não tem suporte no <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> do WCF.

O código a seguir mostra como criar programaticamente a Associação usando o codificador de mensagem de texto personalizado.

```csharp
ICollection<BindingElement> bindingElements = new List<BindingElement>();
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();
CustomTextMessageBindingElement textBindingElement = new CustomTextMessageBindingElement();
bindingElements.Add(textBindingElement);
bindingElements.Add(httpBindingElement);
CustomBinding binding = new CustomBinding(bindingElements);
```

## <a name="adding-metadata-support-to-the-message-encoding-binding-element"></a>Adicionando suporte de metadados ao elemento de associação de codificação de mensagens

Qualquer tipo derivado de <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> é responsável por atualizar a versão da Associação SOAP no documento WSDL gerado para o serviço. Isso é feito implementando o `ExportEndpoint` método na <xref:System.ServiceModel.Description.IWsdlExportExtension> interface e, em seguida, modificando o WSDL gerado. Neste exemplo, o `CustomTextMessageBindingElement` usa a lógica de exportação WSDL do `TextMessageEncodingBindingElement` .

Para este exemplo, a configuração do cliente é configurar mão. Você não pode usar Svcutil.exe para gerar a configuração do cliente porque o `CustomTextMessageBindingElement` não exporta uma declaração de política para descrever seu comportamento. Em geral, você deve implementar a <xref:System.ServiceModel.Description.IPolicyExportExtension> interface em um elemento de associação personalizado para exportar uma declaração de política personalizada que descreve o comportamento ou a funcionalidade implementada pelo elemento de associação. Para obter um exemplo de como exportar uma declaração de política para um elemento de associação personalizado, consulte o exemplo [Transport: UDP](transport-udp.md) .

## <a name="message-encoding-binding-configuration-handler"></a>Manipulador de configuração de associação de codificação de mensagens
A seção anterior mostra como usar o codificador de mensagem de texto personalizado programaticamente. O `CustomTextMessageEncodingBindingSection` implementa um manipulador de configuração que permite que você especifique o uso de um codificador de mensagem de texto personalizado dentro de um arquivo de configuração. A `CustomTextMessageEncodingBindingSection` classe deriva da <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> classe. A `BindingElementType` propriedade informa o sistema de configuração do tipo de elemento de associação a ser criado para esta seção.

Todas as configurações definidas pelo `CustomTextMessageBindingElement` são expostas como as propriedades no `CustomTextMessageEncodingBindingSection` . O <xref:System.Configuration.ConfigurationPropertyAttribute> ajuda no mapeamento dos atributos do elemento de configuração para as propriedades e a definição de valores padrão se o atributo não estiver definido. Depois que os valores da configuração são carregados e aplicados às propriedades do tipo, o <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A> método é chamado, o que converte as propriedades em uma instância concreta de um elemento de associação.

Esse manipulador de configuração é mapeado para a representação a seguir no App.config ou Web.config para o serviço ou cliente.

```xml
<customTextMessageEncoding encoding="utf-8" contentType="text/xml" messageVersion="Soap11Addressing1" />
```

O exemplo usa a codificação ISO-8859-1.

Para usar esse manipulador de configuração, ele deve ser registrado usando o elemento de configuração a seguir.

```xml
<extensions>
    <bindingElementExtensions>
        <add name="customTextMessageEncoding" type="
Microsoft.ServiceModel.Samples.CustomTextMessageEncodingBindingSection,
                  CustomTextMessageEncoder" />
    </bindingElementExtensions>
</extensions>
```
