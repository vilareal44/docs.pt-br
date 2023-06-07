---
title: Publicação personalizada de WSDL
ms.date: 03/30/2017
ms.assetid: 3b3e8103-2c95-4db3-a05b-46aa8e9d4d29
ms.openlocfilehash: b18ac2f72d58c768b3784e1c414a71cdaec50c01
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596689"
---
# <a name="custom-wsdl-publication"></a>Publicação personalizada de WSDL
Este exemplo demonstra como:  
  
- Implemente um <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> em um <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType> atributo personalizado para exportar Propriedades de atributo como anotações WSDL.  
  
- Implemente <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> para importar as anotações WSDL personalizadas.  
  
- Implemente <xref:System.ServiceModel.Description.IServiceContractGenerationExtension?displayProperty=nameWithType> e <xref:System.ServiceModel.Description.IOperationContractGenerationExtension?displayProperty=nameWithType> em um comportamento de contrato personalizado e um comportamento de operação personalizado, respectivamente, para gravar anotações importadas como comentários no CodeDOM para o contrato e a operação importados.  
  
- Use o <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> para baixar o WSDL, a <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> para importar o WSDL usando o importador de WSDL personalizado e o <xref:System.ServiceModel.Description.ServiceContractGenerator?displayProperty=nameWithType> para gerar o código do cliente Windows Communication Foundation (WCF) com as anotações WSDL como///e ' ' ' comentários em C# e Visual Basic.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
## <a name="service"></a>Serviço  
 O serviço neste exemplo é marcado com dois atributos personalizados. O primeiro, o `WsdlDocumentationAttribute` , aceita uma cadeia de caracteres no construtor e pode ser aplicado para fornecer uma interface ou operação de contrato com uma cadeia de caracteres que descreve seu uso. O segundo, `WsdlParamOrReturnDocumentationAttribute` , pode ser aplicado a valores de retorno ou parâmetros para descrever esses valores na operação. O exemplo a seguir mostra um contrato de serviço, `ICalculator` , descrito usando esses atributos.  
  
```csharp  
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
// Document it.  
[WsdlDocumentation("The ICalculator contract performs basic calculation services.")]  
public interface ICalculator  
{  
    [OperationContract]  
    [WsdlDocumentation("The Add operation adds two numbers and returns the result.")]  
    [return:WsdlParamOrReturnDocumentation("The result of adding the two arguments together.")]  
    double Add(  
      [WsdlParamOrReturnDocumentation("The first value to add.")]double n1,
      [WsdlParamOrReturnDocumentation("The second value to add.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Subtract operation subtracts the second argument from the first.")]  
    [return:WsdlParamOrReturnDocumentation("The result of the second argument subtracted from the first.")]  
    double Subtract(  
      [WsdlParamOrReturnDocumentation("The value from which the second is subtracted.")]double n1,
      [WsdlParamOrReturnDocumentation("The value that is subtracted from the first.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Multiply operation multiplies two values.")]  
    [return:WsdlParamOrReturnDocumentation("The result of multiplying the first and second arguments.")]  
    double Multiply(  
      [WsdlParamOrReturnDocumentation("The first value to multiply.")]double n1,
      [WsdlParamOrReturnDocumentation("The second value to multiply.")]double n2  
    );  
  
    [OperationContract]  
    [WsdlDocumentation("The Divide operation returns the value of the first argument divided by the second argument.")]  
    [return:WsdlParamOrReturnDocumentation("The result of dividing the first argument by the second.")]  
    double Divide(  
      [WsdlParamOrReturnDocumentation("The numerator.")]double n1,
      [WsdlParamOrReturnDocumentation("The denominator.")]double n2  
    );  
}  
```  
  
 O `WsdlDocumentationAttribute` implementa <xref:System.ServiceModel.Description.IContractBehavior> e <xref:System.ServiceModel.Description.IOperationBehavior> , portanto, as instâncias de atributo são adicionadas ao correspondente <xref:System.ServiceModel.Description.ContractDescription> ou <xref:System.ServiceModel.Description.OperationDescription> quando o serviço é aberto. O atributo também implementa <xref:System.ServiceModel.Description.IWsdlExportExtension> . Quando <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%28System.ServiceModel.Description.WsdlExporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> é chamado, o <xref:System.ServiceModel.Description.WsdlExporter> que é usado para exportar os metadados e o <xref:System.ServiceModel.Description.WsdlContractConversionContext> que contém os objetos de descrição do serviço é passado como parâmetros, permitindo a modificação dos metadados exportados.  
  
 Neste exemplo, dependendo se o objeto de contexto de exportação tem um <xref:System.ServiceModel.Description.ContractDescription> ou um <xref:System.ServiceModel.Description.OperationDescription> , um comentário é extraído do atributo usando a propriedade Text e é adicionado ao elemento de anotação WSDL, conforme mostrado no código a seguir.  
  
```csharp
public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)
{
    if (contractDescription != null)
    {
        // Inside this block it is the contract-level comment attribute.
        // This.Text returns the string for the contract attribute.
        // Set the doc element; if this isn't done first, there is no XmlElement in the
        // DocumentElement property.
        context.WsdlPortType.Documentation = string.Empty;
        // Contract comments.
        XmlDocument owner = context.WsdlPortType.DocumentationElement.OwnerDocument;
        XmlElement summaryElement = owner.CreateElement("summary");
        summaryElement.InnerText = this.Text;
        context.WsdlPortType.DocumentationElement.AppendChild(summaryElement);
    }
    else
    {
        Operation operation = context.GetOperation(operationDescription);
        if (operation != null)
        {
            // We are dealing strictly with the operation here.
            // This.Text returns the string for the operation-level attributes.
            // Set the doc element; if this isn't done first, there is no XmlElement in the
            // DocumentElement property.
            operation.Documentation = String.Empty;

            // Operation C# triple comments.
            XmlDocument owner = operation.DocumentationElement.OwnerDocument;
            XmlElement newSummaryElement = owner.CreateElement("summary");
            newSummaryElement.InnerText = this.Text;
            operation.DocumentationElement.AppendChild(newSummaryElement);
        }
    }
}
```  
  
 Se uma operação estiver sendo exportada, o exemplo usará a reflexão para obter quaisquer `WsdlParamOrReturnDocumentationAttribute` valores para parâmetros e valores de retorno e os adicionará aos elementos da anotação WSDL para essa operação da seguinte maneira.  
  
```csharp
// Get returns information  
ParameterInfo returnValue = operationDescription.SyncMethod.ReturnParameter;  
object[] returnAttrs = returnValue.GetCustomAttributes(typeof(WsdlParamOrReturnDocumentationAttribute), false);  
if (returnAttrs.Length != 0)  
{  
    // <returns>text.</returns>  
    XmlElement returnsElement = owner.CreateElement("returns");  
    returnsElement.InnerText = ((WsdlParamOrReturnDocumentationAttribute)returnAttrs[0]).ParamComment;  
    operation.DocumentationElement.AppendChild(returnsElement);  
}  
  
// Get parameter information.  
ParameterInfo[] args = operationDescription.SyncMethod.GetParameters();  
for (int i = 0; i < args.Length; i++)  
{  
    object[] docAttrs = args[i].GetCustomAttributes(typeof(WsdlParamOrReturnDocumentationAttribute), false);  
    if (docAttrs.Length == 1)  
    {  
        // <param name="Int1">Text.</param>  
        XmlElement newParamElement = owner.CreateElement("param");  
        XmlAttribute paramName = owner.CreateAttribute("name");  
        paramName.Value = args[i].Name;  
        newParamElement.InnerText = ((WsdlParamOrReturnDocumentationAttribute)docAttrs[0]).ParamComment;  
        newParamElement.Attributes.Append(paramName);  
        operation.DocumentationElement.AppendChild(newParamElement);  
    }  
}  
```  
  
 Em seguida, o exemplo publica metadados da maneira padrão, usando o arquivo de configuração a seguir.  
  
```xml  
<services>  
  <service
      name="Microsoft.ServiceModel.Samples.CalculatorService"  
      behaviorConfiguration="CalculatorServiceBehavior">  
    <!-- ICalculator is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc  -->  
    <endpoint address=""  
              binding="wsHttpBinding"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    <!-- the mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->  
    <endpoint address="mex"  
              binding="mexHttpBinding"  
              contract="IMetadataExchange" />  
  </service>  
</services>  
  
<!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="False" />  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
## <a name="svcutil-client"></a>Cliente svcutil  
 Este exemplo não usa svcutil. exe. O contrato é fornecido no arquivo generatedClient.cs para que, após o exemplo, demonstrasse a importação de WSDL personalizada e a geração de código, o serviço pode ser invocado. Para usar o importador WSDL personalizado a seguir para este exemplo, você pode executar svcutil. exe e especificar a `/svcutilConfig` opção, fornecendo o caminho para o arquivo de configuração do cliente usado neste exemplo, que faz referência à `WsdlDocumentation.dll` biblioteca. Para carregar o `WsdlDocumentationImporter` , no entanto, o Svuctil. exe deve ser capaz de localizar e carregar a `WsdlDocumentation.dll` biblioteca, o que significa que ela está registrada no cache de assembly global, no caminho ou está no mesmo diretório que svcutil. exe. Para um exemplo básico como esse, a coisa mais fácil é copiar svcutil. exe e o arquivo de configuração do cliente no mesmo diretório que `WsdlDocumentation.dll` e executá-lo a partir daí.  
  
## <a name="the-custom-wsdl-importer"></a>O importador WSDL personalizado  
 O <xref:System.ServiceModel.Description.IWsdlImportExtension> objeto personalizado `WsdlDocumentationImporter` também implementa <xref:System.ServiceModel.Description.IContractBehavior> e <xref:System.ServiceModel.Description.IOperationBehavior> deve ser adicionado a um ServiceEndpoint importado e <xref:System.ServiceModel.Description.IServiceContractGenerationExtension> e <xref:System.ServiceModel.Description.IOperationContractGenerationExtension> para ser chamado para modificar a geração de código quando o contrato ou o código de operação está sendo criado.  
  
 Primeiro, no <xref:System.ServiceModel.Description.IWsdlImportExtension.ImportContract%28System.ServiceModel.Description.WsdlImporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> método, o exemplo determina se a anotação WSDL está no nível de contrato ou de operação e se adiciona como um comportamento no escopo apropriado, passando o texto de anotação importado para seu construtor.  
  
```csharp
public void ImportContract(WsdlImporter importer, WsdlContractConversionContext context)  
{  
    // Contract Documentation  
    if (context.WsdlPortType.Documentation != null)  
    {  
        // System examines the contract behaviors to see whether any implement IWsdlImportExtension.  
        context.Contract.Behaviors.Add(new WsdlDocumentationImporter(context.WsdlPortType.Documentation));  
    }  
    // Operation Documentation  
    foreach (Operation operation in context.WsdlPortType.Operations)  
    {  
        if (operation.Documentation != null)  
        {  
            OperationDescription operationDescription = context.Contract.Operations.Find(operation.Name);  
            if (operationDescription != null)  
            {  
                // System examines the operation behaviors to see whether any implement IWsdlImportExtension.  
                operationDescription.Behaviors.Add(new WsdlDocumentationImporter(operation.Documentation));  
            }  
        }  
    }  
}  
```  
  
 Em seguida, quando o código é gerado, o sistema invoca os <xref:System.ServiceModel.Description.IServiceContractGenerationExtension.GenerateContract%28System.ServiceModel.Description.ServiceContractGenerationContext%29> <xref:System.ServiceModel.Description.IOperationContractGenerationExtension.GenerateOperation%28System.ServiceModel.Description.OperationContractGenerationContext%29> métodos e, passando as informações de contexto apropriadas. O exemplo formata as anotações WSDL personalizadas e as insere como comentários no CodeDom.  
  
```csharp
public void GenerateContract(ServiceContractGenerationContext context)  
{  
    Debug.WriteLine("In generate contract.");  
    context.ContractType.Comments.AddRange(FormatComments(text));  
}  
  
public void GenerateOperation(OperationContractGenerationContext context)  
{  
    context.SyncMethod.Comments.AddRange(FormatComments(text));  
    Debug.WriteLine("In generate operation.");  
}  
```  
  
## <a name="the-client-application"></a>O aplicativo cliente  
 O aplicativo cliente carrega o importador WSDL personalizado especificando-o no arquivo de configuração do aplicativo.  
  
```xml  
<client>  
  <endpoint address="http://localhost/servicemodelsamples/service.svc"
  binding="wsHttpBinding"
  contract="ICalculator" />  
  <metadata>  
    <wsdlImporters>  
      <extension type="Microsoft.ServiceModel.Samples.WsdlDocumentationImporter, WsdlDocumentation"/>  
    </wsdlImporters>  
  </metadata>  
</client>  
```  
  
 Depois que o importador personalizado tiver sido especificado, o sistema de metadados do WCF carregará o importador personalizado em qualquer <xref:System.ServiceModel.Description.WsdlImporter> criado para essa finalidade. Este exemplo usa o <xref:System.ServiceModel.Description.MetadataExchangeClient> para baixar os metadados, o <xref:System.ServiceModel.Description.WsdlImporter> configurado corretamente para importar os metadados usando o importador personalizado que o exemplo cria e o <xref:System.ServiceModel.Description.ServiceContractGenerator> para compilar as informações de contrato modificadas em Visual Basic e código de cliente C# que pode ser usado no Visual Studio para dar suporte ao IntelliSense ou compilado na documentação XML.  
  
```csharp
/// From WSDL Documentation:  
///
/// <summary>The ICalculator contract performs basic calculation
/// services.</summary>
///
[System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "3.0.0.0")]  
[System.ServiceModel.ServiceContractAttribute(Namespace="http://Microsoft.ServiceModel.Samples", ConfigurationName="ICalculator")]  
public interface ICalculator  
{  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Add operation adds two numbers and returns the
    /// result.</summary><returns>The result of adding the two arguments
    /// together.</returns><param name="n1">The first value to add.</param><param
    /// name="n2">The second value to add.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Add", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/AddResponse")]  
    double Add(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Subtract operation subtracts the second argument from the
    /// first.</summary><returns>The result of the second argument subtracted from the
    /// first.</returns><param name="n1">The value from which the second is
    /// subtracted.</param><param name="n2">The value that is subtracted from the
    /// first.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Subtract", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/SubtractResponse")]  
    double Subtract(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Multiply operation multiplies two values.</summary><returns>The
    /// result of multiplying the first and second arguments.</returns><param
    /// name="n1">The first value to multiply.</param><param name="n2">The second value
    /// to multiply.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Multiply", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/MultiplyResponse")]  
    double Multiply(double n1, double n2);  
  
    /// From WSDL Documentation:  
    ///
    /// <summary>The Divide operation returns the value of the first argument divided
    /// by the second argument.</summary><returns>The result of dividing the first
    /// argument by the second.</returns><param name="n1">The numerator.</param><param
    /// name="n2">The denominator.</param>
    ///
    [System.ServiceModel.OperationContractAttribute(Action="http://Microsoft.ServiceModel.Samples/ICalculator/Divide", ReplyAction="http://Microsoft.ServiceModel.Samples/ICalculator/DivideResponse")]  
    double Divide(double n1, double n2);  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Metadata\WsdlDocumentation`  
