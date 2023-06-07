---
title: Etapas do processo de serialização
description: O processo de serialização começa quando o método Serialize é chamado em um formatador. Este artigo descreve a sequência de eventos.
ms.date: 08/07/2017
helpviewer_keywords:
- binary serialization, steps
- serialization, steps
ms.assetid: 4bcbc883-2a91-418f-b968-6c86a25e9737
ms.openlocfilehash: 1f749b9102182e78bc3fda436cf386a9f5759d5a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83379095"
---
# <a name="steps-in-the-serialization-process"></a>Etapas do processo de serialização
Quando o método <xref:System.Runtime.Serialization.Formatter.Serialize%2A> é chamado em um [formatador](xref:System.Runtime.Serialization.Formatter), a serialização do objeto procede de acordo com a seguinte sequência de regras:

- Uma verificação é feita para determinar se o formatador tem um seletor substituto. Se tiver, verifique se o seletor substituto administra objetos do tipo determinado. Se o seletor manipular o tipo de objeto, <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A?displayProperty=nameWithType> será chamado no seletor alternativo.

- Se não houver nenhum seletor alternativo ou se ele não manipular o tipo de objeto, uma verificação será feita para determinar se o objeto está marcado com o atributo [Serializable](xref:System.SerializableAttribute). Se o objeto não estiver marcado, uma <xref:System.Runtime.Serialization.SerializationException> será gerada.

- Se o objeto estiver marcado corretamente, verifique se ele implementa a interface <xref:System.Runtime.Serialization.ISerializable>. Se o objeto implementá-la, <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A> será chamado no objeto.
  
- Se o objeto não implementar <xref:System.Runtime.Serialization.ISerializable>, a política de serialização padrão será usada, serializando todos os campos não marcados como [NonSerialized](xref:System.NonSerializedAttribute).

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
## <a name="see-also"></a>Confira também

- [Serialização binária](binary-serialization.md)
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
