---
title: System.ServiceModel.Channels.PeerNodeAuthenticationFailure
ms.date: 03/30/2017
ms.assetid: 0b50f782-ca06-4a82-aa7f-71f78ddc5177
ms.openlocfilehash: cb7019f1e4b47bde7c98b0109afa7b0125b7ef63
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84577299"
---
# <a name="systemservicemodelchannelspeernodeauthenticationfailure"></a>System.ServiceModel.Channels.PeerNodeAuthenticationFailure
O handshake de segurança com um vizinho potencial não foi bem-sucedido.  
  
## <a name="description"></a>Descrição  
 Esse rastreamento ocorre durante a tentativa de estabelecer uma conexão de vizinho segura. Isso pode ocorrer devido a credenciais insuficientes ou incorretas.  
  
 O PeerChannel reconhece um único tipo de token para certificados de identificação forte, X. 509, que fornecem um modelo de identidade forte baseado no tipo de autenticação e autorização que podem ser implementados. O PeerChannel também oferece suporte a aplicativos simples por meio do uso de senhas. As senhas só podem ser usadas para permitir a entrada na sessão; Eles não podem ser usados para executar a autenticação de mensagens. Isso ocorre porque um token simétrico que um grupo de colegas compartilha é difícil e inadequado para a autenticação de origem.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Verifique se todos os vizinhos têm as credenciais de segurança apropriadas.  
  
## <a name="see-also"></a>Consulte também

- [Segurança de canal par](../../feature-details/peer-channel-security.md)
- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
