---
title: Federação e confiabilidade
ms.date: 03/30/2017
helpviewer_keywords:
- federation [WCF], and trust
ms.assetid: 4bdec4f2-f8a2-4512-bdcf-14ef54b5877a
ms.openlocfilehash: 8b924a4aeb9c667592e99d65666cd8f048d34c22
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595473"
---
# <a name="federation-and-trust"></a>Federação e confiabilidade
Este tópico aborda vários aspectos relacionados a aplicativos federados, limites de confiança e configuração e o uso de tokens emitidos no Windows Communication Foundation (WCF).  
  
## <a name="services-security-token-services-and-trust"></a>Serviços, serviços de token de segurança e confiança  
 Os serviços que expõem pontos de extremidade federados normalmente esperam que os clientes se autentiquem usando um token fornecido por um emissor específico. É importante que o serviço esteja configurado com as credenciais corretas para o emissor; caso contrário, ele não poderá verificar as assinaturas nos tokens emitidos e o cliente não poderá se comunicar com o serviço. Para obter mais informações sobre como configurar credenciais do emissor no serviço, consulte [como configurar credenciais em um serviço de Federação](how-to-configure-credentials-on-a-federation-service.md).  
  
 Da mesma forma, ao usar chaves simétricas, as chaves são criptografadas para o serviço de destino, portanto, você deve configurar o serviço de token de segurança com as credenciais corretas para o serviço de destino; caso contrário, não será possível criptografar a chave para o serviço de destino e, novamente, o cliente não poderá se comunicar com o serviço.  
  
 Os serviços WCF usam o valor da <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.MaxClockSkew%2A> propriedade no [SecurityBindingElement](../diagnostics/wmi/securitybindingelement.md) para permitir a distorção de relógio entre o cliente e o serviço. Na Federação, a `MaxClockSkew` configuração se aplica a distorções de relógio entre o cliente e o serviço de token de segurança de onde o cliente obteve o token emitido. Portanto, os serviços de token de segurança não precisam fazer concessões de distorção de relógio ao definir os tempos de efetivação e de expiração do token emitido.  
  
> [!NOTE]
> A importância da distorção do relógio aumenta conforme o tempo de vida do token emitido diminui. Na maioria dos casos, a distorção do relógio não será um problema significativo se o tempo de vida do token for de 30 minutos ou mais. Os cenários com vidas de vida mais curtas ou onde o tempo de validade exato do token são importantes devem ser projetados para levar em conta a inclinação do relógio.  
  
## <a name="federated-endpoints-and-time-outs"></a>Pontos de extremidade federados e tempos limite  
 Quando um cliente se comunica com um ponto de extremidade federado, ele deve primeiro adquirir um token apropriado de um serviço de token de segurança. Se o serviço de token de segurança expõe um ponto de extremidade federado, o cliente deve primeiro obter um token do emissor para esse ponto de extremidade. Cada aquisição de token leva tempo e esse tempo está sujeito ao tempo limite geral para enviar a mensagem real para o ponto de extremidade final.  
  
 Por exemplo, o tempo limite no canal do lado do cliente é definido como 30 segundos. Dois emissores de token precisam ser chamados para recuperar Tokens antes de enviar a mensagem para o ponto de extremidade final, e cada um leva 15 segundos para emitir um token. Nesse caso, a tentativa falhará e um <xref:System.TimeoutException> será gerado. Portanto, você precisa definir o <xref:System.ServiceModel.IContextChannel.OperationTimeout%2A> valor no canal do cliente com um valor grande o suficiente para incluir o tempo necessário para recuperar todos os tokens emitidos. No caso em que um valor não é especificado para a <xref:System.ServiceModel.IContextChannel.OperationTimeout%2A> propriedade, a <xref:System.ServiceModel.Channels.Binding.OpenTimeout%2A> propriedade ou a <xref:System.ServiceModel.Channels.Binding.SendTimeout%2A> Propriedade (ou ambos) precisa ser definida como um valor grande o suficiente para incluir o tempo necessário para recuperar todos os tokens emitidos.  
  
## <a name="token-lifetime-and-renewal"></a>Tempo de vida e renovação do token  
 Os clientes WCF não verificam o token emitido ao fazer uma solicitação inicial para um serviço.  Em vez disso, o WCF confia no serviço de token de segurança para emitir um token com tempos efetivos e de validade apropriados. Se o token for armazenado em cache pelo cliente e reutilizado, o tempo de vida do token será verificado nas solicitações subsequentes e o cliente renovará o token automaticamente, se necessário. Para obter mais informações sobre o cache de token, consulte [como: criar um cliente federado](how-to-create-a-federated-client.md).  
  
 A especificação de tempos de vida curtos, na ordem de 30 segundos ou menos para tokens emitidos ou tokens de contexto de segurança, pode resultar em tempos limite de negociação ou outras exceções sendo geradas por clientes WCF ao solicitar tokens emitidos ou ao negociar ou renovar tokens de contexto de segurança.  
  
## <a name="issued-tokens-and-inclusionmode"></a>Tokens emitidos e Inclusõesmode  
 Se um token emitido é serializado em uma mensagem enviada de um cliente para um ponto de extremidade federado ou não é controlado pela definição da <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters.InclusionMode%2A> propriedade da <xref:System.ServiceModel.Security.Tokens.SecurityTokenParameters> classe. Essa propriedade pode ser definida como um dos <xref:System.ServiceModel.Security.Tokens.SecurityTokenInclusionMode> valores de enumeração, mas não é útil na maioria dos cenários federados. Os `SecurityTokenInclusionMode.Never` `SecurityTokenInclusionMode.AlwaysToInitiator` valores e fazem com que o cliente envie uma referência para o token emitido pelo serviço de token de segurança para a terceira parte confiável. A menos que a terceira parte confiável possua uma cópia do token emitido, a autenticação falhará porque a referência de token não poderá ser resolvida. O WCF trata `SecurityTokenInclusionMode.Once` como equivalente a `SecurityTokenInclusionMode.AlwaysToRecipient` .  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Security.Tokens.SecurityTokenInclusionMode>
- [Como criar um cliente federado](how-to-create-a-federated-client.md)
- [Como configurar credenciais em um serviço de federação](how-to-configure-credentials-on-a-federation-service.md)
- [Como criar um WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)
