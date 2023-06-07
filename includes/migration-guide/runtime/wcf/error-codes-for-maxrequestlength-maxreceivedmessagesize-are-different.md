---
ms.openlocfilehash: 3aafb14b65f7c0f9e5d77927809547f9d4b96e1c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619808"
---
### <a name="error-codes-for-maxrequestlength-or-maxreceivedmessagesize-are-different"></a>Códigos de erro de maxRequestLength ou maxReceivedMessageSize são diferentes

#### <a name="details"></a>Detalhes

Mensagens nos serviços Web WCF hospedadas no IIS (Serviços de Informações da Internet) ou no ASP.NET Development Server que excedam maxRequestLength (no ASP.NET) ou maxReceivedMessageSize (no WCF) têm códigos de erro diferentes. O código de status HTTP foi alterado de 400 (Solicitação Incorreta) para 413 (Entidade de Solicitação Muito Longa), e as mensagens que excederam as configurações de maxRequestLength ou maxReceivedMessageSize lançarão uma exceção <xref:System.ServiceModel.ProtocolException?displayProperty=fullName>. Isso inclui os casos em que o modo de transferência é Streamed.

#### <a name="suggestion"></a>Sugestão

Essa alteração facilita a depuração nos casos em que o comprimento da mensagem excede os limites permitidos pelo ASP.NET ou WCF. É necessário modificar os códigos que executam o processamento baseado em um código de status HTTP 400.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
