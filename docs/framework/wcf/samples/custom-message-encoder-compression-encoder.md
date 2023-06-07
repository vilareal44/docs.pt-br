---
title: 'Codificador de mensagem personalizado: Codificador de compactação'
ms.date: 03/30/2017
ms.assetid: 57450b6c-89fe-4b8a-8376-3d794857bfd7
ms.openlocfilehash: db20ec20579d6fcb0ec202920db0d7781b0676df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600614"
---
# <a name="custom-message-encoder-compression-encoder"></a>Codificador de mensagem personalizado: Codificador de compactação

Este exemplo demonstra como implementar um codificador personalizado usando a plataforma Windows Communication Foundation (WCF).

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Compression`

## <a name="sample-details"></a>Detalhes de exemplo

Este exemplo consiste em um programa de console do cliente (. exe), um programa de console de serviço de hospedagem interna (. exe) e uma biblioteca de codificador de mensagem de compactação (. dll). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ISampleServer` interface, que expõe operações de eco de cadeia de caracteres básicas ( `Echo` e `BigEcho` ). O cliente faz solicitações síncronas para uma determinada operação e as respostas do serviço repetindo a mensagem de volta para o cliente. A atividade de cliente e serviço é visível nas janelas do console. A intenção deste exemplo é mostrar como escrever um codificador personalizado e demonstrar o impacto da compactação de uma mensagem na conexão. Você pode adicionar instrumentação ao codificador de mensagem de compactação para calcular o tamanho da mensagem, o tempo de processamento ou ambos.

> [!NOTE]
> No .NET Framework 4, a descompactação automática foi habilitada em um cliente WCF se o servidor estiver enviando uma resposta compactada (criada com um algoritmo como GZip ou deflate). Se o serviço for hospedado na Web no IIS (servidor de informações da Internet), o IIS poderá ser configurado para que o serviço envie uma resposta compactada. Este exemplo pode ser usado se o requisito for fazer a compactação e a descompactação no cliente e no serviço ou se o serviço for auto-hospedado.

O exemplo demonstra como criar e integrar um codificador de mensagem personalizado em um aplicativo WCF. A biblioteca GZipEncoder. dll é implantada com o cliente e o serviço. Este exemplo também demonstra o impacto da compactação de mensagens. O código em GZipEncoder. dll demonstra o seguinte:

- Criando um codificador personalizado e um alocador de codificador.

- Desenvolvendo um elemento Binding para um codificador personalizado.

- Usando a configuração de associação personalizada para integrar elementos de associação personalizados.

- Desenvolver um manipulador de configuração personalizado para permitir a configuração de arquivo de um elemento de associação personalizado.

Conforme indicado anteriormente, há várias camadas que são implementadas em um codificador personalizado. Para ilustrar melhor a relação entre cada uma dessas camadas, uma ordem simplificada de eventos para a inicialização do serviço está na lista a seguir:

1. O servidor é iniciado.

2. As informações de configuração são lidas.

    1. A configuração de serviço registra o manipulador de configuração personalizada.

    2. O host de serviço é criado e aberto.

    3. O elemento de configuração personalizada cria e retorna o elemento de associação personalizado.

    4. O elemento de associação personalizado cria e retorna um alocador de codificador de mensagem.

3. Uma mensagem é recebida.

4. O alocador de codificador de mensagem retorna um codificador de mensagem para leitura na mensagem e gravação da resposta.

5. A camada do codificador é implementada como uma fábrica de classes. Somente a fábrica de classes de codificador deve ser exposta publicamente para o codificador personalizado. O objeto de fábrica é retornado pelo elemento de associação quando <xref:System.ServiceModel.ServiceHost> o <xref:System.ServiceModel.ChannelFactory%601> objeto ou é criado. Os codificadores de mensagens podem operar em um modo de streaming ou em buffer. Este exemplo demonstra o modo em buffer e o modo de streaming.

Para cada modo, há um `ReadMessage` `WriteMessage` método e o que acompanha a `MessageEncoder` classe abstract. A maior parte do trabalho de codificação ocorre nesses métodos. O exemplo encapsula o texto existente e codificadores de mensagem binária. Isso permite que o exemplo delegue a leitura e gravação da representação de transmissão de mensagens para o codificador interno e permite que o codificador de compactação compacte ou descompacte os resultados. Como não há pipeline para codificação de mensagens, esse é o único modelo para usar vários codificadores no WCF. Depois que a mensagem for descompactada, a mensagem resultante será passada para cima na pilha para o identificador do canal. Durante a compactação, a mensagem compactada resultante é gravada diretamente no fluxo fornecido.

Este exemplo usa métodos auxiliares ( `CompressBuffer` e `DecompressBuffer` ) para executar a conversão de buffers em fluxos para usar a `GZipStream` classe.

O buffer `ReadMessage` e as `WriteMessage` classes fazem uso da `BufferManager` classe. O codificador é acessível somente por meio do alocador de codificador. A `MessageEncoderFactory` classe abstract fornece uma propriedade chamada `Encoder` para acessar o codificador atual e um método chamado `CreateSessionEncoder` para criar um codificador que dá suporte a sessões. Esse codificador pode ser usado no cenário em que o canal dá suporte a sessões, é ordenado e é confiável. Esse cenário permite a otimização em cada sessão dos dados gravados na conexão. Se isso não for desejado, o método base não deve ser sobrecarregado. A `Encoder` propriedade fornece um mecanismo para acessar o codificador sem sessão e a implementação padrão do `CreateSessionEncoder` método retorna o valor da propriedade. Como o exemplo encapsula um codificador existente para fornecer compactação, a `MessageEncoderFactory` implementação aceita um `MessageEncoderFactory` que representa a fábrica interna do codificador.

Agora que o codificador e a fábrica do codificador estão definidos, eles podem ser usados com um cliente e um serviço WCF. No entanto, esses codificadores devem ser adicionados à pilha de canais. Você pode derivar classes das <xref:System.ServiceModel.ServiceHost> <xref:System.ServiceModel.ChannelFactory%601> classes e e substituir os `OnInitialize` métodos para adicionar essa fábrica de codificador manualmente. Você também pode expor a fábrica do codificador por meio de um elemento de ligação personalizado.

Para criar um novo elemento de associação personalizado, derive uma classe da <xref:System.ServiceModel.Channels.BindingElement> classe. No entanto, há vários tipos de elementos de ligação. Para garantir que o elemento de associação personalizado seja reconhecido como um elemento de ligação de codificação de mensagem, você também deve implementar o <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> . O <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> expõe um método para criar uma nova fábrica de codificador de mensagem ( `CreateMessageEncoderFactory` ), que é implementada para retornar uma instância do alocador de codificador de mensagem correspondente. Além disso, o <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> tem uma propriedade para indicar a versão de endereçamento. Como esse exemplo encapsula os codificadores existentes, a implementação de exemplo também encapsula os elementos de associação de codificador existentes e usa um elemento de ligação de codificador interno como um parâmetro para o construtor e o expõe por meio de uma propriedade. O código de exemplo a seguir mostra a implementação da `GZipMessageEncodingBindingElement` classe.

```csharp
public sealed class GZipMessageEncodingBindingElement
                        : MessageEncodingBindingElement //BindingElement
                        , IPolicyExportExtension
{

    //We use an inner binding element to store information
    //required for the inner encoder.
    MessageEncodingBindingElement innerBindingElement;

        //By default, use the default text encoder as the inner encoder.
        public GZipMessageEncodingBindingElement()
            : this(new TextMessageEncodingBindingElement()) { }

    public GZipMessageEncodingBindingElement(MessageEncodingBindingElement messageEncoderBindingElement)
    {
        this.innerBindingElement = messageEncoderBindingElement;
    }

    public MessageEncodingBindingElement InnerMessageEncodingBindingElement
    {
        get { return innerBindingElement; }
        set { innerBindingElement = value; }
    }

    //Main entry point into the encoder binding element.
    // Called by WCF to get the factory that creates the
    //message encoder.
    public override MessageEncoderFactory CreateMessageEncoderFactory()
    {
        return new
GZipMessageEncoderFactory(innerBindingElement.CreateMessageEncoderFactory());
    }

    public override MessageVersion MessageVersion
    {
        get { return innerBindingElement.MessageVersion; }
        set { innerBindingElement.MessageVersion = value; }
    }

    public override BindingElement Clone()
    {
        return new
        GZipMessageEncodingBindingElement(this.innerBindingElement);
    }

    public override T GetProperty<T>(BindingContext context)
    {
        if (typeof(T) == typeof(XmlDictionaryReaderQuotas))
        {
            return innerBindingElement.GetProperty<T>(context);
        }
        else
        {
            return base.GetProperty<T>(context);
        }
    }

    public override IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.BuildInnerChannelFactory<TChannel>();
    }

    public override IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.BuildInnerChannelListener<TChannel>();
    }

    public override bool CanBuildChannelListener<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.CanBuildInnerChannelListener<TChannel>();
    }

    void IPolicyExportExtension.ExportPolicy(MetadataExporter exporter, PolicyConversionContext policyContext)
    {
        if (policyContext == null)
        {
            throw new ArgumentNullException("policyContext");
        }
       XmlDocument document = new XmlDocument();
       policyContext.GetBindingAssertions().Add(document.CreateElement(
            GZipMessageEncodingPolicyConstants.GZipEncodingPrefix,
            GZipMessageEncodingPolicyConstants.GZipEncodingName,
            GZipMessageEncodingPolicyConstants.GZipEncodingNamespace));
    }
}
```

Observe que `GZipMessageEncodingBindingElement` a classe implementa a `IPolicyExportExtension` interface, para que esse elemento de ligação possa ser exportado como uma política nos metadados, conforme mostrado no exemplo a seguir.

```xml
<wsp:Policy wsu:Id="BufferedHttpSampleServer_ISampleServer_policy">
    <wsp:ExactlyOne>
      <wsp:All>
        <gzip:text xmlns:gzip=
        "http://schemas.microsoft.com/ws/06/2004/mspolicy/netgzip1" />
       <wsaw:UsingAddressing />
     </wsp:All>
   </wsp:ExactlyOne>
</wsp:Policy>
```

A `GZipMessageEncodingBindingElementImporter` classe implementa a `IPolicyImportExtension` interface, essa classe importa a política para `GZipMessageEncodingBindingElement` . A ferramenta svcutil. exe pode ser usada para importar políticas para o arquivo de configuração, para manipular `GZipMessageEncodingBindingElement` , o seguinte deve ser adicionado a svcutil. exe. config.

```xml
<configuration>
  <system.serviceModel>
    <extensions>
      <bindingElementExtensions>
        <add name="gzipMessageEncoding"
          type=
            "Microsoft.ServiceModel.Samples.GZipMessageEncodingElement, GZipEncoder, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      </bindingElementExtensions>
    </extensions>
    <client>
      <metadata>
        <policyImporters>
          <remove type=
"System.ServiceModel.Channels.MessageEncodingBindingElementImporter, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
          <extension type=
"Microsoft.ServiceModel.Samples.GZipMessageEncodingBindingElementImporter, GZipEncoder, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        </policyImporters>
      </metadata>
    </client>
  </system.serviceModel>
</configuration>
```

Agora que há um elemento de associação correspondente para o codificador de compactação, ele pode ser conectado de forma programática ao serviço ou cliente construindo um novo objeto de associação personalizado e adicionando o elemento de associação personalizado a ele, conforme mostrado no código de exemplo a seguir.

```csharp
ICollection<BindingElement> bindingElements = new List<BindingElement>();
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();
GZipMessageEncodingBindingElement compBindingElement = new GZipMessageEncodingBindingElement ();
bindingElements.Add(compBindingElement);
bindingElements.Add(httpBindingElement);
CustomBinding binding = new CustomBinding(bindingElements);
binding.Name = "SampleBinding";
binding.Namespace = "http://tempuri.org/bindings";
```

Embora isso possa ser suficiente para a maioria dos cenários de usuário, o suporte a uma configuração de arquivo é essencial se um serviço for hospedado na Web. Para dar suporte ao cenário hospedado na Web, você deve desenvolver um manipulador de configuração personalizado para permitir que um elemento de ligação personalizado seja configurável em um arquivo.

Você pode criar um manipulador de configuração para o elemento de associação na parte superior do sistema de configuração. O manipulador de configuração para o elemento de associação deve derivar da <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> classe. O <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.BindingElementType?displayProperty=nameWithType> informa o sistema de configuração do tipo de elemento de associação a ser criado para esta seção. Todos os aspectos do `BindingElement` que podem ser definidos devem ser expostos como propriedades na <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> classe derivada. O <xref:System.Configuration.ConfigurationPropertyAttribute> ajuda no mapeamento dos atributos do elemento de configuração para as propriedades e a definição de valores padrão se os atributos estiverem ausentes. Depois que os valores da configuração são carregados e aplicados às propriedades, o <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A?displayProperty=nameWithType> método é chamado, o que converte as propriedades em uma instância concreta de um elemento de associação. O <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.ApplyConfiguration%2A?displayProperty=nameWithType> método é usado para converter as propriedades na <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> classe derivada nos valores a serem definidos no elemento de associação recém-criado.

O código de exemplo a seguir mostra a implementação do `GZipMessageEncodingElement` .

```csharp
public class GZipMessageEncodingElement : BindingElementExtensionElement
{
    public GZipMessageEncodingElement()
    {
    }

//Called by the WCF to discover the type of binding element this
//config section enables
    public override Type BindingElementType
    {
        get { return typeof(GZipMessageEncodingBindingElement); }
    }

    //The only property we need to configure for our binding element is
    //the type of inner encoder to use. Here, we support text and
    //binary.
    [ConfigurationProperty("innerMessageEncoding",
                         DefaultValue = "textMessageEncoding")]
    public string InnerMessageEncoding
    {
        get { return (string)base["innerMessageEncoding"]; }
        set { base["innerMessageEncoding"] = value; }
    }

    //Called by the WCF to apply the configuration settings (the
    //property above) to the binding element
    public override void ApplyConfiguration(BindingElement bindingElement)
    {
        GZipMessageEncodingBindingElement binding =
                (GZipMessageEncodingBindingElement)bindingElement;
        PropertyInformationCollection propertyInfo =
                    this.ElementInformation.Properties;
        if (propertyInfo["innerMessageEncoding"].ValueOrigin !=
                                     PropertyValueOrigin.Default)
        {
            switch (this.InnerMessageEncoding)
            {
                case "textMessageEncoding":
                    binding.InnerMessageEncodingBindingElement =
                      new TextMessageEncodingBindingElement();
                    break;
                case "binaryMessageEncoding":
                    binding.InnerMessageEncodingBindingElement =
                         new BinaryMessageEncodingBindingElement();
                    break;
            }
        }
    }

    //Called by the WCF to create the binding element
    protected override BindingElement CreateBindingElement()
    {
        GZipMessageEncodingBindingElement bindingElement =
                new GZipMessageEncodingBindingElement();
        this.ApplyConfiguration(bindingElement);
        return bindingElement;
    }
}
```

Esse manipulador de configuração é mapeado para a representação a seguir no app. config ou Web. config para o serviço ou cliente.

```xml
<gzipMessageEncoding innerMessageEncoding="textMessageEncoding" />
```

Para usar esse manipulador de configuração, ele deve ser registrado no [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) elemento, conforme mostrado na seguinte configuração de exemplo.

```xml
<extensions>
    <bindingElementExtensions>
       <add
           name="gzipMessageEncoding"
           type=
           "Microsoft.ServiceModel.Samples.GZipMessageEncodingElement,
           GZipEncoder, Version=1.0.0.0, Culture=neutral,
           PublicKeyToken=null" />
      </bindingElementExtensions>
</extensions>
```

Quando você executa o servidor, as solicitações de operação e as respostas são exibidas na janela do console. Pressione ENTER na janela para desligar o servidor.

```console
Press Enter key to Exit.

        Server Echo(string input) called:
        Client message: Simple hello

        Server BigEcho(string[] input) called:
        64 client messages
```

Quando você executa o cliente, as solicitações de operação e as respostas são exibidas na janela do console. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Calling Echo(string):
Server responds: Simple hello Simple hello

Calling BigEcho(string[]):
Server responds: Hello 0

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Instale o ASP.NET 4,0 usando o seguinte comando:

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

3. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Compression`
