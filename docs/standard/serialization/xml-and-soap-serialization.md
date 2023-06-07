---
title: Serialização XML e SOAP
description: Esta visão geral discute a serialização XML, que pode ser usada para serializar objetos em fluxos XML que estão em conformidade com a especificação SOAP.
ms.date: 03/30/2017
helpviewer_keywords:
- SOAP, XML serialization
- XML serialization, SOAP
- serialization, SOAP
- serialization, about serialization
- XML serialization
- serialization
ms.assetid: 832ac524-21bc-419a-a27b-ca8bfc45840f
ms.openlocfilehash: 6b7d6f59694a28207758fa7772781eed073917e4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83379538"
---
# <a name="xml-and-soap-serialization"></a>serialização XML e SOAP

A serialização de XML converte (serializa) os campos públicos e as propriedades de um objeto e os parâmetros e valores de retorno de métodos, em um fluxo XML que está de acordo com um documento XSD (linguagem de definição de esquema XML) específico. A serialização XML resulta em classes fortemente tipadas com propriedades e campos públicos que são convertidos em um formato serial (neste caso, em XML) para armazenamento ou transporte.

Como XML é um padrão aberto, o fluxo XML pode ser processado por qualquer aplicativo, quando necessário, independentemente da plataforma. Por exemplo, serviços Web XML criados com ASP.NET usam a classe <xref:System.Xml.Serialization.XmlSerializer> para criar fluxos XML que passam dados entre aplicativos de serviço Web XML por toda a Internet ou entre intranets. Por outro lado, a desserialização obtém esse fluxo XML e reconstrói o objeto.

A serialização XML também pode ser usada para serializar objetos em fluxos XML que atendam à especificação SOAP. SOAP é um protocolo baseado em XML, projetado especificamente para transportar chamadas de procedimentos usando XML.

Para serializar e desserializar objetos, use a classe <xref:System.Xml.Serialization.XmlSerializer>. Para criar as classes a serem serializadas, use a ferramenta de definição de esquema XML.

## <a name="see-also"></a>Confira também

- [Serialização binária](binary-serialization.md)
- [Serviços Web XML criados usando ASP.NET e clientes de serviço Web XML](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/7bkzywba(v=vs.100))
