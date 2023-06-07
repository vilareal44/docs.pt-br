---
title: Tipos com suporte fornecido pelo serializador de contrato de dados
description: Consulte a lista completa de tipos que o serializador de contrato de dados WCF dá suporte para serialização e desserialização.
ms.date: 03/30/2017
helpviewer_keywords:
- serialization [WCF], supported types
ms.assetid: 7381b200-437a-4506-9556-d77bf1bc3f34
ms.openlocfilehash: ef9d2e61ab7121c97bd474bb151fee32907b1dac
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246526"
---
# <a name="types-supported-by-the-data-contract-serializer"></a>Tipos com suporte fornecido pelo serializador de contrato de dados

Windows Communication Foundation (WCF) usa o <xref:System.Runtime.Serialization.DataContractSerializer> como seu mecanismo de serialização padrão para converter dados em XML e converter XML de volta em dados. O <xref:System.Runtime.Serialization.DataContractSerializer> é projetado para serializar tipos de *contrato de dados* . No entanto, ele dá suporte a muitos outros tipos, que podem ser considerados como tendo um contrato de dados implícito. A seguir está uma lista completa de tipos que podem ser serializados:

- Todos os tipos publicamente visíveis que têm um construtor que não tem parâmetros.

- Tipos de contrato de dados. Esses são tipos para os quais o <xref:System.Runtime.Serialization.DataContractAttribute> atributo foi aplicado. Os novos tipos personalizados que representam objetos comerciais normalmente devem ser criados como tipos de contrato de dados. Para obter mais informações, consulte [usando contratos de dados](using-data-contracts.md) e [tipos serializáveis](serializable-types.md).

- Tipos de coleção. Esses são tipos que representam listas de dados. Elas podem ser matrizes regulares de tipos, ou tipos de coleção, como <xref:System.Collections.ArrayList> e <xref:System.Collections.Generic.Dictionary%602> . O <xref:System.Runtime.Serialization.CollectionDataContractAttribute> atributo pode ser usado para personalizar a serialização desses tipos, mas não é necessário. Para obter mais informações, consulte [tipos de coleção em contratos de dados](collection-types-in-data-contracts.md).

- Tipos de enumeração. Enumerações, incluindo enumerações de sinalizador, são serializáveis. Opcionalmente, os tipos de enumeração podem ser marcados com o <xref:System.Runtime.Serialization.DataContractAttribute> atributo; nesse caso, todos os membros que participam da serialização devem ser marcados com o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo. Os membros que não estão marcados não são serializados. Para obter mais informações, consulte [tipos de enumeração em contratos de dados](enumeration-types-in-data-contracts.md).

- .NET Framework tipos primitivos. Os tipos a seguir criados no .NET Framework podem ser serializados e considerados tipos primitivos:,,,,,,,,,,, <xref:System.Byte> <xref:System.SByte> <xref:System.Int16> ,, <xref:System.Int32> <xref:System.Int64> <xref:System.UInt16> <xref:System.UInt32> <xref:System.UInt64> <xref:System.Single> <xref:System.Double> <xref:System.Boolean> <xref:System.Char> <xref:System.Decimal> <xref:System.Object> e <xref:System.String> .

- Outros tipos primitivos. Esses tipos não são primitivos na .NET Framework, mas são tratados como primitivos no formulário XML serializado. Esses tipos são,,,,, <xref:System.DateTime> <xref:System.DateTimeOffset> <xref:System.TimeSpan> <xref:System.Guid> <xref:System.Uri> <xref:System.Xml.XmlQualifiedName> e matrizes do <xref:System.Byte> .

  > [!NOTE]
  > Ao contrário de outros tipos primitivos, <xref:System.DateTimeOffset> não é um tipo conhecido por padrão. Para obter mais informações, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md)).

- Tipos marcados com o <xref:System.SerializableAttribute> atributo. Muitos tipos incluídos na .NET Framework biblioteca de classes base se enquadram nessa categoria. O <xref:System.Runtime.Serialization.DataContractSerializer> dá suporte total a esse modelo de programação de serialização usado pelo .NET Framework comunicação remota, o <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> e o <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> , incluindo suporte para a <xref:System.Runtime.Serialization.ISerializable> interface.

- Tipos que representam XML ou tipos brutos que representam dados relacionais do ADO.NET. A <xref:System.Xml.XmlElement> matriz e dos <xref:System.Xml.XmlNode> tipos têm suporte como uma maneira de representar o XML diretamente. Além disso, os tipos que implementam a <xref:System.Xml.Serialization.IXmlSerializable> interface têm suporte, incluindo o <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> atributo relacionado e os <xref:System.Xml.Linq.XDocument> <xref:System.Xml.Linq.XElement> tipos e. O <xref:System.Data.DataTable> tipo ADO.net e o <xref:System.Data.DataSet> tipo (bem como suas classes derivadas tipadas) implementam a <xref:System.Xml.Serialization.IXmlSerializable> interface e, portanto, se encaixam nessa categoria. Para obter mais informações, consulte [tipos XML e ADO.net em contratos de dados](xml-and-ado-net-types-in-data-contracts.md).

## <a name="limitations-of-using-certain-types-in-partial-trust-mode"></a>Limitações do uso de determinados tipos no modo de confiança parcial

Veja a seguir uma lista de limitações ao usar determinados tipos em cenários de modo de confiança parcial:

- Para serializar ou desserializar um tipo que implementa <xref:System.Runtime.Serialization.ISerializable> em código parcialmente confiável usando o <xref:System.Runtime.Serialization.DataContractSerializer> requer as <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter%2A> <xref:System.Security.Permissions.SecurityPermissionAttribute.UnmanagedCode%2A> permissões e.

- Ao executar o código WCF no modo de [confiança parcial](partial-trust.md) , a serialização e a desserialização de `readonly` campos ( `public` e `private` ) não têm suporte. Isso ocorre porque o IL gerado não é verificável e, portanto, requer permissões elevadas.

- Tanto o <xref:System.Runtime.Serialization.DataContractSerializer> quanto o <xref:System.Xml.Serialization.XmlSerializer> têm suporte em um ambiente de confiança parcial. No entanto, o uso do <xref:System.Runtime.Serialization.DataContractSerializer> está sujeito às seguintes condições:

  - Todos os `[DataContract]` tipos serializáveis devem ser públicos.

  - Todos os `[DataMember]` campos serializáveis ou propriedades em um `[DataContract]` tipo devem ser públicos e de leitura/gravação. Não há suporte para a serialização e desserialização de `readonly` campos ao executar o WCF em um aplicativo parcialmente confiável.

  - `[Serializable]` / `ISerializable]` Não há suporte para o modelo de programação em um ambiente de confiança parcial.

  - Tipos conhecidos devem ser especificados no código ou na configuração no nível do computador ( `Machine.config` ). Tipos conhecidos não podem ser especificados na configuração em nível de aplicativo por motivos de segurança.

- Os tipos que implementam <xref:System.Runtime.Serialization.IObjectReference> geram uma exceção em um ambiente parcialmente confiável, pois o <xref:System.Runtime.Serialization.IObjectReference.GetRealObject%2A> método requer a permissão de segurança `[SecurityPermission(SecurityAction.LinkDemand, Flags=SecurityPermissionFlag.SerializationFormatter)]` .

## <a name="additional-notes-on-serialization"></a>Observações adicionais sobre serialização

As regras a seguir também se aplicam a tipos com suporte no serializador de contrato de dados:

- Os tipos genéricos são totalmente suportados pelo serializador de contrato de dados.

- Os tipos de valores anuláveis são totalmente suportados pelo serializador de contrato de dados.

- Os tipos de interface são tratados como <xref:System.Object> ou, no caso de interfaces de coleção, como tipos de coleção.

- Há suporte para estruturas e classes.

- O <xref:System.Runtime.Serialization.DataContractSerializer> não oferece suporte ao modelo de programação usado pelos <xref:System.Xml.Serialization.XmlSerializer> Serviços Web e ASP.net. Em particular, ele não oferece suporte a atributos como <xref:System.Xml.Serialization.XmlElementAttribute> e <xref:System.Xml.Serialization.XmlAttributeAttribute> . Para habilitar o suporte para esse modelo de programação, o WCF deve ser alternado para usar o <xref:System.Xml.Serialization.XmlSerializer> em vez do <xref:System.Runtime.Serialization.DataContractSerializer> .

- O <xref:System.DBNull> tipo é tratado de forma especial. É um tipo singleton e, após a desserialização, o desserializador respeita a restrição singleton e aponta todas as `DBNull` referências para a instância singleton. Como `DBNull` é um tipo serializável, ele exige <xref:System.Security.Permissions.SecurityPermissionAttribute.SerializationFormatter%2A> permissão.

## <a name="see-also"></a>Veja também

- [Tipos de XML e ADO.NET em contratos de dados](xml-and-ado-net-types-in-data-contracts.md)
- [Usando contratos de dados](using-data-contracts.md)
- [Tipos serializáveis](serializable-types.md)
- [Tipos de coleção em contratos de dados](collection-types-in-data-contracts.md)
- [Tipos de enumeração em contratos de dados](enumeration-types-in-data-contracts.md)
