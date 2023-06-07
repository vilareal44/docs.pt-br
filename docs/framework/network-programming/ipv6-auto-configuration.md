---
title: Configuração automática de endereço IPv6
description: Saiba como o IPv6 dá suporte ao node Plug and Play, em que um nó ingressa em uma rede IPv6 e está configurado sem intervenção humana.
ms.date: 03/30/2017
ms.assetid: 581c1d21-1013-43a3-bf3e-2d9ead62b79c
ms.openlocfilehash: 9d65bc453478ac4679556e931b1758c18cfedcf3
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502321"
---
# <a name="ipv6-auto-configuration"></a>Configuração automática de endereço IPv6
Uma meta importante para IPv6 é dar suporte a Plug and Play de nó. Ou seja, deve ser possível conectar um nó a uma rede IPv6 de modo que ele seja configurado automaticamente sem intervenção humana.  
  
## <a name="type-of-auto-configuration"></a>Tipo de configuração automática  
 O IPv6 dá suporte aos seguintes tipos de configuração automática:  
  
- **Configuração automática com estado**. Esse tipo de configuração requer um certo nível de intervenção humana, porque é necessário um protocolo DHCP para servidor IPv6 (DHCPv6) para a instalação e administração dos nós. O servidor DHCPv6 mantém uma lista de nós para os quais ele fornece informações de configuração. Ele também mantém informações de estado para que o servidor saiba quanto tempo cada endereço está em uso e quando ele pode estar disponível para reatribuição.  
  
- **Configuração automática sem monitoração de estado**. Esse tipo de configuração é adequado para indivíduos e pequenas empresas. Nesse caso, cada host determina seus endereços com base no conteúdo de anúncios de roteador recebidos. Usando o padrão IEEE EUI-64 para definir a parte de ID de rede do endereço, é razoável pressupor a exclusividade do endereço de host no link.  
  
 Independentemente de como o endereço é determinado, o nó deve verificar se seu endereço potencial é exclusivo para o link local. Isso é feito enviando uma mensagem de solicitação de vizinho para o endereço potencial. Se o nó receber alguma resposta, ele saberá que o endereço já está em uso e deverá determinar outro endereço.  
  
## <a name="ipv6-mobility"></a>Mobilidade IPv6  
 A proliferação de dispositivos móveis introduziu um novo requisito: um dispositivo deve ser capaz de alterar os locais na Internet IPv6 arbitrariamente e ainda manter as conexões existentes. Para fornecer essa funcionalidade, um endereço residencial é atribuído a um nó móvel, endereço no qual ele sempre pode ser alcançado. Quando o nó móvel está no endereço residencial, ele se conecta ao link residencial e usa seu endereço residencial. Quando o nó móvel está longe do endereço residencial, um agente, que geralmente é um roteador, retransmite mensagens entre o nó móvel e os nós com os quais ele está se comunicando.  
  
## <a name="see-also"></a>Confira também

- [Protocolo IP versão 6](internet-protocol-version-6.md)
- [Soquetes](sockets.md)
