---
title: Cenários comuns de segurança
ms.date: 03/30/2017
helpviewer_keywords:
- security [WCF], scenarios
ms.assetid: 201923b5-5162-4a8a-8d4c-e7bd242748d5
ms.openlocfilehash: f36ebdb5ea248ec8134c688f89eb5d0be38dfe38
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579730"
---
# <a name="common-security-scenarios"></a>Cenários comuns de segurança
Os tópicos nesta seção catalogam várias configurações de segurança de cliente e serviço possíveis. As configurações variam de acordo com uma série de fatores. Por exemplo, se um serviço ou cliente está em uma intranet ou se a segurança é fornecida pelo Windows ou pelo transporte (como o HTTPS).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Serviço e cliente de internet desprotegido](internet-unsecured-client-and-service.md)  
 Um exemplo de um cliente e serviço públicos e desprotegidos.  
  
 [Cliente e serviço sem segurança na Intranet](intranet-unsecured-client-and-service.md)  
 Um serviço de Windows Communication Foundation básico (WCF) desenvolvido para fornecer informações sobre uma rede privada segura para um aplicativo WCF.  
  
 [Segurança de transporte com autenticação básica](transport-security-with-basic-authentication.md)  
 O aplicativo permite que os clientes façam logon usando a autenticação personalizada.  
  
 [Segurança de transporte com autenticação do Windows](transport-security-with-windows-authentication.md)  
 Mostra um cliente e um serviço protegidos pela segurança do Windows.  
  
 [Segurança do transporte com um cliente anônimo](transport-security-with-an-anonymous-client.md)  
 Esse cenário usa a segurança de transporte (como HTTPS) para garantir a confidencialidade e a integridade.  
  
 [Segurança de transporte com autenticação de certificado](transport-security-with-certificate-authentication.md)  
 Mostra um cliente e um serviço protegidos por um certificado.  
  
 [Mensagem de segurança com um cliente anônimo](message-security-with-an-anonymous-client.md)  
 Mostra um cliente e um serviço protegidos pela segurança de mensagem do WCF.  
  
 [Segurança de mensagem com um nome de usuário cliente](message-security-with-a-user-name-client.md)  
 O cliente é um aplicativo Windows Forms que permite que os clientes façam logon usando um nome de usuário e senha de domínio.  
  
 [Segurança de mensagem com um certificado de cliente](message-security-with-a-certificate-client.md)  
 Os servidores têm certificados e cada cliente tem um certificado. Um contexto de segurança é estabelecido por meio da negociação TLS.  
  
 [Segurança de mensagem com um cliente do Windows](message-security-with-a-windows-client.md)  
 Uma variação do cliente de certificado. Os servidores têm certificados e cada cliente tem um certificado. Um contexto de segurança é estabelecido por meio da negociação TLS.  
  
 [Segurança de mensagem com um cliente Windows sem negociação de credencial](message-security-with-a-windows-client-without-credential-negotiation.md)  
 Mostra um cliente e um serviço protegidos por um domínio Kerberos.  
  
 [Segurança de mensagem com certificados mútuos](message-security-with-mutual-certificates.md)  
 Os servidores têm certificados e cada cliente tem um certificado. O certificado do servidor é distribuído com o aplicativo e está disponível fora de banda.  
  
 [Segurança de mensagem com tokens emitidos](message-security-with-issued-tokens.md)  
 Segurança federada que permite o estabelecimento de confiança entre domínios independentes.  
  
 [Subsistema de confiança](trusted-subsystem.md)  
 Um cliente acessa um ou mais serviços Web que são distribuídos em uma rede. Os serviços Web acessam recursos adicionais (como bancos de dados ou outros serviços Web) que devem ser protegidos.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Autorização](authorization-in-wcf.md)  
  
 [Visão geral de segurança](security-overview.md)  
  
 [Segurança](security.md)  
  
 [Associações e segurança](bindings-and-security.md)  
  
 [Protegendo serviços e clientes](securing-services-and-clients.md)  
  
 [Autenticação](authentication-in-wcf.md)  
  
 [Autorização](authorization-in-wcf.md)  
  
 [Federação e tokens emitidos](federation-and-issued-tokens.md)  
  
 [Auditoria](auditing-security-events.md)  
  
## <a name="see-also"></a>Consulte também

- [Diretrizes e práticas recomendadas de segurança](security-guidance-and-best-practices.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
