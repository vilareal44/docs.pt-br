---
title: Segurança de canal par
ms.date: 03/30/2017
ms.assetid: 2c59b164-3729-44f0-a967-f247c42de662
ms.openlocfilehash: f8015f41254d17f908f3665db65af3d82eaa2b6a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576077"
---
# <a name="peer-channel-security"></a>Segurança de canal par
O Peer Channel permite uma variedade de tipos de aplicativos distribuídos que dependem de mensagens de vários partes. Alguns exemplos incluem distribuição de conteúdo de escala da Internet, em que uma fonte confiável distribui conteúdo (como atualizações de mídia ou software), um grupo de amigos que troca músicas e fotos, ou uma equipe de colegas que edita um documento. Cada um desses cenários requer um modelo de segurança exclusivo. O modelo de segurança de canal par é projetado para lidar com esses cenários e fornece um modelo de segurança de som para as respectivas necessidades de identidade, autenticação e modelos de autorização diferentes.  
  
## <a name="security-scenarios"></a>Cenários de segurança  
 Um cenário de distribuição de conteúdo requer que cada destinatário de conteúdo identifique a fonte de conteúdo. Devido à natureza distribuída do cenário, nem sempre é possível conhecer e confiar nos intermediários que processam ou interceptam mensagens. Para reduzir efetivamente a ameaça de que um intermediário não confiável pode violar as mensagens, os aplicativos podem proteger a mensagem no remetente para que as tentativas de violação sejam detectadas facilmente. Nesse caso, dependendo da confidencialidade do conteúdo, a criptografia pode ser necessária.  
  
 Cenários de colaboração como a colaboração de documentos de grupo geralmente exigem que cada membro que participa da sessão seja identificado e autenticado individualmente. Isso significa que um mecanismo para definir grupos de usuários e autenticar nesses grupos é necessário para ter uma sessão segura. Além disso, os aplicativos podem exigir o rastreamento de cada mensagem por meio da autenticação no nível da mensagem. Nesses tipos de aplicativos, o desempenho pode ser sacrificado por um esquema de segurança mais forte.  
  
 Uma sessão de comunicação entre um grupo de usuários casuais pode exigir modelos de segurança informais, como o conhecimento de um segredo comum entre o grupo. Para esses tipos de aplicativos, ter um modelo de segurança que seja conveniente para estabelecer e configurar é mais importante do que ter a forma mais forte de autenticação ou fornecer medidas de não-repúdio. Para esses cenários, um mecanismo de autenticação baseado em senha ajuda a proteger a camada de comunicação e ainda permitir a autenticação de mensagens. A segurança baseada em senha é a configuração padrão para o canal par.  
  
## <a name="token-types"></a>Tipos de token  
 O canal par reconhece um único tipo de token para certificados de identificação forte, X. 509, que fornecem um modelo de identidade forte baseado no tipo de autenticação e autorização que podem ser implementados. A confidencialidade e a integridade são facilmente fornecidas usando certificados. No entanto, os certificados X. 509 podem ser difíceis de usar e implantar.  
  
 O canal par também fornece suporte para aplicativos simples por meio do uso de senhas. Os aplicativos podem optar por configurar grupos de pares rápidos e simples com base em uma senha fornecida. Nesse caso, um proprietário de grupo decide e comunica a senha aos membros. Cada membro deve entrar usando essa senha antes que possa ingressar na sessão. As senhas só podem ser usadas para permitir a entrada na sessão; Eles não podem ser usados para executar a autenticação de mensagens. Isso ocorre porque um token simétrico que um grupo de colegas compartilha é difícil e inadequado para a autenticação de origem.  
  
## <a name="security-model"></a>Modelo de segurança  
 O canal par fornece a capacidade de proteger os links individuais entre os pares. Isso significa que uma mensagem nunca flui em um link não seguro (da perspectiva do aplicativo). Internamente, cada link (um canal de transporte entre dois pares) é protegido usando TLS (Transport Layer Security). Isso significa que, quando um remetente compõe e envia uma mensagem, ele é enviado pelo transporte seguro para cada um de seus pares imediatos, quem acessa a mensagem e, por sua vez, envia a mensagem para seus pares imediatos em conexões seguras. Essa segurança só funciona no nível de transporte e é independente dos modelos de segurança de mensagem.  
  
 O canal par também fornece uma maneira de proteger mensagens independentemente da segurança de transporte usada. Nesse modelo, a mensagem é protegida na origem usando o token de segurança da origem, embora atualmente haja suporte apenas para certificados X. 509. A mensagem protegida é então transmitida pela rede de mesmo nível. Cada ponto de recebimento pode verificar a autenticidade da origem. Observe que a mensagem é protegida para que os intermediários não possam adulterar isso.  
  
 Para obter confidencialidade, os aplicativos podem empregar segurança de transporte com esquemas de associação de grupo fortes para impedir o acesso não autorizado à mensagem.  
  
 O canal par não requer um modelo de identidade específico, desde que o aplicativo escolha um dos tipos de token com suporte. Os aplicativos são completamente proprietários do ciclo de vida dessas identidades e decisões de autenticação.  
  
## <a name="see-also"></a>Consulte também

- [Protegendo aplicativos de canal par](securing-peer-channel-applications.md)
- [Conceitos de canal par](peer-channel-concepts.md)
- [Compilando um aplicativo de canal par](building-a-peer-channel-application.md)
