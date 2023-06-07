---
title: Revelação de informações
ms.date: 03/30/2017
ms.assetid: 4064c89f-afa6-444a-aa7e-807ef072131c
ms.openlocfilehash: a58ac4dd3715052031c7fb5c1da480c0d01396ea
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596858"
---
# <a name="information-disclosure"></a>Revelação de informações

A divulgação de informações permite que um invasor receba informações valiosas sobre um sistema. Portanto, sempre considere quais informações você está revelando e se elas podem ser usadas por um usuário mal-intencionado. A lista a seguir contém possíveis ataques de divulgação de informações e fornece mitigações para cada um.

## <a name="message-security-and-http"></a>Segurança de mensagem e HTTP

Se você estiver usando a segurança em nível de mensagem em uma camada de transporte HTTP, lembre-se de que a segurança em nível de mensagem não protege os cabeçalhos HTTP. A única maneira de proteger cabeçalhos HTTP é usar o transporte HTTPS em vez de HTTP. O transporte HTTPS faz com que toda a mensagem, incluindo os cabeçalhos HTTP, seja criptografada usando o protocolo protocolo SSL (SSL).

## <a name="policy-information"></a>Informações de política

Manter a política segura é importante, especialmente em cenários de Federação em que os requisitos de token emitidos confidenciais ou informações de emissor de token são expostos na política. Nesses casos, a recomendação é proteger o ponto de extremidade de política do serviço federado para impedir que os invasores obtenham informações sobre o serviço, como o tipo de declarações para colocar no token emitido ou redirecionar clientes para emissores de token mal-intencionado. Por exemplo, um invasor pode descobrir pares de nome de usuário/senha reconfigurando a cadeia de confiança federada para terminar em um emissor que executava um ataque man-in-the-Middle. Também é recomendável que os clientes federados que obtêm suas associações por meio da recuperação de política verifiquem se eles confiam nos emissores na cadeia de confiança federada obtida. Para obter mais informações sobre cenários de Federação, consulte [Federation](federation.md).

## <a name="memory-dumps-can-reveal-claim-information"></a>Os despejos de memória podem revelar informações de declaração

Quando um aplicativo falha, os arquivos de log, como os produzidos pelo Dr. Watson, podem conter as informações de declaração. Essas informações não devem ser exportadas para outras entidades, como equipes de suporte; caso contrário, as informações de declaração que contêm dados privados também são exportadas. Você pode mitigar isso ao não enviar os arquivos de log para entidades desconhecidas.

## <a name="endpoint-addresses"></a>Endereços do ponto de extremidade

Um endereço de ponto de extremidade contém as informações necessárias para se comunicar com um ponto de extremidade. A segurança SOAP deve incluir o endereço por completo nas mensagens de negociação de segurança trocadas para negociar uma chave simétrica entre um cliente e um servidor. Como a negociação de segurança é um processo de inicialização, os cabeçalhos de endereço não podem ser criptografados durante esse processo. Portanto, o endereço não deve conter dados confidenciais; caso contrário, ele leva a ataques de divulgação de informações.

## <a name="certificates-transferred-unencrypted"></a>Certificados transferidos sem criptografia

Quando você usa um certificado X. 509 para autenticar um cliente, o certificado é transferido em claro, dentro do cabeçalho SOAP. Esteja ciente disso como uma possível divulgação de PII (informações de identificação pessoal). Isso não é um problema para `TransportWithMessageCredential` o modo, em que toda a mensagem é criptografada com segurança em nível de transporte.

## <a name="service-references"></a>Referências de serviço

Uma referência de serviço é uma referência a outro serviço. Por exemplo, um serviço pode passar uma referência de serviço para um cliente no decorrer de uma operação. A referência de serviço também é usada com um *Verificador de identidade de confiança*, um componente interno que garante a identidade da entidade de destino antes de divulgar informações como dados de aplicativo ou credenciais para o destino. Se a identidade de confiança remota não puder ser verificada ou estiver incorreta, o remetente deverá garantir que nenhum dado foi divulgado que possa comprometer a si mesmo, o aplicativo ou o usuário.

As atenuações incluem o seguinte:

- As referências de serviço são consideradas confiáveis. Tome cuidado sempre que transferir as instâncias de referência de serviço para garantir que elas não tenham sido adulteradas.

- Alguns aplicativos podem apresentar uma experiência de usuário que permite o estabelecimento interativo de confiança com base nos dados na referência de serviço e na confiança dos dados comprovados pelo host remoto. O WCF fornece pontos de extensibilidade para tal recurso, mas o usuário deve implementá-los.

## <a name="ntlm"></a>NTLM

Por padrão, no ambiente de domínio do Windows, a autenticação do Windows usa o protocolo Kerberos para autenticar e autorizar usuários. Se o protocolo Kerberos não puder ser usado por algum motivo, o NTLM (NT LAN Manager) será usado como um fallback. Você pode desabilitar esse comportamento definindo a <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> propriedade como `false` . Problemas que você deve saber ao permitir o NTLM incluem:

- O NTLM expõe o nome de usuário do cliente. Se o nome de usuário precisar ser mantido confidencial, defina a `AllowNTLM` Propriedade na associação como `false` .

- O NTLM não fornece autenticação de servidor. Portanto, o cliente não pode garantir que ele esteja se comunicando com o serviço correto quando você usa NTLM como um protocolo de autenticação.

### <a name="specifying-client-credentials-or-invalid-identity-forces-ntlm-usage"></a>Especificar credenciais de cliente ou identidade inválida força o uso do NTLM

Ao criar um cliente, especificar credenciais de cliente sem um nome de domínio ou especificar uma identidade de servidor inválida, faz com que o NTLM seja usado em vez do protocolo Kerberos (se a `AllowNtlm` propriedade for definida como `true` ). Como o NTLM não faz a autenticação do servidor, as informações podem ser divulgadas potencialmente.

Por exemplo, é possível especificar credenciais de cliente do Windows sem um nome de domínio, conforme mostrado no código do Visual C# a seguir.

```csharp
MyChannelFactory.Credentials.Windows.ClientCredential = new System.Net.NetworkCredential("username", "password");
```

O código não especifica um nome de domínio e, portanto, NTLM será usado.

Se o domínio for especificado, mas um nome de entidade de serviço inválido for especificado usando o recurso de identidade do ponto de extremidade, o NTLM será usado. Para obter mais informações sobre como a identidade do ponto de extremidade é especificada, consulte [identidade e autenticação de serviço](service-identity-and-authentication.md).

## <a name="see-also"></a>Consulte também

- [Considerações sobre segurança](security-considerations-in-wcf.md)
- [Elevação de privilégio](elevation-of-privilege.md)
- [Negação de serviço](denial-of-service.md)
- [Adulteração](tampering.md)
- [Cenários sem suporte](unsupported-scenarios.md)
- [Ataques por repetição](replay-attacks.md)
