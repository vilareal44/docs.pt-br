---
title: Alternativa de DataContract
ms.date: 03/30/2017
ms.assetid: b0188f3c-00a9-4cf0-a887-a2284c8fb014
ms.openlocfilehash: 9677e3cf024e6c1e5b2f3360423ab55536748495
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600030"
---
# <a name="datacontract-surrogate"></a>Alternativa de DataContract
Este exemplo demonstra como processos como serialização, desserialização, exportação de esquema e importação de esquema podem ser personalizados usando uma classe substituta de contrato de dados. Este exemplo mostra como usar um substituto em um cenário de cliente e servidor em que os dados são serializados e transmitidos entre um cliente do Windows Communication Foundation (WCF) e um serviço.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O exemplo usa o seguinte contrato de serviço:  
  
```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
[AllowNonSerializableTypes]  
public interface IPersonnelDataService  
{  
    [OperationContract]  
    void AddEmployee(Employee employee);  
  
    [OperationContract]  
    Employee GetEmployee(string name);  
}  
```  
  
 A `AddEmployee` operação permite aos usuários adicionar dados sobre novos funcionários e a `GetEmployee` operação dá suporte à pesquisa de funcionários com base no nome.  
  
 Essas operações usam o seguinte tipo de dados:  
  
```csharp
[DataContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
class Employee  
{  
    [DataMember]  
    public DateTime dateHired;  
  
    [DataMember]  
    public Decimal salary;  
  
    [DataMember]  
    public Person person;  
}  
```  
  
 No `Employee` tipo, a `Person` classe (mostrada no código de exemplo a seguir) não pode ser serializada pelo <xref:System.Runtime.Serialization.DataContractSerializer> porque ela não é uma classe de contrato de dados válida.  
  
```csharp
public class Person  
{  
    public string firstName;  
  
    public string lastName;  
  
    public int age;  
  
    public Person() { }  
}  
```  
  
 Você pode aplicar o <xref:System.Runtime.Serialization.DataContractAttribute> atributo à `Person` classe, mas isso nem sempre é possível. Por exemplo, a `Person` classe pode ser definida em um assembly separado sobre o qual você não tem controle.  
  
 Dada essa restrição, uma maneira de serializar a `Person` classe é substituí-la por outra classe marcada com <xref:System.Runtime.Serialization.DataContractAttribute> e copiar os dados necessários para a nova classe. O objetivo é fazer com que a `Person` classe apareça como um DataContract para o <xref:System.Runtime.Serialization.DataContractSerializer> . Observe que essa é uma maneira de serializar classes de contrato que não são de dados.  
  
 O exemplo substitui logicamente a `Person` classe por uma classe diferente denominada `PersonSurrogated` .  
  
```csharp
[DataContract(Name="Person", Namespace = "http://Microsoft.ServiceModel.Samples")]  
public class PersonSurrogated  
{  
    [DataMember]  
    public string FirstName;  
  
    [DataMember]  
    public string LastName;  
  
    [DataMember]  
    public int Age;  
}  
```  
  
 O substituto de contrato de dados é usado para obter essa substituição. Um substituto de contrato de dados é uma classe que implementa <xref:System.Runtime.Serialization.IDataContractSurrogate> . Neste exemplo, a `AllowNonSerializableTypesSurrogate` classe implementa essa interface.  
  
 Na implementação da interface, a primeira tarefa é estabelecer um mapeamento de tipo de `Person` para `PersonSurrogated` . Isso é usado no momento da serialização, bem como no momento da exportação do esquema. Esse mapeamento é obtido com a implementação do <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%28System.Type%29> método.  
  
```csharp
public Type GetDataContractType(Type type)  
{  
    if (typeof(Person).IsAssignableFrom(type))  
    {  
        return typeof(PersonSurrogated);  
    }  
    return type;  
}  
```  
  
 O <xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%28System.Object%2CSystem.Type%29> método mapeia uma `Person` instância do para uma `PersonSurrogated` instância do durante a serialização, conforme mostrado no código de exemplo a seguir.  
  
```csharp
public object GetObjectToSerialize(object obj, Type targetType)  
{  
    if (obj is Person)  
    {  
        Person person = (Person)obj;  
        PersonSurrogated personSurrogated = new PersonSurrogated();  
        personSurrogated.FirstName = person.firstName;  
        personSurrogated.LastName = person.lastName;  
        personSurrogated.Age = person.age;  
        return personSurrogated;  
    }  
    return obj;  
}  
```  
  
 O <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDeserializedObject%28System.Object%2CSystem.Type%29> método fornece o mapeamento reverso para desserialização, conforme mostrado no código de exemplo a seguir.  
  
```csharp
public object GetDeserializedObject(object obj,
Type targetType)  
{  
    if (obj is PersonSurrogated)  
    {  
        PersonSurrogated personSurrogated = (PersonSurrogated)obj;  
        Person person = new Person();  
        person.firstName = personSurrogated.FirstName;  
        person.lastName = personSurrogated.LastName;  
        person.age = personSurrogated.Age;  
        return person;  
    }  
    return obj;  
}  
```  
  
 Para mapear o `PersonSurrogated` contrato de dados para a `Person` classe existente durante a importação de esquema, o exemplo implementa o <xref:System.Runtime.Serialization.IDataContractSurrogate.GetReferencedTypeOnImport%28System.String%2CSystem.String%2CSystem.Object%29> método, conforme mostrado no código de exemplo a seguir.  
  
```csharp
public Type GetReferencedTypeOnImport(string typeName,
               string typeNamespace, object customData)  
{  
if (  
typeNamespace.Equals("http://schemas.datacontract.org/2004/07/DCSurrogateSample")  
)  
    {  
         if (typeName.Equals("PersonSurrogated"))  
        {  
             return typeof(Person);  
        }  
     }  
     return null;  
}  
```  
  
 O código de exemplo a seguir conclui a implementação da <xref:System.Runtime.Serialization.IDataContractSurrogate> interface.  
  
```csharp
public System.CodeDom.CodeTypeDeclaration ProcessImportedType(  
          System.CodeDom.CodeTypeDeclaration typeDeclaration,
          System.CodeDom.CodeCompileUnit compileUnit)  
{  
    return typeDeclaration;  
}  
public object GetCustomDataToExport(Type clrType,
                               Type dataContractType)  
{  
    return null;  
}  
  
public object GetCustomDataToExport(  
System.Reflection.MemberInfo memberInfo, Type dataContractType)  
{  
    return null;  
}  
public void GetKnownCustomDataTypes(  
        KnownTypeCollection customDataTypes)  
{  
    // It does not matter what we do here.  
    throw new NotImplementedException();  
}  
```  
  
 Neste exemplo, o substituto é habilitado em ServiceModel por um atributo chamado `AllowNonSerializableTypesAttribute` . Os desenvolvedores precisariam aplicar esse atributo em seu contrato de serviço, conforme mostrado no `IPersonnelDataService` contrato de serviço acima. Esse atributo implementa `IContractBehavior` e configura o substituto em operações em seus `ApplyClientBehavior` métodos e `ApplyDispatchBehavior` .  
  
 O atributo não é necessário nesse caso – ele é usado para fins de demonstração neste exemplo. Como alternativa, os usuários podem habilitar um substituto Adicionando manualmente um `IContractBehavior` `IEndpointBehavior` ou `IOperationBehavior` usando código ou usando a configuração.  
  
 A `IContractBehavior` implementação procura operações que usam DataContract verificando se elas têm um `DataContractSerializerOperationBehavior` registrado. Se isso for feito, ele definirá a `DataContractSurrogate` Propriedade nesse comportamento. O código de exemplo a seguir mostra como isso é feito. A definição do substituto nesse comportamento de operação permite a serialização e a desserialização.  
  
```csharp
public void ApplyClientBehavior(ContractDescription description, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.ClientRuntime proxy)  
{  
    foreach (OperationDescription opDesc in description.Operations)  
    {  
        ApplyDataContractSurrogate(opDesc);  
    }  
}  
  
public void ApplyDispatchBehavior(ContractDescription description, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.DispatchRuntime dispatch)  
{  
    foreach (OperationDescription opDesc in description.Operations)  
    {  
        ApplyDataContractSurrogate(opDesc);  
    }  
}  
  
private static void ApplyDataContractSurrogate(OperationDescription description)  
{  
    DataContractSerializerOperationBehavior dcsOperationBehavior = description.Behaviors.Find<DataContractSerializerOperationBehavior>();  
    if (dcsOperationBehavior != null)  
    {  
        if (dcsOperationBehavior.DataContractSurrogate == null)  
            dcsOperationBehavior.DataContractSurrogate = new AllowNonSerializableTypesSurrogate();  
    }  
}  
```  
  
 Etapas adicionais precisam ser tomadas para conectar o substituto para uso durante a geração de metadados. Um mecanismo para fazer isso é fornecer um `IWsdlExportExtension` que é o que este exemplo demonstra. Outra maneira é modificar o `WsdlExporter` diretamente.  
  
 O `AllowNonSerializableTypesAttribute` atributo implementa `IWsdlExportExtension` e `IContractBehavior` . A extensão pode ser um `IContractBehavior` ou, `IEndpointBehavior` nesse caso. Sua `IWsdlExportExtension.ExportContract` implementação de método habilita o substituto adicionando-o ao `XsdDataContractExporter` usado durante a geração de esquema para DataContract. O trecho de código a seguir mostra como fazer isso.  
  
```csharp
public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)  
{  
    if (exporter == null)  
        throw new ArgumentNullException("exporter");  
  
    object dataContractExporter;  
    XsdDataContractExporter xsdDCExporter;  
    if (!exporter.State.TryGetValue(typeof(XsdDataContractExporter), out dataContractExporter))  
    {  
        xsdDCExporter = new XsdDataContractExporter(exporter.GeneratedXmlSchemas);  
        exporter.State.Add(typeof(XsdDataContractExporter), xsdDCExporter);  
    }  
    else  
    {  
        xsdDCExporter = (XsdDataContractExporter)dataContractExporter;  
    }  
    if (xsdDCExporter.Options == null)  
        xsdDCExporter.Options = new ExportOptions();  
  
    if (xsdDCExporter.Options.DataContractSurrogate == null)  
        xsdDCExporter.Options.DataContractSurrogate = new AllowNonSerializableTypesSurrogate();  
}  
```  
  
 Quando você executa o exemplo, o cliente chama AddEmployee seguido por uma chamada GetEmployee para verificar se a primeira chamada foi bem-sucedida. O resultado da solicitação de operação GetEmployee é exibido na janela do console do cliente. A operação GetEmployee deve ter sucesso ao localizar o funcionário e imprimir "Found".  
  
> [!NOTE]
> Este exemplo mostra como conectar um substituto para serializar, desserializar e a geração de metadados. Ele não mostra como conectar um substituto para a geração de código de metadados. Para ver um exemplo de como um substituto pode ser usado para conectar-se à geração de código de cliente, consulte o exemplo de [publicação WSDL personalizada](custom-wsdl-publication.md) .  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\DataContract`  
