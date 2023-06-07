---
title: Diretrizes de serialização
description: Este documento fornece diretrizes a serem consideradas ao criar uma API para ser serializada e um resumo das três ofertas principais de tecnologias de serialização .NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serialization, guidelines
- binary serialization, guidelines
ms.assetid: ebbeddff-179d-443f-bf08-9c373199a73a
ms.openlocfilehash: eb11f0b8ddd34df7c6970c275d4b83cb95f59a53
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84287487"
---
# <a name="serialization-guidelines"></a>Diretrizes de serialização
Este documento lista as diretrizes a serem consideradas na criação de uma API a ser serializado.  

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
 O .NET oferece três tecnologias de serialização principais que são otimizadas para vários cenários de serialização. A tabela a seguir lista essas tecnologias e os principais tipos de .NET relacionados a elas.  
  
|Tecnologia|Classes relevantes|Observações|  
|----------------|----------------------|-----------|  
|Serialização do contrato de dados|<xref:System.Runtime.Serialization.DataContractAttribute><br /><br /> <xref:System.Runtime.Serialization.DataMemberAttribute><br /><br /> <xref:System.Runtime.Serialization.DataContractSerializer><br /><br /> <xref:System.Runtime.Serialization.NetDataContractSerializer><br /><br /> <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer><br /><br /> <xref:System.Runtime.Serialization.ISerializable>|Persistência geral<br /><br /> Serviços Web<br /><br /> JSON|  
|Serialização XML|<xref:System.Xml.Serialization.XmlSerializer>|Formato XML <br />com controle total|  
|Runtime - serialização (binário e SOAP)|<xref:System.SerializableAttribute><br /><br /> <xref:System.Runtime.Serialization.ISerializable><br /><br /> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter><br /><br /> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>|Comunicação remota .NET|  
  
 Ao criar novos tipos, você deverá decidir a quais dessas tecnologias, se houver alguma, esses tipos precisam oferecer suporte. As diretrizes a seguir descrevem como fazer essa escolha e como fornecer esse suporte. Essas diretrizes não foram criadas para ajudar você a escolher qual tecnologia de serialização usará na implementação do aplicativo ou da biblioteca. Elas não estão diretamente relacionadas ao design da API e, portanto, não estão dentro do escopo deste tópico.  
  
## <a name="guidelines"></a>Diretrizes  
  
- Pense sobre a serialização ao criar novos tipos.  
  
     A serialização é uma questão de design importante a ser considerada para qualquer tipo, pois os programas talvez precisem persistir ou transmitir instâncias do tipo.  
  
### <a name="choosing-the-right-serialization-technology-to-support"></a>Escolhendo a tecnologia de serialização correta que receberá suporte  
 Qualquer tipo específico pode oferecer suporte a nenhuma, uma ou mais tecnologias de serialização.  
  
- CONSIDERE dar suporte à *serialização de contrato de dados* se houver possibilidade de que as instâncias do seu tipo precisem ser persistentes ou usadas nos Serviços Web.  
  
- CONSIDERE dar suporte à *serialização XML* em vez da ou além da serialização do contrato de dados se você precisar ter mais controle sobre o formato XML gerado quando o tipo for serializado.  
  
     Isso talvez seja necessário em alguns cenários de interoperabilidade no qual você precise usar uma construção XML para a qual não haja suporte na serialização do contrato de dados, por exemplo, para gerar atributos XML.  
  
- CONSIDERE dar suporte à *serialização em runtime* se as instâncias do tipo precisarem ultrapassar os limites da comunicação remota .NET.  
  
- EVITE o suporte à serialização em runtime ou à serialização XML apenas por motivos gerais de persistência. Prefira a serialização do contrato de dados em vez disso  
  
#### <a name="supporting-data-contract-serialization"></a>Dando suporte à serialização do contrato de dados  
 Os tipos podem dar suporte à serialização do contrato de dados aplicando o <xref:System.Runtime.Serialization.DataContractAttribute> ao tipo e o <xref:System.Runtime.Serialization.DataMemberAttribute> aos membros (campos e propriedades) do tipo.  
  
 [!code-csharp[SerializationGuidelines#1](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#1)]
 [!code-vb[SerializationGuidelines#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#1)]  
  
1. CONSIDERE marcar os membros de dados do tipo público se o tipo puder ser usado na confiança parcial. Na confiança total, os serializadores do contrato de dados podem serializar e desserializar tipos e membros não públicos, mas somente os membros públicos podem ser serializados e desserializados na confiança parcial.  
  
2. IMPLEMENTE um getter e um setter em todas as propriedades que têm Data-MemberAttribute. Os serializadores do contrato de dados requerem o getter e o setter para o tipo a ser considerado serializável. Se o tipo não será usado na confiança parcial, um ou ambos os acessadores de propriedade poderão ser não públicos.  
  
     [!code-csharp[SerializationGuidelines#2](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#2)]
     [!code-vb[SerializationGuidelines#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#2)]  
  
3. CONSIDERE o uso de retornos de chamada de serialização para a inicialização de instâncias desserializadas.  
  
     Os construtores não são chamados quando os objetos são desserializados. Portanto, qualquer lógica que executada durante a construção normal precisa ser implementada como um dos *retornos de chamada de serialização*.  
  
     [!code-csharp[SerializationGuidelines#3](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#3)]
     [!code-vb[SerializationGuidelines#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#3)]  
  
     <xref:System.Runtime.Serialization.OnDeserializedAttribute> é o atributo de retorno de chamada mais usado. Os outros atributos da família são <xref:System.Runtime.Serialization.OnDeserializingAttribute> , <xref:System.Runtime.Serialization.OnSerializingAttribute> e <xref:System.Runtime.Serialization.OnSerializedAttribute> . Eles podem ser usados para marcar os retornos de chamada que são executados antes de desserialização, antes da serialização e, por fim, após a serialização, respectivamente.  
  
4. CONSIDERE o uso do <xref:System.Runtime.Serialization.KnownTypeAttribute> para indicar os tipos concretos que devem ser usados ao desserializar um grafo de objeto complexo.  
  
     Por exemplo, se o tipo de um membro de dados desserializado for representado por uma classe abstrata, o serializador precisará das informações de *tipo conhecido* para decidir qual tipo concreto será instanciado e atribuído ao membro. Se o tipo conhecido não for especificado por meio do atributo, ele precisará ser passado para o serializador explicitamente (você pode fazer isso passando os tipos conhecidos para o construtor do serializador) ou precisará ser especificado no arquivo de configuração.  
  
     [!code-csharp[SerializationGuidelines#4](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#4)]
     [!code-vb[SerializationGuidelines#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#4)]  
  
     Nos casos em que a lista de tipos conhecidos não é conhecida de forma estática (quando a classe **Person** é compilada), o **KnownTypeAttribute** também pode apontar para um método que retorna uma lista de tipos conhecidos em tempo de execução.  
  
5. Considere a compatibilidade com versões anteriores e posteriores ao criar ou modificar tipos serializáveis.  
  
     Tenha em mente que os fluxos serializados de versões futuras do tipo podem ser desserializados na versão atual de tipo e vice-versa. Verifique se compreendeu que os membros de dados, até mesmo os privados e internos, não podem alterar seus nomes, tipos ou mesmo sua ordem nas versões futuras do tipo, a menos que se tome um cuidado especial para preservar o contrato usando parâmetros explícitos para os atributos do contrato de dados. Teste a compatibilidade da serialização ao fazer alterações nos tipos serializáveis. Tente desserializar a nova versão em uma versão antiga e vice-versa.  
  
6. CONSIDERE implementar a interface <xref:System.Runtime.Serialization.IExtensibleDataObject> para permitir o ciclo completo entre diferentes versões do tipo.  
  
     A interface permite que o serializador assegure de que nenhum dado sejam perdido durante o ciclo completo. A propriedade <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> armazena todos os dados da versão futura do tipo desconhecido na versão atual. Quando a versão atual for serializada e desserializada subsequentemente em uma versão futura, os dados adicionais estarão disponíveis no fluxo serializado através do valor da propriedade **ExtensionData**.  
  
     [!code-csharp[SerializationGuidelines#5](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#5)]
     [!code-vb[SerializationGuidelines#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#5)]  
  
     Para obter mais informações, consulte [Contratos de dados compatíveis por encaminhamento](../../framework/wcf/feature-details/forward-compatible-data-contracts.md).  
  
#### <a name="supporting-xml-serialization"></a>Suporte à serialização XML  
 A serialização do contrato de dados é a tecnologia de serialização principal (padrão) do .NET Framework, mas há situações de serialização para as quais a serialização do contrato de dados não oferece suporte. Por exemplo, ela não fornece controle total sobre o formato XML gerado ou consumido pelo serializador. Se esse controle fino for necessário, a *serialização XML* precisará ser usada e você precisará criar seus tipos para oferecer suporte essa tecnologia de serialização.  
  
1. EVITE criar os tipos especificamente para a Serialização XML, a menos que você tenha um motivo muito forte para controlar o formato do XML gerado Essa tecnologia de serialização foi substituída pela serialização do contrato de dados abordada na seção anterior.  
  
     Em outras palavras, não aplique atributos do namespace <xref:System.Xml.Serialization> a novos tipos, a menos que você saiba que o tipo será usado com a Serialização XML. O exemplo a seguir mostra como **System.Xml.Serialization** pode ser usado para controlar a forma do XML gerado.  
  
     [!code-csharp[SerializationGuidelines#6](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#6)]
     [!code-vb[SerializationGuidelines#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#6)]  
  
2. CONSIDERE implementar a interface <xref:System.Xml.Serialization.IXmlSerializable> se quiser ainda mais controle sobre o formato do XML serializado do que o que é oferecido aplicando os atributos de Serialização XML. Dois métodos da interface <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> e <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> permitem controlar totalmente o fluxo XML serializado. Você também pode controlar o esquema XML gerado para o tipo aplicando o atributo <xref:System.Xml.Serialization.XmlSchemaProviderAttribute>.  
  
#### <a name="supporting-runtime-serialization"></a>Suporte à serialização em runtime  
 A *serialização em runtime* é uma tecnologia usada pelo .NET Remoting. Se você considerar que os tipos serão transportados por meio da comunicação remota .NET, será necessário verificar se eles oferecem suporte à serialização em runtime.  
  
 O suporte básico da *serialização em runtime* pode ser fornecido aplicando o atributo <xref:System.SerializableAttribute> e os cenários mais avançados envolvem a implementação de um *padrão serializável de runtime* simples (implemente –<xref:System.Runtime.Serialization.ISerializable> e forneça um construtor de serialização).  
  
1. CONSIDERE o suporte à serialização em runtime se os tipos forem usados com a comunicação remota .NET. Por exemplo, o namespace <xref:System.AddIn> usa a comunicação remota .NET e, portanto, todos os tipos trocados entre os suplementos **System.AddIn** precisam oferecer suporte à serialização em runtime.  
  
     [!code-csharp[SerializationGuidelines#7](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#7)]
     [!code-vb[SerializationGuidelines#7](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#7)]  
  
2. CONSIDERE a implementação do *padrão serializável em runtime* se quiser controle completo sobre o processo de serialização. Por exemplo, se você quiser transformar os dados à medida que eles forem serializados ou desserializados.  
  
     O padrão é muito simples. Tudo o que você precisa fazer é implementar a interface <xref:System.Runtime.Serialization.ISerializable> e fornecer um construtor especial que é usado quando o objeto é desserializado.  
  
     [!code-csharp[SerializationGuidelines#8](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#8)]
     [!code-vb[SerializationGuidelines#8](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#8)]  
  
3. PROTEJA o construtor de serialização e forneça dois parâmetros tipados e nomeados exatamente conforme mostrado no exemplo aqui.  
  
     [!code-csharp[SerializationGuidelines#9](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#9)]
     [!code-vb[SerializationGuidelines#9](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#9)]  
  
4. IMPLEMENTE os membros ISerializable explicitamente.  
  
     [!code-csharp[SerializationGuidelines#10](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#10)]
     [!code-vb[SerializationGuidelines#10](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#10)]  
  
5. Aplicar uma demanda de link para a implementação **ISerializable.GetObjectData**. Isso garantirá que o somente núcleo completamente confiável e o serializador em runtime terão acesso ao membro.  
  
     [!code-csharp[SerializationGuidelines#11](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#11)]
     [!code-vb[SerializationGuidelines#11](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#11)]  
  
## <a name="see-also"></a>Confira também

- [Usando contratos de dados](../../framework/wcf/feature-details/using-data-contracts.md)
- [Serializador de contrato de dados](../../framework/wcf/feature-details/data-contract-serializer.md)
- [Tipos com suporte fornecido pelo serializador de contrato de dados](../../framework/wcf/feature-details/types-supported-by-the-data-contract-serializer.md)
- [Serialização binária](binary-serialization.md)
- [Comunicação remota .NET](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- [Segurança e serialização](../../framework/misc/security-and-serialization.md)
