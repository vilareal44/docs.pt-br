---
title: Protocolo PNRP
description: Saiba mais sobre o PNRP (Peer Name Resolution Protocol), um protocolo de registro de nome seguro, escalonável e dinâmico e resolução de nomes.
ms.date: 03/30/2017
ms.assetid: 11940511-c124-4d91-ae31-d4ed6e81ee58
ms.openlocfilehash: 72eb63c2c90f398c515d77cd2b2d693237e533a5
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502217"
---
# <a name="peer-name-resolution-protocol"></a>Protocolo PNRP
Em ambientes de ponto a ponto, pares usam sistemas de resolução de nome específicos para resolver os locais de rede (endereços, protocolos e portas) uns dos outros, com base em nomes ou outros tipos de identificadores. No passado, a resolução de nome de par foi complicada devido à conectividade inerentemente transitória, bem como outras falhas dentro do sistema DNS (Sistema de Nomes de Domínio).  
  
 A plataforma de rede ponto a ponto do Microsoft® Windows® resolve esse problema com o protocolo PNRP, um protocolo de registro de nomes e de resolução de nomes seguro, escalonável e dinâmico desenvolvido primeiro para o Windows XP e depois atualizado para o Windows Vista™. O PNRP funciona de forma muito diferente dos sistemas de resolução de nome tradicionais, abrindo incríveis novas possibilidades para desenvolvedores de aplicativos.  
  
 Com o PNRP, nomes de par podem ser aplicados ao computador ou a serviços ou aplicativos individuais no computador. Uma resolução de nome de par inclui um endereço, porta e possivelmente uma carga estendida. Os benefícios do sistema incluem a tolerância a falhas, nenhum gargalo e também resoluções de nome que nunca retornarão endereços obsoletos, o que torna o protocolo de uma solução excelente para localizar usuários móveis.  
  
 Em termos de segurança, os nomes de par podem ser publicados como seguros (protegidos) ou não seguros (desprotegidos). O PNRP usa criptografia de chave pública para proteger os nomes de par segura contra falsificação; tanto computadores quanto serviços podem ser nomeados com PNRP.  
  
O protocolo PNRP demonstra as seguintes propriedades:  
  
- Distribuído e quase que totalmente sem servidor. Servidores só são necessários para o processo de inicialização.  
  
- Publicação de nome segura sem o envolvimento de terceiros. Ao contrário da publicação de nome DNS, a publicação de nome PNRP é instantânea e sem custos financeiros.  
  
- O PNRP é atualizado em tempo real, o que impede a resolução de endereços obsoletos.  
  
- A resolução de nomes por meio de PNRP vai além de computadores, permitindo também a resolução de nomes de serviços.  
  
## <a name="the-systemnetpeertopeer-namespace"></a>O namespace System.Net.PeerToPeer  
  
- A funcionalidade do protocolo PNRP é definida pelo namespace <xref:System.Net.PeerToPeer> dentro do .NET Framework versão 3.5. Ele fornece um conjunto de tipos que podem ser usados para registrar e resolver os nomes de ponto a ponto com um serviço PNRP disponível.  
  
- (PNRP e resolvedores de par personalizados podem ser criados e instanciados usando os tipos fornecidos no namespace <xref:System.ServiceModel.PeerResolvers>.)  
  
- Os tipos básicos usados para registrar e resolver os nomes com um serviço PNRP disponível são os seguintes:  
  
- <xref:System.Net.PeerToPeer.Cloud>: define as informações que descrevem uma nuvem PNRP disponíveis, incluindo o escopo dela.  
  
- <xref:System.Net.PeerToPeer.PeerName>: define um nome de par que pode ser usado para registrar e subsequentemente resolver um par em uma nuvem.  
  
- <xref:System.Net.PeerToPeer.PeerNameRecord>: Define o registro na nuvem PNRP que contém as informações de registro para um par, o que inclui os pontos de extremidade de rede em que o par pode ser contatado.  
  
- <xref:System.Net.PeerToPeer.PeerNameRegistration>: define o processo de registro para um nome de par, incluindo métodos para iniciar e parar o registro de nome de par.  
  
- <xref:System.Net.PeerToPeer.PeerNameResolver>: Define o processo de resolução de um nome de par para o respectivo ponto de extremidade da rede, incluindo métodos síncronos e assíncronos para a resolução.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.PeerResolvers>
- <xref:System.Net.PeerToPeer>
- [Amostras de programação de rede](network-programming-samples.md)

<!-- to-do: review sample links
- [PeerToPeer Technology Sample](https://go.microsoft.com/fwlink/?LinkID=179571)
-->
