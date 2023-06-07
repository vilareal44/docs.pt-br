---
title: DataContractResolver
ms.date: 03/30/2017
ms.assetid: 6c200c02-bc14-4b8d-bbab-9da31185b805
ms.openlocfilehash: c2a2afaa450e9abe17b62f6be07a2dc41459ca20
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600017"
---
# <a name="datacontractresolver"></a>DataContractResolver
Este exemplo demonstra como os processos de serialização e desserialização podem ser personalizados usando a <xref:System.Runtime.Serialization.DataContractResolver> classe. Este exemplo mostra como usar um DataContractResolver para mapear tipos CLR de e para uma representação xsi: Type durante a serialização e desserialização.

## <a name="sample-details"></a>Detalhes de exemplo
 O exemplo define os tipos CLR a seguir.

```csharp
using System;
using System.Runtime.Serialization;

namespace Types
{
    [DataContract]
    public class Customer
    {
        [DataMember]
        public string Name { get; set; }
    }

    [DataContract]
    public class VIPCustomer : Customer
    {
        [DataMember]
        public string VipInfo { get; set; }
    }

    [DataContract]
    public class RegularCustomer : Customer
    {
    }

    [DataContract]
    public class PreferredVIPCustomer : VIPCustomer
    {
    }
}
```

 O exemplo carrega o assembly, extrai cada um desses tipos e, em seguida, serializa e desserializa-os. O <xref:System.Runtime.Serialization.DataContractResolver> é conectado ao processo de serialização, passando uma instância da <xref:System.Runtime.Serialization.DataContractResolver> classe derivada para o <xref:System.Runtime.Serialization.DataContractSerializer> Construtor, conforme mostrado no exemplo a seguir.

```csharp
this.serializer = new DataContractSerializer(typeof(Object), null, int.MaxValue, false, true, null, new MyDataContractResolver(assembly));
```

 Em seguida, o exemplo serializa os tipos CLR, conforme mostrado no exemplo de código a seguir.

```csharp
Assembly assembly = Assembly.Load(new AssemblyName("Types"));

public void serialize(Type type)
{
    Object instance = Activator.CreateInstance(type);

    Console.WriteLine("----------------------------------------");
    Console.WriteLine();
    Console.WriteLine("Serializing type: {0}", type.Name);
    Console.WriteLine();
    this.buffer = new StringBuilder();
    using (XmlWriter xmlWriter = XmlWriter.Create(this.buffer))
    {
        try
        {
            this.serializer.WriteObject(xmlWriter, instance);
        }
        catch (SerializationException error)
        {
            Console.WriteLine(error.ToString());
        }
    }
    Console.WriteLine(this.buffer.ToString());
}
```

 Em seguida, o exemplo desserializa os tipos xsi:, conforme mostrado no exemplo de código a seguir.

```csharp
public void deserialize(Type type)
{
    Console.WriteLine();
    Console.WriteLine("Deserializing type: {0}", type.Name);
    Console.WriteLine();
    using (XmlReader xmlReader = XmlReader.Create(new StringReader(this.buffer.ToString())))
    {
        Object obj = this.serializer.ReadObject(xmlReader);
    }
}
```

 Como o personalizado <xref:System.Runtime.Serialization.DataContractResolver> é passado para o <xref:System.Runtime.Serialization.DataContractSerializer> Construtor, o <xref:System.Runtime.Serialization.DataContractResolver.TryResolveType%2A> é chamado durante a serialização para mapear um tipo CLR para um equivalente `xsi:type` . Da mesma forma, o <xref:System.Runtime.Serialization.DataContractResolver.ResolveName%2A> é chamado durante a desserialização para mapear o `xsi:type` para um tipo CLR equivalente. Neste exemplo, o <xref:System.Runtime.Serialization.DataContractResolver> é definido como mostrado no exemplo a seguir.

 O exemplo de código a seguir é uma classe derivada de <xref:System.Runtime.Serialization.DataContractResolver> .

```csharp
class MyDataContractResolver : DataContractResolver
{
    private Dictionary<string, XmlDictionaryString> dictionary = new Dictionary<string, XmlDictionaryString>();
    Assembly assembly;

    public MyDataContractResolver(Assembly assembly)
    {
        this.assembly = assembly;
    }

    // Used at deserialization
    // Allows users to map xsi:type name to any Type
    public override Type ResolveName(string typeName, string typeNamespace, DataContractResolver knownTypeResolver)
    {
        XmlDictionaryString tName;
        XmlDictionaryString tNamespace;
        if (dictionary.TryGetValue(typeName, out tName) && dictionary.TryGetValue(typeNamespace, out tNamespace))
        {
            return this.assembly.GetType(tNamespace.Value + "." + tName.Value);
        }
        else
        {
            return null;
        }
    }

    // Used at serialization
    // Maps any Type to a new xsi:type representation
    public override void ResolveType(Type dataContractType, DataContractResolver knownTypeResolver, out XmlDictionaryString typeName, out XmlDictionaryString typeNamespace)
    {
        string name = dataContractType.Name;
        string namesp = dataContractType.Namespace;
        typeName = new XmlDictionaryString(XmlDictionary.Empty, name, 0);
        typeNamespace = new XmlDictionaryString(XmlDictionary.Empty, namesp, 0);
        if (!dictionary.ContainsKey(dataContractType.Name))
        {
            dictionary.Add(name, typeName);
        }
        if (!dictionary.ContainsKey(dataContractType.Namespace))
        {
            dictionary.Add(namesp, typeNamespace);
        }
    }
}
```

 Como parte do exemplo, o projeto de tipos gera o assembly com todos os tipos que são usados neste exemplo. Use este projeto para adicionar, remover ou modificar os tipos que serão serializados.

#### <a name="to-use-this-sample"></a>Para usar este exemplo

1. Usando o Visual Studio 2012, abra o arquivo de solução DCRSample. sln.

2. Para executar a solução, pressione F5

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Data\DataContractResolver`  
  
## <a name="see-also"></a>Consulte também

- [Usar um resolvedor de contrato de dados](../feature-details/using-a-data-contract-resolver.md)
