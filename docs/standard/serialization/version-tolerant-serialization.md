---
title: Serialização tolerante a versão
description: O .NET Framework 2,0 apresenta a serialização tolerante a versão, um conjunto de recursos que facilita a modificação de tipos serializáveis.
ms.date: 08/08/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- version tolerant serialization
- serialization, custom serialization
- serialization, version tolerant
- serialization, controlling
- versions and serialization
- BinaryFormatter class, samples
- serialization, attributes
ms.assetid: bea0ffe3-2708-4a16-ac7d-e586ed6b8e8d
ms.openlocfilehash: afc822e1f8873bac069f6634fdf1d4665d392e69
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83762585"
---
# <a name="version-tolerant-serialization"></a>Serialização tolerante a versão

Na versão 1.0 e 1.1 do .NET Framework, criar tipos serializáveis que fossem reutilizáveis de uma versão de um aplicativo para a outra era problemático. Se um tipo tivesse sido modificado adicionando campos extras, o seguinte problema ocorreria.

- As versões antigas de um aplicativo gerariam exceções quando fossem solicitadas a desserializar novas versões do tipo antigo.
- As versões recentes de um aplicativo gerariam exceções ao desserializar versões antigas de um tipo com dados ausentes.

VTS (Version Tolerant Serialization) é um conjunto de recursos introduzido no .NET Framework 2.0 que facilita, ao longo do tempo, modificar tipos serializáveis. Especificamente, os recursos do VTS serão habilitados para classes para as quais o atributo <xref:System.SerializableAttribute> tiver sido aplicado, incluindo tipos genéricos. O VTS possibilita adicionar novos campos a essas classes sem interromper a compatibilidade com outras versões do tipo. Para obter um aplicativo de exemplo funcional, consulte [Amostra de tecnologia de serialização tolerante a versão](basic-serialization-technology-sample.md).

Os recursos do VTS são habilitados ao usar o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>. Além disso, todos os recursos exceto tolerância a dados desconhecidos também são habilitados ao usar o <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>. Para obter mais informações sobre como usar essas classes para serialização, consulte [Serialização binária](binary-serialization.md).

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

## <a name="feature-list"></a>Lista de recursos

O conjunto de recursos inclui o seguinte:

- Tolerância a dados desconhecidos ou inesperados. Isso permite que versões mais recentes do tipo enviem dados a versões mais antigas.
- Tolerância de dados opcionais ausentes. Isso permite que versões mais antigas enviem dados a versões mais recentes.
- Retornos de chamada de serialização. Isso permite a configuração inteligente de valor padrão em casos onde os dados estão ausentes.

Além disso, há um recurso para declarar quando um novo campo opcional tiver sido adicionado. Essa é a propriedade <xref:System.Runtime.Serialization.OptionalFieldAttribute.VersionAdded%2A> do atributo <xref:System.Runtime.Serialization.OptionalFieldAttribute>.

Esses recursos são discutidos mais detalhadamente nas seções a seguir.

### <a name="tolerance-of-extraneous-or-unexpected-data"></a>Tolerância a dados desconhecidos ou inesperados

No passado, durante a desserialização, os dados desconhecidos ou inesperados fizeram exceções serem geradas. Com o VTS, na mesma situação, os dados desconhecidos ou inesperados eram ignorados em vez de fazerem exceções serem geradas. Isso permite que aplicativos que usam versões mais recentes de um tipo (ou seja, uma versão que inclua mais campos) enviem informações para aplicativos que esperam versões mais recentes do mesmo tipo.

No exemplo a seguir, os dados extras contidos no `CountryField` da versão 2.0 da classe `Address` são ignorados quando um aplicativo mais antigo desserializa a versão mais recente.

```csharp  
// Version 1 of the Address class.  
[Serializable]  
public class Address  
{  
    public string Street;  
    public string City;  
}  
// Version 2.0 of the Address class.  
[Serializable]  
public class Address  
{  
    public string Street;  
    public string City;  
    // The older application ignores this data.  
    public string CountryField;  
}  
```  
  
```vb  
' Version 1 of the Address class.  
<Serializable> _  
Public Class Address  
    Public Street As String  
    Public City As String  
End Class  
  
' Version 2.0 of the Address class.  
<Serializable> _  
Public Class Address  
    Public Street As String  
    Public City As String  
    ' The older application ignores this data.  
    Public CountryField As String  
End Class  
```  

### <a name="tolerance-of-missing-data"></a>Tolerância de dados ausentes

Os campos podem ser marcados como opcionais aplicando o atributo <xref:System.Runtime.Serialization.OptionalFieldAttribute> a eles. Durante a desserialização, se os dados opcionais estiverem ausentes, o mecanismo de serialização ignorará a ausência e não gerará uma exceção. Assim, os aplicativos que esperam versões mais recentes de um tipo podem enviar dados para aplicativos que esperam versões mais recentes do mesmo tipo.

O exemplo a seguir mostra uma versão 2.0 da classe `Address` com o campo `CountryField` marcado como opcional. Se um aplicativo mais antigo enviar a versão 1 para um aplicativo mais recente que espera a versão 2.0, a ausência dos dados será ignorada.

```csharp
[Serializable]
public class Address
{
    public string Street;
    public string City;

    [OptionalField]
    public string CountryField;
}
```

```vb
<Serializable> _
Public Class Address
    Public Street As String
    Public City As String

    <OptionalField> _
    Public CountryField As String
End Class
```
  
### <a name="serialization-callbacks"></a>Retornos de chamada de serialização

Os retornos de chamada de serialização são um mecanismo que fornece ganchos no processo de serialização/desserialização em quatro pontos.

|Atributo|Quando o método associado é chamado|Usos comum|
|---------------|------------------------------------------|-----------------|
|<xref:System.Runtime.Serialization.OnDeserializingAttribute>|Antes da desserialização.\*|Inicialize valores padrão para campos opcionais.|
|<xref:System.Runtime.Serialization.OnDeserializedAttribute>|Depois da desserialização.|Corrija valores de campo opcionais com base nos conteúdos de outros campos.|
|<xref:System.Runtime.Serialization.OnSerializingAttribute>|Antes da serialização.|Prepare para a serialização. Por exemplo, crie estruturas de dados opcionais.|
|<xref:System.Runtime.Serialization.OnSerializedAttribute>|Depois da serialização.|Registre os eventos de serialização.|

 \* Esse retorno de chamada é invocado antes do construtor de desserialização, se houver.

#### <a name="using-callbacks"></a>Usando retornos de chamada

Para usar retornos de chamada, aplique o atributo apropriado a um método que aceita um parâmetro <xref:System.Runtime.Serialization.StreamingContext>. Somente um método por classe pode ser marcado com cada um desses atributos. Por exemplo:

```csharp
[OnDeserializing]
private void SetCountryRegionDefault(StreamingContext sc)
{
    CountryField = "Japan";
}
```

```vb
<OnDeserializing>
Private Sub SetCountryRegionDefault(sc As StreamingContext)
    CountryField = "Japan"
End Sub
```

O uso pretendido desses métodos é para controle de versão. Durante a desserialização, um campo opcional pode não estar corretamente inicializado se os dados para o campo estiverem ausentes. Isso pode ser corrigido com a criação do método que atribui o valor correto e, em seguida, com a aplicação do atributo **OnDeserializingAttribute** ou **OnDeserializedAttribute** ao método.

O exemplo a seguir mostra o método no contexto de um tipo. Se uma versão anterior de um aplicativo enviar uma instância da classe `Address` para uma versão posterior do aplicativo, os dados do campo `CountryField` ficarão faltando. Mas, após a desserialização, o campo será definido como um valor padrão "Japão".

```csharp
[Serializable]
public class Address
{
    public string Street;
    public string City;
    [OptionalField]
    public string CountryField;

    [OnDeserializing]
    private void SetCountryRegionDefault(StreamingContext sc)
    {
        CountryField = "Japan";
    }
}
```

```vb
<Serializable> _
Public Class Address
    Public Street As String
    Public City As String
    <OptionalField> _
    Public CountryField As String

    <OnDeserializing> _
    Private Sub SetCountryRegionDefault(sc As StreamingContext)
        CountryField = "Japan"
    End Sub
End Class
```

## <a name="the-versionadded-property"></a>A propriedade VersionAdded

O **OptionalFieldAttribute** tem a propriedade **VersionAdded**. Na versão 2.0 do .NET Framework, isso não é usado. No entanto, é importante definir essa propriedade corretamente para verificar se o tipo será compatível com mecanismos de serialização futuros.

A propriedade indica a qual versão de um tipo um determinado campo foi adicionado. Ele deve ser incrementado exatamente em um (a partir de 2) a cada vez que o tipo é modificado, conforme mostrado no seguinte exemplo:

```csharp
// Version 1.0
[Serializable]
public class Person
{
    public string FullName;
}

// Version 2.0
[Serializable]
public class Person
{
    public string FullName;

    [OptionalField(VersionAdded = 2)]
    public string NickName;
    [OptionalField(VersionAdded = 2)]
    public DateTime BirthDate;
}

// Version 3.0
[Serializable]
public class Person
{
    public string FullName;

    [OptionalField(VersionAdded=2)]
    public string NickName;
    [OptionalField(VersionAdded=2)]
    public DateTime BirthDate;

    [OptionalField(VersionAdded=3)]
    public int Weight;
}
```

```vb
' Version 1.0
<Serializable> _
Public Class Person
    Public FullName
End Class

' Version 2.0
<Serializable> _
Public Class Person
    Public FullName As String

    <OptionalField(VersionAdded := 2)> _
    Public NickName As String
    <OptionalField(VersionAdded := 2)> _
    Public BirthDate As DateTime
End Class

' Version 3.0
<Serializable> _
Public Class Person
    Public FullName As String

    <OptionalField(VersionAdded := 2)> _
    Public NickName As String
    <OptionalField(VersionAdded := 2)> _
    Public BirthDate As DateTime

    <OptionalField(VersionAdded := 3)> _
    Public Weight As Integer
End Class
```

## <a name="serializationbinder"></a>SerializationBinder

Alguns usuários podem precisar controlar qual classe serializar e desserializar porque uma versão diferente da classe é necessária no servidor e no cliente. O <xref:System.Runtime.Serialization.SerializationBinder> é uma classe abstrata usada para controlar os tipos reais usados durante a serialização e a desserialização. Para usar essa classe, derive uma classe de <xref:System.Runtime.Serialization.SerializationBinder> e substitua os métodos <xref:System.Runtime.Serialization.SerializationBinder.BindToName%2A> e <xref:System.Runtime.Serialization.SerializationBinder.BindToType%2A>. Para obter mais informações, consulte [controlando a serialização e desserialização com o SerializationBinder](../../framework/wcf/feature-details/controlling-serialization-and-deserialization-with-serializationbinder.md).

## <a name="best-practices"></a>Práticas recomendadas

Para garantir um comportamento de controle de versão apropriado, siga essas regras ao modificar um tipo de versão para versão:

- Nunca remova um campo serializado.
- Nunca aplique o atributo <xref:System.NonSerializedAttribute> a um campo se o atributo não tiver sido aplicado ao campo na versão anterior.
- Nunca altere o nome ou o tipo de um campo serializado.
- Ao adicionar um novo campo serializado, aplique o atributo **OptionalFieldAttribute**.
- Ao remover um atributo **NonSerializedAttribute** de um campo (que não era serializável em uma versão anterior), aplique o atributo **OptionalFieldAttribute**.
- Para todos os campos opcionais, defina padrões significativos usando os retornos de chamada de serialização, a menos que 0 ou **null** sejam aceitáveis como padrões.

Para definir que um tipo seja compatível com mecanismos de serialização futuros, siga essas diretrizes:

- Sempre defina a propriedade **VersionAdded** no atributo **OptionalFieldAttribute** corretamente.
- Evite controle de versão ramificado.

## <a name="see-also"></a>Confira também

- <xref:System.SerializableAttribute>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
- <xref:System.Runtime.Serialization.OptionalFieldAttribute.VersionAdded%2A>
- <xref:System.Runtime.Serialization.OptionalFieldAttribute>
- <xref:System.Runtime.Serialization.OnDeserializingAttribute>
- <xref:System.Runtime.Serialization.OnDeserializedAttribute>
- <xref:System.Runtime.Serialization.OnSerializingAttribute>
- <xref:System.Runtime.Serialization.OnSerializedAttribute>
- <xref:System.Runtime.Serialization.StreamingContext>
- <xref:System.NonSerializedAttribute>
- [Serialização binária](binary-serialization.md)
