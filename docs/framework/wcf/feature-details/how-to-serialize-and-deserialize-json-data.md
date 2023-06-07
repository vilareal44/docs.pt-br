---
title: 'Como: usar DataContractJsonSerializer'
description: Saiba como serializar objetos de tipo .NET em dados codificados em JSON e, em seguida, desserializar esses dados de volta em instâncias de tipos .NET.
ms.date: 03/25/2019
ms.assetid: 88abc1fb-8196-4ee3-a23b-c6934144d1dd
ms.openlocfilehash: 4ffa0e9dec0a677a38d244b4a0da476d91852da5
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246799"
---
# <a name="how-to-use-datacontractjsonserializer"></a>Como usar o DataContractJsonSerializer

> [!NOTE]
> Este artigo é sobre <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> . Para a maioria dos cenários que envolvem a serialização e desserialização do JSON, recomendamos as APIs na [System.Text.Jsno namespace](../../../standard/serialization/system-text-json-overview.md).

JSON (JavaScript Object Notation) é um formato eficiente de codificação de dados que permite a troca rápida de pequenas quantidades de dados entre navegadores cliente e serviços Web habilitados para AJAX.

Este artigo demonstra como serializar objetos do tipo .NET em dados codificados em JSON e, em seguida, desserializar os dados no formato JSON de volta em instâncias de tipos .NET. Este exemplo usa um contrato de dados para demonstrar a serialização e a desserialização de um tipo definido pelo usuário `Person` e usa o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> .

Normalmente, a serialização e desserialização JSON são manipuladas automaticamente pelo Windows Communication Foundation (WCF) quando você usa tipos de contrato de dados em operações de serviço que são expostas em pontos de extremidade habilitados para AJAX. No entanto, em alguns casos, talvez seja necessário trabalhar diretamente com dados JSON.

Este artigo se baseia no [exemplo de DataContractJsonSerializer](../samples/json-serialization.md).

## <a name="to-define-the-data-contract-for-a-person-type"></a>Para definir o contrato de dados para um tipo Person

1. Defina o contrato de dados para `Person` anexando <xref:System.Runtime.Serialization.DataContractAttribute> à classe e o atributo <xref:System.Runtime.Serialization.DataMemberAttribute> aos membros que você deseja serializar. Para obter mais informações sobre contratos de dados, consulte [Designing Service Contracts](../designing-service-contracts.md).

    ```csharp
    [DataContract]
    internal class Person
    {
        [DataMember]
        internal string name;

        [DataMember]
        internal int age;
    }
    ```

## <a name="to-serialize-an-instance-of-type-person-to-json"></a>Para serializar uma instância do tipo Person para JSON

> [!NOTE]
> Se ocorrer um erro durante a serialização de uma resposta de saída no servidor ou por algum outro motivo, ele não poderá ser retornado ao cliente como uma falha.

1. Crie uma instância do tipo `Person`.

    ```csharp
    var p = new Person();
    p.name = "John";
    p.age = 42;
    ```

2. Serialize o `Person` objeto para um fluxo de memória usando o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> .

    ```csharp
    var stream1 = new MemoryStream();
    var ser = new DataContractJsonSerializer(typeof(Person));
    ```

3. Use o método <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject%2A> para gravar dados JSON no fluxo.

    ```csharp
    ser.WriteObject(stream1, p);
    ```

4. Mostre a saída JSON.

    ```csharp
    stream1.Position = 0;
    var sr = new StreamReader(stream1);
    Console.Write("JSON form of Person object: ");
    Console.WriteLine(sr.ReadToEnd());
    ```

## <a name="to-deserialize-an-instance-of-type-person-from-json"></a>Para desserializar uma instância do tipo Person de JSON

1. Desserialize os dados codificados por JSON em uma nova instância de `Person` usando o método <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.ReadObject%2A> da classe <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>.

    ```csharp
    stream1.Position = 0;
    var p2 = (Person)ser.ReadObject(stream1);
    ```

2. Mostre os resultados.

    ```csharp
    Console.WriteLine($"Deserialized back, got name={p2.name}, age={p2.age}");
    ```

## <a name="example"></a>Exemplo

```csharp
// Create a User object and serialize it to a JSON stream.
public static string WriteFromObject()
{
    // Create User object.
    var user = new User("Bob", 42);

    // Create a stream to serialize the object to.
    var ms = new MemoryStream();

    // Serializer the User object to the stream.
    var ser = new DataContractJsonSerializer(typeof(User));
    ser.WriteObject(ms, user);
    byte[] json = ms.ToArray();
    ms.Close();
    return Encoding.UTF8.GetString(json, 0, json.Length);
}

// Deserialize a JSON stream to a User object.
public static User ReadToObject(string json)
{
    var deserializedUser = new User();
    var ms = new MemoryStream(Encoding.UTF8.GetBytes(json));
    var ser = new DataContractJsonSerializer(deserializedUser.GetType());
    deserializedUser = ser.ReadObject(ms) as User;
    ms.Close();
    return deserializedUser;
}
```

> [!NOTE]
> O serializador JSON gera uma exceção de serialização para contratos de dados que têm vários membros com o mesmo nome, como mostrado no código de exemplo a seguir.

```csharp
[DataContract]
public class TestDuplicateDataBase
{
    [DataMember]
    public int field1 = 123;
}

[DataContract]
public class TestDuplicateDataDerived : TestDuplicateDataBase
{
    [DataMember]
    public new int field1 = 999;
}
```

## <a name="see-also"></a>Veja também

- [Serialização JSON no .NET](../../../standard/serialization/system-text-json-overview.md)
