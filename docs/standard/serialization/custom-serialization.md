---
title: Serialização personalizada
description: A serialização personalizada está controlando a serialização e a desserialização de um tipo. Controlar a serialização pode garantir a compatibilidade da serialização.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- binary serialization, custom serialization
- custom serialization
- binary serialization, controlling
- OptionalFieldAttribute class, custom serialization
- ISerializable interface, custom serialization
- OnDeserializingAttribute class, custom serialization
- OnSerializedAttribute class, custom serialization
- serialization, custom serialization
- serialization, controlling
- OnDeserializedAttribute class, custom serialization
- OnSerializingAttribute class, custom serialization
ms.assetid: 12ed422d-5280-49b8-9b71-a2ed129c0384
ms.openlocfilehash: 1532c4eeb09e7110d0f369ec47f342256889e576
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289649"
---
# <a name="custom-serialization"></a>Serialização personalizada
A serialização personalizada é o processo de controlar a serialização e a desserialização de um tipo. Controlando a serialização, é possível garantir a compatibilidade de serialização, que é a capacidade de serializar e desserializar entre versões de um tipo sem interromper a funcionalidade básica do tipo. Por exemplo, na primeira versão de um tipo, pode haver apenas dois campos. Na próxima versão de um tipo, vários outros campos são adicionados. No entanto, a segunda versão de um aplicativo deve ser capaz de serializar e desserializar ambos os tipos. As seções a seguir descrevem como controlar a serialização.

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
> [!IMPORTANT]
> Nas versões anteriores ao .NET Framework 4.0, a serialização de dados personalizados do usuário em um assembly parcialmente confiável era realizada com o GetObjectData. Iniciando com a versão 4.0, esse método é marcado com o atributo <xref:System.Security.SecurityCriticalAttribute> que impede a execução em assemblies parcialmente confiáveis. Para contornar esta condição, implemente a interface <xref:System.Runtime.Serialization.ISafeSerializationData>.  
  
## <a name="running-custom-methods-during-and-after-serialization"></a>Executando métodos personalizados durante e após a serialização  
 A prática recomendada e a maneira mais fácil (introduzidas na versão 2.0 do .NET Framework) é aplicar os seguintes atributos a métodos que são usados para corrigir dados durante e após a serialização:  
  
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>  
  
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>  
  
- <xref:System.Runtime.Serialization.OnSerializedAttribute>  
  
- <xref:System.Runtime.Serialization.OnSerializingAttribute>  
  
 Esses atributos permitem que o tipo participe de qualquer uma ou todas as quatro fases dos processos de serialização e desserialização. Os atributos especificam os métodos do tipo que devem ser chamados durante cada fase. Os métodos não acessam o fluxo de serialização. Em vez disso, permitem alterar o objeto antes e depois da serialização, ou antes e depois da desserialização. Os atributos podem ser aplicados a todos os níveis da hierarquia de herança do tipo e cada método é chamado na hierarquia da base para a mais derivada. Esse mecanismo impede a complexidade e os problemas resultantes de se implementar a interface <xref:System.Runtime.Serialization.ISerializable> dando a responsabilidade para serialização e desserialização à implementação mais derivada. Além disso, esse mecanismo permite que os formatadores ignorem a população de campos e de recuperação do fluxo de serialização. Para obter detalhes e exemplos de controle de serialização e desserialização, clique em um dos links anteriores.  
  
 Além disso, ao adicionar um novo campo a um tipo serializável existente, aplique o atributo <xref:System.Runtime.Serialization.OptionalFieldAttribute> ao campo. <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> e <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> ignoram a ausência do campo quando um fluxo que não tem o novo campo é processado.  
  
## <a name="implementing-the-iserializable-interface"></a>Implementando a interface ISerializable  
 A outra maneira de controlar a serialização é obtida pela implementação da interface <xref:System.Runtime.Serialization.ISerializable> em um objeto. Observe, no entanto, que o método na seção anterior substitui esse método para controlar a serialização.  
  
 Além disso, você não deve usar a serialização padrão em uma classe que está marcada com o atributo [Serializable](xref:System.SerializableAttribute) e que tem segurança declarativa ou imperativa no nível da classe ou em seus construtores. Em vez disso, essas classes sempre devem implementar a interface <xref:System.Runtime.Serialization.ISerializable>.  
  
 A implementação de <xref:System.Runtime.Serialization.ISerializable> envolve a implementação do método `GetObjectData` e um construtor especial que é usado quando o objeto é desserializado. O código de exemplo a seguir mostra como implementar <xref:System.Runtime.Serialization.ISerializable> na classe `MyObject` de uma seção anterior.  
  
```csharp
[Serializable]
public class MyObject : ISerializable
{
    public int n1;
    public int n2;
    public String str;

    public MyObject()
    {
    }

    protected MyObject(SerializationInfo info, StreamingContext context)
    {
      n1 = info.GetInt32("i");
      n2 = info.GetInt32("j");
      str = info.GetString("k");
    }

    [SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter = true)]
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context)
    {
        info.AddValue("i", n1);
        info.AddValue("j", n2);
        info.AddValue("k", str);
    }
}
```

```vb
<Serializable()>  _
Public Class MyObject
    Implements ISerializable
    Public n1 As Integer
    Public n2 As Integer
    Public str As String

    Public Sub New()
    End Sub

    Protected Sub New(ByVal info As SerializationInfo, ByVal context As StreamingContext)
        n1 = info.GetInt32("i")
        n2 = info.GetInt32("j")
        str = info.GetString("k")
    End Sub 'New

    <SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)> _
    Public Overridable Sub GetObjectData(ByVal info As SerializationInfo, ByVal context As StreamingContext)
        info.AddValue("i", n1)
        info.AddValue("j", n2)
        info.AddValue("k", str)
    End Sub
End Class
```  
  
 Quando **GetObjectData** é chamado durante a serialização, você é responsável por popular as <xref:System.Runtime.Serialization.SerializationInfo> fornecidas com a chamada de método. Adicione as variáveis a serem serializadas como pares de nome e valor. Qualquer texto pode ser usado como o nome. Você tem a liberdade para decidir quais variáveis de membro serão adicionadas às <xref:System.Runtime.Serialization.SerializationInfo>, desde que os dados suficientes sejam serializados para restaurar o objeto durante a desserialização. As classes derivadas deverão chamar o método **GetObjectData** no objeto base se o último implementar <xref:System.Runtime.Serialization.ISerializable>.  
  
 Observe que a serialização pode permitir que outro código veja ou modifique os dados da instância de objeto que estejam de outra forma inacessíveis. Portanto, o código que executa a serialização exige a [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) com o sinalizador <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> especificado. De acordo com a política padrão, essa permissão não é dada ao código da intranet ou baixado da Internet; somente o código no computador local recebe essa permissão. O método **GetObjectData** deve ser explicitamente protegido por meio da exigência de [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) com o sinalizador <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> especificado ou de outras permissões que especificamente ajudam a proteger dados particulares.  
  
 Se um campo particular armazenar informações confidenciais, você deverá exigir as permissões apropriadas no **GetObjectData** para proteger os dados. Lembre-se de que o código que recebeu a [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) com o sinalizador **SerializationFormatter** especificado pode exibir e modificar os dados armazenados em campos particulares. Um chamador mal-intencionado que recebeu esta [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) pode exibir dados, como locais de diretório ocultos ou permissões concedidas, e usar os dados para explorar uma vulnerabilidade de segurança no computador. Para obter uma lista completa dos sinalizadores de permissões de segurança que podem ser especificados, consulte a [Enumeração SecurityPermissionFlag](xref:System.Security.Permissions.SecurityPermissionFlag).  
  
 É importante ressaltar que, quando <xref:System.Runtime.Serialization.ISerializable> é adicionado a uma classe, é necessário implementar **GetObjectData** e o construtor especial. O compilador avisa se **GetObjectData** está ausente. No entanto, como é impossível impor a implementação de um construtor, nenhum aviso será fornecido se o construtor estiver ausente, e uma exceção é gerada quando é feita uma tentativa de desserializar uma classe sem o construtor.  
  
 O projeto atual foi favorecido em relação a um método <xref:System.Runtime.Serialization.ISerializationSurrogate.SetObjectData%2A> para solucionar possíveis problemas de segurança e controle de versão. Por exemplo, um método `SetObjectData` deverá ser público se estiver definido como parte de uma interface; portanto, os usuários devem escrever o código para impedir que o método **SetObjectData** seja chamado várias vezes. Caso contrário, um aplicativo mal-intencionado que chama o método **SetObjectData** em um objeto durante a execução de uma operação poderá causar problemas potenciais.  
  
 Durante a desserialização, <xref:System.Runtime.Serialization.SerializationInfo> é passado para a classe usando o construtor fornecido para essa finalidade. Todas as restrições de visibilidade colocadas no construtor são ignorados quando o objeto é desserializado; assim você pode marcar a classe como pública, protegida, interna ou privada. No entanto, é uma prática recomendada marcar o construtor como protegido a menos que a classe seja fechada; nesse caso, o construtor é marcado particular. O construtor também deve executar a validação completa de entrada. Para evitar o uso indevido pelo código mal-intencionado, o construtor deve aplicar as mesmas verificações de segurança e permissões necessárias para obter uma instância da classe usando qualquer outro construtor. Se você não seguir essa recomendação, o código mal-intencionado poderá pré-serializar um objeto, obter o controle com a [SecurityPermission](xref:System.Security.Permissions.SecurityPermissionAttribute) com o sinalizador <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter> especificado e desserializar o objeto em um computador cliente, ignorando qualquer segurança que tenha sido aplicada durante a construção de instância padrão com um construtor público.  
  
 Para restaurar o estado do objeto, basta recuperar os valores das variáveis de <xref:System.Runtime.Serialization.SerializationInfo> usando os nomes usados durante a serialização. Se a classe base implementar o <xref:System.Runtime.Serialization.ISerializable>, o construtor de base deverá ser chamado para permitir que o objeto base restaure suas variáveis.  
  
 Quando você derivar uma nova classe de uma que implementa <xref:System.Runtime.Serialization.ISerializable>, a classe derivada deverá implementar o construtor, bem como o método **GetObjectData**, se tiver variáveis que precisam ser serializadas. O exemplo de código a seguir mostra como isso é feito usando a classe `MyObject` mostrada anteriormente.  
  
```csharp
[Serializable]
public class ObjectTwo : MyObject
{
    public int num;

    public ObjectTwo()
      : base()
    {
    }

    protected ObjectTwo(SerializationInfo si, StreamingContext context)
      : base(si, context)
    {
        num = si.GetInt32("num");
    }

    [SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter = true)]
    public override void GetObjectData(SerializationInfo si, StreamingContext context)
    {
        base.GetObjectData(si,context);
        si.AddValue("num", num);
    }
}
```

```vb
<Serializable()>  _
Public Class ObjectTwo
    Inherits MyObject
    Public num As Integer

    Public Sub New()

    End Sub

    Protected Sub New(ByVal si As SerializationInfo, _
    ByVal context As StreamingContext)
        MyBase.New(si, context)
        num = si.GetInt32("num")
    End Sub

    <SecurityPermissionAttribute(SecurityAction.Demand, SerializationFormatter := True)> _
    Public Overrides Sub GetObjectData(ByVal si As SerializationInfo, ByVal context As StreamingContext)
        MyBase.GetObjectData(si, context)
        si.AddValue("num", num)
    End Sub
End Class
```  
  
 Não se esqueça de chamar a classe base no construtor de desserialização. Se isso não for feito, o construtor na classe base nunca será chamado e o objeto não será totalmente construído após a desserialização.  
  
 Os objetos são recriados de dentro para fora; e os métodos de chamada durante a desserialização podem ter efeitos colaterais indesejáveis, porque os métodos chamados podem se referir a referências de objeto que não foram desserializados no momento em que a chamada foi feita. Se a classe que está sendo desserializada implementar o <xref:System.Runtime.Serialization.IDeserializationCallback>, o método <xref:System.Runtime.Serialization.IDeserializationCallback.OnDeserialization%2A> será chamado automaticamente quando o gráfico do objeto inteiro tiver sido desserializado. Neste ponto, todos os objetos filho referenciados foram restaurados totalmente. Uma tabela de hash é um exemplo típico de uma classe que é difícil desserializar sem usar a escuta de eventos. É fácil recuperar os pares de chave e valor durante a desserialização, mas adicionar esses objetos de volta à tabela de hash pode causar problemas, porque não há nenhuma garantia de que as classes que derivaram da tabela de hash foram desserializadas. Os métodos de chamada em uma tabela de hash nesse estágio são, portanto, não aconselháveis.  
  
## <a name="see-also"></a>Confira também

- [Serialização binária](binary-serialization.md)
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- [Segurança e serialização](../../framework/misc/security-and-serialization.md)
