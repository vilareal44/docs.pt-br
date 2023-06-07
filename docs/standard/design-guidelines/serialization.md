---
title: Serialização
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: bebb27ac-9712-4196-9931-de19fc04dbac
ms.openlocfilehash: d07549da371e403adca089c601ee5b028b268086
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291676"
---
# <a name="serialization"></a>Serialização
A serialização é o processo de converter um objeto em um formato que pode ser prontamente persistente ou transportado. Por exemplo, você pode serializar um objeto, transportá-lo pela Internet usando HTTP e desserializado no computador de destino.

 O .NET Framework oferece três tecnologias de serialização principais otimizadas para vários cenários de serialização. A tabela a seguir lista essas tecnologias e os principais tipos de estrutura relacionados a elas.

|**Nome da tecnologia**|**Tipos principais**|**Cenários**|
|-------------------------|--------------------|-------------------|
|**Serialização do contrato de dados**|<xref:System.Runtime.Serialization.DataContractAttribute> <br /> <xref:System.Runtime.Serialization.DataMemberAttribute> <br /> <xref:System.Runtime.Serialization.DataContractSerializer> <br /> <xref:System.Runtime.Serialization.NetDataContractSerializer> <br /> <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> <br /> <xref:System.Runtime.Serialization.ISerializable>|Persistência geral<br />Serviços Web<br />JSON|
|**Serialização XML**|<xref:System.Xml.Serialization.XmlSerializer>|Formato XML com controle total sobre a forma do XML|
|**Serialização de tempo de execução (Binary e SOAP)**|<xref:System.SerializableAttribute> <br /> <xref:System.Runtime.Serialization.ISerializable> <br /> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> <br /> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>|Comunicação remota .NET|

 ✔️ Pense na serialização quando cria novos tipos.

## <a name="choosing-the-right-serialization-technology-to-support"></a>Escolhendo a tecnologia de serialização correta que receberá suporte
 ✔️ Considere a possibilidade de oferecer suporte à serialização de contrato de dados se instâncias do seu tipo talvez precisem ser persistentes ou usadas em serviços da Web.

 ✔️ Considere dar suporte à serialização XML em vez de ou além da serialização de contrato de dados se precisar de mais controle sobre o formato XML que é produzido quando o tipo é serializado.

 Isso pode ser necessário em alguns cenários de interoperabilidade em que você precisa usar uma construção XML que não tenha suporte da serialização de contrato de dados, por exemplo, para produzir atributos XML.

 ✔️ CONSIDERAR o suporte à serialização em tempo de execução se instâncias do seu tipo precisarem percorrer os limites de comunicação remota do .NET.

 ❌Evite o suporte à serialização de tempo de execução ou serialização de XML apenas por motivos de persistência geral. Em vez disso, prefira a serialização do contrato de dados.

## <a name="supporting-data-contract-serialization"></a>Oferecendo suporte à serialização do contrato de dados
 Os tipos podem dar suporte à serialização de contrato de dados aplicando o <xref:System.Runtime.Serialization.DataContractAttribute> ao tipo e ao <xref:System.Runtime.Serialization.DataMemberAttribute> para os membros (campos e propriedades) do tipo.

 ✔️ Considere a marcação de membros de dados do seu tipo público se o tipo puder ser usado em confiança parcial.

 Em confiança total, os serializadores de contrato de dados podem serializar e desserializar tipos e membros não públicos, mas somente membros públicos podem ser serializados e desserializados em confiança parcial.

 ✔️ Implemente um getter e setter em todas as propriedades que têm <xref:System.Runtime.Serialization.DataMemberAttribute> . Os serializadores de contrato de dados exigem que o getter e o setter do tipo sejam considerados serializáveis. (No .NET Framework 3,5 SP1, algumas propriedades de coleção podem ser somente obtenção.) Se o tipo não for usado em confiança parcial, um ou ambos os acessadores de propriedade poderão ser não públicos.

 ✔️ Considere usar os retornos de chamada de serialização para a inicialização de instâncias desserializadas.

 Os construtores não são chamados quando os objetos são desserializados. (Há exceções para a regra. Os construtores de coleções marcadas com <xref:System.Runtime.Serialization.CollectionDataContractAttribute> são chamados durante a desserialização.) Portanto, qualquer lógica executada durante a construção normal precisa ser implementada como um dos retornos de chamada de serialização.

 `OnDeserializedAttribute`é o atributo de retorno de chamada mais comumente usado. Os outros atributos da família são <xref:System.Runtime.Serialization.OnDeserializingAttribute>, <xref:System.Runtime.Serialization.OnSerializingAttribute> e <xref:System.Runtime.Serialization.OnSerializedAttribute>. Eles podem ser usados para marcar os retornos de chamada que são executados antes de desserialização, antes da serialização e, por fim, após a serialização, respectivamente.

 ✔️ Considere usar o <xref:System.Runtime.Serialization.KnownTypeAttribute> para indicar tipos concretos que devem ser usados ao desserializar um grafo de objeto complexo.

 ✔️ Considere a compatibilidade com versões anteriores e posteriores ao criar ou alterar tipos serializáveis.

 Tenha em mente que os fluxos serializados de versões futuras do tipo podem ser desserializados na versão atual de tipo e vice-versa.

 Certifique-se de entender que os membros de dados, mesmo privados e internos, não podem alterar seus nomes, tipos ou até mesmo seus pedidos em versões futuras do tipo, a menos que seja levado um cuidado especial para preservar o contrato usando parâmetros explícitos para os atributos de contrato de dados.

 Teste a compatibilidade da serialização ao fazer alterações em tipos serializáveis. Tente desserializar a nova versão em uma versão antiga e vice-versa.

 ✔️ Considere <xref:System.Runtime.Serialization.IExtensibleDataObject> a implementação para permitir o ida e volta entre diferentes versões do tipo.

 A interface permite que o serializador assegure de que nenhum dado sejam perdido durante o ciclo completo. A <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A?displayProperty=nameWithType> propriedade é usada para armazenar todos os dados da versão futura do tipo que é desconhecido para a versão atual e, portanto, não pode armazená-los em seus membros de dados. Quando a versão atual for posteriormente serializada e desserializada em uma versão futura, os dados adicionais estarão disponíveis no fluxo serializado.

## <a name="supporting-xml-serialization"></a>Suporte à serialização XML
 A serialização de contrato de dados é a tecnologia de serialização principal (padrão) no .NET Framework, mas há cenários de serialização para os quais a serialização de contrato de dados não oferece suporte. Por exemplo, ela não fornece controle total sobre o formato XML gerado ou consumido pelo serializador. Se esse controle fino for necessário, a serialização de XML precisará ser usada e você precisará projetar seus tipos para dar suporte a essa tecnologia de serialização.

 ❌Evite criar seus tipos especificamente para a serialização XML, a menos que você tenha um motivo muito forte para controlar a forma do XML produzido. Essa tecnologia de serialização foi substituída pela serialização do contrato de dados abordada na seção anterior.

 ✔️ Considere a implementação da <xref:System.Xml.Serialization.IXmlSerializable> interface se desejar ter ainda mais controle sobre a forma do XML serializado do que a oferecida aplicando os atributos de SERIALIZAÇÃO XML. Dois métodos da interface <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> e <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> permitem controlar totalmente o fluxo XML serializado. Você também pode controlar o esquema XML que é gerado para o tipo aplicando o `XmlSchemaProviderAttribute` .

## <a name="supporting-runtime-serialization"></a>Suporte à serialização em runtime
 A serialização de tempo de execução é uma tecnologia usada pela comunicação remota do .NET. Se você considerar que seus tipos serão transportados usando a comunicação remota do .NET, precisará certificar-se de que eles oferecem suporte à serialização de tempo de execução.

 O suporte básico para serialização de tempo de execução pode ser fornecido aplicando-se os <xref:System.SerializableAttribute> cenários e mais avançados envolvem a implementação de um padrão serializável de tempo de execução simples (implemente <xref:System.Runtime.Serialization.ISerializable> e forneça o construtor de serialização).

 ✔️ Considere o suporte à serialização de tempo de execução se seus tipos forem usados com a comunicação remota do .NET. Por exemplo, o <xref:System.AddIn?displayProperty=nameWithType> namespace usa a comunicação remota do .net e, portanto, todos os tipos trocados entre os `System.AddIn` suplementos precisam dar suporte à serialização de tempo de execução.

 ✔️ Considere a implementação do padrão serializável de tempo de execução se desejar ter controle total sobre o processo de serialização. Por exemplo, se você quiser transformar os dados à medida que eles forem serializados ou desserializados.

 O padrão é muito simples. Tudo o que você precisa fazer é implementar a interface <xref:System.Runtime.Serialization.ISerializable> e fornecer um construtor especial que é usado quando o objeto é desserializado.

 ✔️ tornar o construtor de serialização protegido e fornecer dois parâmetros digitados e nomeados exatamente conforme mostrado no exemplo aqui.

```csharp
[Serializable]
public class Person : ISerializable
{
    protected Person(SerializationInfo info, StreamingContext context)
    {
        // ...
    }
}
```

 ✔️ Implemente os <xref:System.Runtime.Serialization.ISerializable> Membros explicitamente.

 ✔️ aplicar uma demanda de link à <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A?displayProperty=nameWithType> implementação. Isso garante que apenas núcleos totalmente confiáveis e o serializador de tempo de execução tenham acesso ao membro.

 *Partes © 2005, 2009 Microsoft Corporation. Todos os direitos reservados.*

 *Reimpresso com permissão da Pearson Education, Inc. das [Diretrizes de Design do Framework: convenções, linguagens e padrões para bibliotecas do .NET reutilizável, 2ª edição](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) por Krzysztof Cwalina e Brad Abrams, publicado em 22 de outubro de 2008 por Addison-Wesley Professional como parte da série de desenvolvimento do Microsoft Windows.*

## <a name="see-also"></a>Veja também

- [Diretrizes de design de estrutura](index.md)
- [Diretrizes de uso](usage-guidelines.md)
