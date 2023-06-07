---
ms.openlocfilehash: fb9436ec9e525afb497033775e34b6b636ced22d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621040"
---
### <a name="wcf-services-that-use-nettcp-with-ssl-security-and-md5-certificate-authentication"></a>Serviços WCF que usam NETTCP com a segurança SSL e autenticação de certificado MD5

#### <a name="details"></a>Detalhes

O .NET Framework 4.6 adiciona o TLS 1.1 e o TLS 1.2 à lista padrão de protocolos SSL do WCF. Quando os computadores cliente e servidor têm o .NET Framework 4.6 ou versões posteriores instaladas, o TLS 1.2 é usado para negociação. O TLS 1.2 não é compatível com a autenticação do certificado MD5. Consequentemente, se um cliente usar um certificado MD5, o cliente WCF falhará ao se conectar ao serviço WCF.

#### <a name="suggestion"></a>Sugestão

Você pode contornar esse problema para que um cliente WCF possa se conectar a um servidor WCF seguindo um destes procedimentos:

- Atualize o certificado para não usar o algoritmo MD5. Esta é a solução recomendada.
- Se a associação não tiver sido configurada dinamicamente no código-fonte, atualize o arquivo de configuração de aplicativo para usar o TLS 1.1 ou uma versão anterior do protocolo. Isso permite que você continue usando um certificado com o algoritmo de hash MD5.

> [!WARNING]
> Essa solução alternativa não é recomendada, pois um certificado com o algoritmo de hash MD5 é considerado inseguro.

O arquivo de configuração que se segue demonstra isso:

```xml
<configuration>
  <system.serviceModel>
    <bindings>
      <netTcpBinding>
        <binding>
          <security mode= "None/Transport/Message/TransportWithMessageCredential" >
            <transport clientCredentialType="None/Windows/Certificate"
                       protectionLevel="None/Sign/EncryptAndSign"
                       sslProtocols="Ssl3/Tls1/Tls11">
            </transport>
          </security>
        </binding>
      </netTcpBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```

- Se a associação foi configurada dinamicamente no código-fonte, atualize a propriedade <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols?displayProperty=nameWithType> para usar o TLS 1.1 (<xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType>) ou uma versão anterior do protocolo no código-fonte.

> [!WARNING]
> Essa solução alternativa não é recomendada, pois um certificado com o algoritmo de hash MD5 é considerado inseguro.

| Name    | Valor   |
|:--------|:--------|
| Escopo   | Secundária   |
| Versão | 4.6     |
| Type    | Runtime |
