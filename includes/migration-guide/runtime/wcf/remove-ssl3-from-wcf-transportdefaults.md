---
ms.openlocfilehash: 9b734fe960165b6d4b97b861cb3e8f31979f25c5
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621017"
---
### <a name="remove-ssl3-from-the-wcf-transportdefaults"></a>Remoção de Ssl3 de TransportDefaults do WCF

#### <a name="details"></a>Detalhes

Ao usar NetTcp com segurança de transporte e um tipo de credencial de certificado, o SSL 3 não é mais um protocolo padrão usado para negociar uma conexão segura. Na maioria dos casos, não deve haver impacto nos aplicativos existentes, pois o TLS 1.0 sempre esteve incluído na lista de protocolos para NetTcp. Todos os clientes existentes devem ser capazes de negociar uma conexão usando, no mínimo, o TLS 1.0.

#### <a name="suggestion"></a>Sugestão

Se Ssl3 for exigido, use um dos seguintes mecanismos de configuração para adicioná-lo à lista de protocolos negociados.<ul><li><xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols></li><li><xref:System.ServiceModel.TcpTransportSecurity.SslProtocols></li><li>[<](~/docs/framework/configure-apps/file-schema/wcf/transport-of-nettcpbinding.md)</li><li>[&lt;sslStreamSecurity&gt; section of &lt;customBinding&gt;]~/docs/framework/configure-apps/file-schema/wcf/sslstreamsecurity.md)</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols?displayProperty=nameWithType></li><li><xref:System.ServiceModel.TcpTransportSecurity.SslProtocols?displayProperty=nameWithType></li></ul>|
