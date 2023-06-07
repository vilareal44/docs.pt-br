---
title: Utilizando um resolvedor de contrato de dados
ms.date: 03/30/2017
ms.assetid: 2e68a16c-36f0-4df4-b763-32021bff2b89
ms.openlocfilehash: 20abd4d928fc51eb359949ecbb216615e9659b7f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595018"
---
# <a name="using-a-data-contract-resolver"></a>Utilizando um resolvedor de contrato de dados
Um resolvedor de contrato de dados permite que você configure tipos conhecidos dinamicamente. Tipos conhecidos são necessários ao serializar ou desserializar um tipo não esperado por um contrato de dados. Para obter mais informações sobre tipos conhecidos, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md). Tipos conhecidos normalmente são especificados estaticamente. Isso significa que você teria que saber todos os tipos possíveis que uma operação pode receber ao implementar a operação. Há cenários em que isso não é verdadeiro e poder especificar tipos conhecidos dinamicamente é importante.  
  
## <a name="creating-a-data-contract-resolver"></a>Criando um resolvedor de contrato de dados  
 A criação de um resolvedor de contrato de dados envolve a implementação de dois métodos, <xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A> e <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A> . Esses dois métodos implementam retornos de chamada que são usados durante a serialização e desserialização, respectivamente. O <xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A> método é invocado durante a serialização e usa um tipo de contrato de dados e o mapeia para um `xsi:type` nome e namespace. O <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A> método é invocado durante a desserialização e usa um `xsi:type` nome e um namespace e o resolve para um tipo de contrato de dados. Ambos os métodos têm um `knownTypeResolver` parâmetro que pode ser usado para usar o resolvedor de tipo conhecido padrão em sua implementação.  
  
 O exemplo a seguir mostra como implementar um <xref:System.Runtime.Serialization.DataContractResolver> para mapear de e para um tipo de contrato de dados denominado `Customer` derivado de um tipo de contrato de dados `Person` .  
  
```csharp  
public class MyCustomerResolver : DataContractResolver  
{  
    public override bool TryResolveType(Type dataContractType, Type declaredType, DataContractResolver knownTypeResolver, out XmlDictionaryString typeName, out XmlDictionaryString typeNamespace)  
    {  
        if (dataContractType == typeof(Customer))  
        {  
            XmlDictionary dictionary = new XmlDictionary();  
            typeName = dictionary.Add("SomeCustomer");  
            typeNamespace = dictionary.Add("http://tempuri.com");  
            return true;  
        }  
        else  
        {  
            return knownTypeResolver.TryResolveType(dataContractType, declaredType, null, out typeName, out typeNamespace);  
        }  
    }  
  
    public override Type ResolveName(string typeName, string typeNamespace, DataContractResolver knownTypeResolver)  
    {  
        if (typeName == "SomeCustomer" && typeNamespace == "http://tempuri.com")  
        {  
            return typeof(Customer);  
        }  
        else  
        {  
            return knownTypeResolver.ResolveName(typeName, typeNamespace, null);  
        }  
    }  
}  
```  
  
 Depois de definir um, <xref:System.Runtime.Serialization.DataContractResolver> você pode usá-lo passando-o para o <xref:System.Runtime.Serialization.DataContractSerializer> Construtor, conforme mostrado no exemplo a seguir.  
  
```csharp
XmlObjectSerializer serializer = new DataContractSerializer(typeof(Customer), null, Int32.MaxValue, false, false, null, new MyCustomerResolver());  
```  
  
 Você pode especificar um <xref:System.Runtime.Serialization.DataContractResolver> em uma chamada para os <xref:System.Runtime.Serialization.DataContractSerializer.ReadObject%2A?displayProperty=nameWithType> <xref:System.Runtime.Serialization.DataContractSerializer.WriteObject%2A?displayProperty=nameWithType> métodos ou, conforme mostrado no exemplo a seguir.  
  
```csharp
MemoryStream ms = new MemoryStream();  
DataContractSerializer serializer = new DataContractSerializer(typeof(Customer));  
XmlDictionaryWriter writer = XmlDictionaryWriter.CreateDictionaryWriter(XmlWriter.Create(ms));  
serializer.WriteObject(writer, new Customer(), new MyCustomerResolver());  
writer.Flush();  
ms.Position = 0;  
Console.WriteLine(((Customer)serializer.ReadObject(XmlDictionaryReader.CreateDictionaryReader(XmlReader.Create(ms)), false, new MyCustomerResolver()));  
```  
  
 Ou você pode defini-lo no <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> conforme mostrado no exemplo a seguir.  
  
```csharp
ServiceHost host = new ServiceHost(typeof(MyService));  
  
ContractDescription cd = host.Description.Endpoints[0].Contract;  
OperationDescription myOperationDescription = cd.Operations.Find("Echo");  
  
DataContractSerializerOperationBehavior serializerBehavior = myOperationDescription.Behaviors.Find<DataContractSerializerOperationBehavior>();  
if (serializerBehavior == null)  
{  
    serializerBehavior = new DataContractSerializerOperationBehavior(myOperationDescription);  
    myOperationDescription.Behaviors.Add(serializerBehavior);  
}  
  
SerializerBehavior.DataContractResolver = new MyCustomerResolver();  
```  
  
 Você pode especificar declarativamente um resolvedor de contrato de dados implementando um atributo que pode ser aplicado a um serviço.  Para obter mais informações, consulte o exemplo [KnownAssemblyAttribute](../samples/knownassemblyattribute.md) . Este exemplo implementa um atributo chamado "KnownAssembly" que adiciona um resolvedor de contrato de dados personalizado ao comportamento do serviço.  
  
## <a name="see-also"></a>Consulte também

- [Tipos conhecidos de contrato de dados](data-contract-known-types.md)
- [Exemplo de DataContractSerializer](../samples/datacontractserializer-sample.md)
- [KnownAssemblyAttribute](../samples/knownassemblyattribute.md)
