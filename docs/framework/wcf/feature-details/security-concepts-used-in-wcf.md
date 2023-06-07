---
title: Conceitos de segurança utilizados no WCF
ms.date: 03/30/2017
ms.assetid: 3b9dfcf5-4bf1-4f35-9070-723171c823a1
ms.openlocfilehash: f852ba4e1100103289bc5fd879b19ebd40443b8d
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595174"
---
# <a name="security-concepts-used-in-wcf"></a>Conceitos de segurança utilizados no WCF
A segurança do Windows Communication Foundation (WCF) baseia-se nos conceitos já em uso e implantados em várias infraestruturas de segurança.  
  
 O WCF dá suporte a algumas dessas infraestruturas, como protocolo SSL (SSL) sobre HTTP (HTTPS). No entanto, o WCF vai além do suporte a infraestruturas de segurança existentes implementando padrões de segurança interoperáveis mais recentes (como o WS-Security) em mensagens codificadas em SOAP. Se você estiver usando mecanismos existentes ou novos padrões interoperáveis, os conceitos de segurança por trás são os mesmos. Entender os conceitos por trás das infraestruturas existentes e os padrões mais recentes é fundamental para implementar o melhor modelo de segurança para um aplicativo.  
  
## <a name="introduction-to-security-for-wcf-web-services"></a>Introdução à segurança para serviços Web WCF  

O grupo de padrões e práticas da Microsoft escreveu uma white paper detalhada chamada [Guia de segurança do WCF](https://archive.codeplex.com/?p=wcfsecurityguide). Este white paper descreve os conceitos fundamentais de segurança que se relacionam com os serviços da Web, os principais conceitos de segurança do WCF, cenários de aplicativos de intranet e cenários de aplicativos de Internet.  
  
## <a name="industry-wide-security-specifications"></a>Especificações de segurança de todo o setor  
  
### <a name="public-key-infrastructure"></a>Infraestrutura de chave pública  

A PKI (infraestrutura de chave pública) é um sistema de certificados digitais, autoridades de certificação e outras autoridades de registro que verificam e autenticam cada parte envolvida em uma transação eletrônica por meio do uso de criptografia de chave pública.
  
### <a name="kerberos-protocol"></a>Protocolo Kerberos  
 O *protocolo Kerberos* é uma especificação para a criação de um mecanismo de segurança que autentica os usuários em um domínio do Windows. Ele permite que um usuário estabeleça um contexto seguro com outras entidades em um domínio. O Windows 2000 e plataformas posteriores usam o protocolo Kerberos por padrão. Entender os mecanismos do sistema é útil ao criar um serviço que irá interagir com clientes de intranet. Além disso, como a *Associação Kerberos especificação Web Services Security* é amplamente publicada, você pode usar o protocolo Kerberos para se comunicar com clientes da Internet (ou seja, o protocolo Kerberos é interoperável). Para obter mais informações sobre como o protocolo Kerberos é implementado no Windows, consulte [Microsoft Kerberos](/windows/win32/secauthn/microsoft-kerberos).  
  
### <a name="x509-certificates"></a>Certificados X. 509  
 Os certificados X. 509 são um formulário de credencial principal usado em aplicativos de segurança. Para obter mais informações sobre certificados X. 509, consulte [certificados de chave pública x. 509](/windows/win32/seccertenroll/about-x-509-public-key-certificates). Os certificados X. 509 são armazenados em um repositório de certificados. Um computador que executa o Windows tem vários tipos de repositórios de certificados, cada um com uma finalidade diferente. Para obter mais informações sobre os diferentes repositórios, consulte [repositórios de certificados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc757138(v=ws.10)).  
  
## <a name="web-services-security-specifications"></a>Especificações de especificação Web Services Security  
 As associações definidas pelo sistema dão suporte a muitas especificações de segurança de serviços da Web mais usadas. Para obter uma lista completa de associações fornecidas pelo sistema e as especificações de serviços Web que eles dão suporte, consulte: [protocolos de serviços Web com suporte de associações de interoperabilidade fornecidas pelo sistema](web-services-protocols-supported-by-system-provided-interoperability-bindings.md)  
  
## <a name="access-control-mechanisms"></a>Mecanismos de controle de acesso  
 O WCF fornece várias maneiras de controlar o acesso a um serviço ou uma operação. Entre eles estão  
  
1. <xref:System.Security.Permissions.PrincipalPermissionAttribute>  
  
2. Provedor de associação do ASP.NET  
  
3. Provedor de função ASP.NET  
  
4. Gerenciador de Autorização  
  
5. Modelo de identidade  
  
 Para obter mais informações sobre esses tópicos, consulte [mecanismos de controle de acesso](access-control-mechanisms.md)  
  
## <a name="see-also"></a>Confira também

- [Visão geral de segurança](security-overview.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
