---
title: Tipos serializáveis
ms.date: 03/30/2017
ms.assetid: f1c8539a-6a79-4413-b294-896f0957b2cd
ms.openlocfilehash: e65fcb93c5c36bb289b825cef58b3adc6f5155f5
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586098"
---
# <a name="serializable-types"></a>Tipos serializáveis
Por padrão, o <xref:System.Runtime.Serialization.DataContractSerializer> serializa todos os tipos publicamente visíveis. Todas as propriedades públicas de leitura/gravação e os campos do tipo são serializados.  
  
 Você pode alterar o comportamento padrão aplicando os <xref:System.Runtime.Serialization.DataContractAttribute> <xref:System.Runtime.Serialization.DataMemberAttribute> atributos e aos tipos e membros. esse recurso pode ser útil em situações nas quais você tem tipos que não estão sob seu controle e não podem ser modificados para adicionar atributos. O <xref:System.Runtime.Serialization.DataContractSerializer> reconhece esses tipos "desmarcados".  
  
## <a name="serialization-defaults"></a>Padrões de serialização  
 Você pode aplicar os <xref:System.Runtime.Serialization.DataContractAttribute> <xref:System.Runtime.Serialization.DataMemberAttribute> atributos e para controlar explicitamente ou personalizar a serialização de tipos e membros. Além disso, você pode aplicar esses atributos a campos particulares. No entanto, mesmo os tipos que não estão marcados com esses atributos são serializados e desserializados. As seguintes regras e exceções se aplicam:  
  
- O <xref:System.Runtime.Serialization.DataContractSerializer> infere um contrato de dados de tipos sem atributos usando as propriedades padrão dos tipos recém-criados.  
  
- Todos os campos públicos e propriedades com Public `get` e `set` métodos são serializados, a menos que você aplique o <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute> atributo a esse membro.  
  
- As semânticas de serialização são semelhantes às do <xref:System.Xml.Serialization.XmlSerializer> .  
  
- Em tipos desmarcados, somente tipos públicos com construtores que não têm parâmetros são serializados. A exceção a essa regra é <xref:System.Runtime.Serialization.ExtensionDataObject> usada com a <xref:System.Runtime.Serialization.IExtensibleDataObject> interface.  
  
- Campos somente leitura, propriedades sem um `get` método ou `set` , e propriedades com os métodos interno ou privado `set` ou `get` não são serializados. Essas propriedades são ignoradas e nenhuma exceção é lançada, exceto no caso de coleções somente obtenção.  
  
- <xref:System.Xml.Serialization.XmlSerializer>os atributos (como `XmlElement` ,,, `XmlAttribute` `XmlIgnore` `XmlInclude` e assim por diante) são ignorados.  
  
- Se você não aplicar o <xref:System.Runtime.Serialization.DataContractAttribute> atributo a um determinado tipo, o serializador ignorará qualquer membro desse tipo ao qual o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo é aplicado.  
  
- A <xref:System.Runtime.Serialization.DataContractSerializer.KnownTypes%2A> propriedade tem suporte em tipos não marcados com o <xref:System.Runtime.Serialization.DataContractAttribute> atributo. Isso inclui suporte para o <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo em tipos desmarcados.  
  
- Para "recusar" o processo de serialização para membros públicos, propriedades ou campos, aplique o <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute> atributo a esse membro.  
  
## <a name="inheritance"></a>Herança  
 Os tipos não marcados (tipos sem o <xref:System.Runtime.Serialization.DataContractAttribute> atributo) podem herdar de tipos que têm esse atributo; no entanto, o inverso não é permitido: tipos com o atributo não podem herdar de tipos desmarcados. Essa regra é imposta principalmente para garantir a compatibilidade com o código escrito em versões anteriores do .NET Framework.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Xml.Serialization.XmlSerializer>
- [Tipos com suporte fornecido pelo serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md)
