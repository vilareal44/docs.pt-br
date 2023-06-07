---
title: Cenários de canal par
ms.date: 03/30/2017
ms.assetid: dae6e0f7-900c-45ee-8be9-3647698382fb
ms.openlocfilehash: c4c24a0bafa4f73760d02de6242a9ed438d7d60e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596871"
---
# <a name="peer-channel-scenarios"></a>Cenários de canal par
As APIs de canal par dão suporte aos seguintes cenários de desenvolvimento.  
  
## <a name="publicationsubscription-messaging"></a>Mensagens de publicação/assinatura  
 As empresas que criam aplicativos de publicação/assinatura (por exemplo, cotações de ações e editores de manchetes de notícias, pontuações esportivas e relatórios meteorológicos) podem usar o canal de pares para aplicativos sem servidor. Por exemplo, os usuários podem obter as últimas pontuações de esportes unindo uma malha comum (ou grupo de clientes) e propagar a grande quantidade de dados de jogos atualizados sem aumentar a carga do servidor. Isso ajuda o provedor de dados a fornecer maior qualidade de serviço sem aumentar substancialmente o investimento em tecnologias baseadas em servidor.  
  
## <a name="collaboration"></a>Colaboração  
 ISVs (fornecedores independentes de software) podem criar aplicativos que permitem que as pessoas criem grupos rígidos para participação em atividades ponto a ponto. Por exemplo, isso pode incluir equipes que trabalham em projetos colaborativos, compartilhamento de imagens entre amigos, atividades de planejamento de terceiros e muito mais. Tradicionalmente, essas atividades sempre envolvem servidores; no entanto, o canal de mesmo nível fornece uma maneira de fazer isso de maneira mais econômica habilitando cenários de acesso offline que não são tão facilmente implementados em um modelo de cliente de servidor tradicional.  
  
## <a name="distributed-processing-and-compute-clusters"></a>Clusters de computação e processamento distribuído  
 Os clusters de computação e o processamento distribuído são normalmente usados para cálculos de grande escala, como modelagem financeira/clima e decodificação de DNA humanos. Normalmente, isso é feito fazendo com que os servidores atribuam tarefas individualmente a todos os clientes que participam do cluster de computação. Esses servidores também podem ter demandas adicionais; por exemplo, todas as tarefas podem precisar ser concluídas dentro de uma determinada duração, exigindo mais de um computador para cada tarefa. Além disso, se qualquer cliente que executa uma tarefa falhar, outro cliente deverá ser capaz de assumir essa tarefa e executar o trabalho nela. Da mesma forma, mais de um cliente pode precisar executar a mesma tarefa para garantir resultados consistentes. Embora os servidores possam executar esse tipo de coordenação de clientes, você pode criar uma solução ponto a ponto em que os clientes que recebem uma tarefa de forma independente determinam os requisitos do servidor em relação à tarefa e usam uma malha de computação para determinar como concluir essa tarefa.  
  
## <a name="gaming"></a>Jogos  
 Usando o Peer Channel, os desenvolvedores de aplicativos podem criar versões sem servidor de seus jogos em que as movimentações de jogos são transmitidas e sincronizadas com outros jogadores por um mecanismo ponto a ponto, e não por meio de um servidor central. Para ISVs pequenos, isso ajuda a remover os custos operacionais associados à implantação, manutenção e manutenção de servidores centrais. Os jogos escritos usando uma arquitetura ponto a ponto podem ser reproduzidos pela Internet ou em redes locais com ou sem fio. As atividades de jogos secundárias, como o lobby e o bate-papo no jogo, podem ser desenvolvidas usando uma rede ponto a ponto.  
  
## <a name="see-also"></a>Consulte também

- [Conceitos de canal par](peer-channel-concepts.md)
