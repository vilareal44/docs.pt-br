---
title: 'Mitigação: serviços WCF e autenticação de certificado'
description: Saiba como mitigar problemas de autenticação de certificado das alterações para a lista padrão do protocolo SSL do WCF no .NET Framework 4,6.
ms.date: 03/30/2017
ms.assetid: ef19c91a-b9df-4bf0-a28e-eb1e99c4bc95
ms.openlocfilehash: b6460e58bb32151003430d6573c4bcf1b514081b
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475366"
---
# <a name="mitigation-wcf-services-and-certificate-authentication"></a>Mitigação: serviços WCF e autenticação de certificado

O .NET Framework 4.6 adiciona o TLS 1.1 e o TLS 1.2 à lista padrão de protocolos SSL do WCF. Quando os computadores cliente e servidor têm o .NET Framework 4.6 ou posteriores instalados, o TLS 1.2 é usado para negociação.

## <a name="impact"></a>Impacto

O TLS 1.2 não dá suporte à autenticação do certificado MD5. Como resultado, se um cliente usar um certificado SSL que usa MD5 para o algoritmo de hash, o cliente WCF falhará ao se conectar ao serviço WCF. Para saber mais, confira [Mitigation: WCF Services and Certificate Authentication](mitigation-wcf-services-and-certificate-authentication.md) (Mitigação: serviços WCF e autenticação de certificado).

## <a name="mitigation"></a>Atenuação

Você pode contornar esse problema para que um cliente WCF possa se conectar a um servidor WCF seguindo um destes procedimentos:

- Atualize o certificado para não usar o algoritmo MD5. Esta é a solução recomendada.

- Se a associação não tiver sido configurada dinamicamente no código-fonte, atualize o arquivo de configuração de aplicativo para usar o TLS 1.1 ou uma versão anterior do protocolo. Isso permite que você continue usando um certificado com o algoritmo de hash MD5.

  > [!CAUTION]
  > Essa solução alternativa não é recomendada, pois um certificado com o algoritmo de hash MD5 é considerado inseguro.

  O arquivo de configuração que se segue demonstra isso:

  ```xml
  <configuration>
      <system.serviceModel>
          <bindings>
              <netTcpBinding>
                  <binding>
                      <security mode= "None|Transport|Message|TransportWithMessageCredential" >
                          <transport clientCredentialType="None|Windows|Certificate"
                                              protectionLevel="None|Sign|EncryptAndSign"
                                              sslProtocols="Ssl3|Tls1|Tls11">
                          </transport>
                      </security>
                  </binding>
              </netTcpBinding>
          </bindings>
      </system.serviceModel>
  </configuration>
  ```

- Se a associação foi configurada dinamicamente no código-fonte, atualize a propriedade <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A?displayProperty=nameWithType> para usar o TLS 1.1 (<xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType>) ou uma versão anterior do protocolo no código-fonte.

  > [!CAUTION]
  > Essa solução alternativa não é recomendada, pois um certificado com o algoritmo de hash MD5 é considerado inseguro.

## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
