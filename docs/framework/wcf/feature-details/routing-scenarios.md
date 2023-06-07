---
title: Cenários de roteamento
ms.date: 03/30/2017
helpviewer_keywords:
- routing [WCF], scenarios
ms.assetid: ec22f308-665a-413e-9f94-7267cb665dab
ms.openlocfilehash: 455a6e42aea064d48846994b4e729b90667bc8e1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590495"
---
# <a name="routing-scenarios"></a>Cenários de roteamento
Embora o serviço de roteamento seja altamente personalizável, pode ser um desafio criar uma lógica de roteamento eficiente ao criar uma nova configuração do zero.  No entanto, há vários cenários comuns que a maioria das configurações de serviço de roteamento seguem. Embora esses cenários possam não se aplicar diretamente à sua configuração específica, entender como o serviço de roteamento pode ser configurado para lidar com esses cenários ajudará você a compreender o serviço de roteamento.  
  
## <a name="common-scenarios"></a>Cenários comuns  
 O uso mais básico do serviço de roteamento é para agregar vários pontos de extremidade de destino para reduzir o número de pontos de extremidade expostos aos aplicativos cliente e, em seguida, usar filtros de mensagem para rotear cada mensagem para o destino correto. As mensagens podem ser roteadas com base em requisitos de processamento lógico ou físico, como um tipo de mensagem que deve ser processado por um serviço específico ou com base em necessidades de negócios arbitrárias, como fornecer o processamento prioritário de mensagens de uma fonte específica. A tabela a seguir lista alguns dos cenários comuns e quando eles são encontrados:  
  
|Cenário|Usar quando|  
|--------------|--------------|  
|Controle de versão de serviço|Você precisa dar suporte a várias versões de um serviço ou pode implantar um serviço atualizado no futuro|  
|Particionamento de dados de serviço|Você deve particionar um serviço em vários hosts|  
|Atualização dinâmica|Você deve reconfigurar dinamicamente a lógica de roteamento em tempo de execução para lidar com a alteração de implantações de serviço|  
|Multicast|Você deve enviar uma mensagem para vários pontos de extremidade|  
|Ponte de protocolo|Você recebe mensagens em um protocolo de transporte e o ponto de extremidade de destino usa um protocolo diferente|  
|Tratamento de erros|Você precisa fornecer resiliência para interrupções de rede e falhas de comunicação|  
  
> [!NOTE]
> Embora muitos dos cenários apresentados sejam específicos a determinadas necessidades de negócios ou requisitos de processamento, o planejamento para dar suporte a atualizações dinâmicas e a utilização do tratamento de erros geralmente pode ser considerado como práticas recomendadas, pois permitem modificar a lógica de roteamento em tempo de execução e se recuperar de falhas de rede e de comunicação transitórias.  
  
### <a name="service-versioning"></a>Controle de versão de serviço  
 Ao introduzir uma nova versão de um serviço, geralmente você deve manter a versão anterior até que todos os clientes tenham sido transferidos para o novo serviço. Isso é especialmente crítico se o serviço for um processo de longa execução que leva dias, semanas ou até meses para ser concluído. Normalmente, isso requer a implementação de um novo endereço de ponto de extremidade para o novo serviço, mantendo o ponto de extremidade original para a versão anterior.  
  
 Usando o serviço de roteamento, você pode expor um ponto de extremidade para receber mensagens de aplicativos cliente e, em seguida, rotear cada mensagem para a versão de serviço correta com base no conteúdo da mensagem. A implementação mais básica envolve a adição de um cabeçalho personalizado à mensagem que indica a versão do serviço pelo qual a mensagem deve ser processada. O serviço de roteamento pode usar o XPathMessageFilter para inspecionar cada mensagem quanto à presença do cabeçalho personalizado e encaminhar a mensagem para o ponto de extremidade de destino apropriado.  
  
 Para as etapas usadas para criar uma configuração de controle de versão de serviço, consulte [como: controle de versão de serviço](how-to-service-versioning.md).
  
### <a name="service-data-partitioning"></a>fornecer particionamento de dados  
 Ao criar um ambiente distribuído, muitas vezes é desejável espalhar a carga de processamento em vários computadores para fornecer alta disponibilidade, diminuir a carga de processamento em computadores individuais ou fornecer recursos dedicados para um subconjunto específico de mensagens. Embora o serviço de roteamento não substitua uma solução de balanceamento de carga dedicada, sua capacidade de executar o roteamento baseado em conteúdo pode ser usada para rotear mensagens semelhantes para destinos específicos. Por exemplo, você pode ter um requisito para processar mensagens de um cliente específico separadamente das mensagens recebidas de outros clientes.  
  
 Para as etapas usadas para criar uma configuração de particionamento de dados de serviço, consulte [como: particionamento de dados de serviço](how-to-service-data-partitioning.md).  
  
### <a name="dynamic-routing"></a>Roteamento dinâmico  
 Geralmente, é desejável modificar a configuração de roteamento para atender às necessidades de negócios em constante mudança, como adicionar uma rota a uma versão mais recente de um serviço, alterar os critérios de roteamento ou alterar o ponto de extremidade de destino para uma mensagem específica que o filtro roteia. O serviço de roteamento permite que você faça isso por meio do <xref:System.ServiceModel.Routing.RoutingExtension> , que permite fornecer um novo RoutingConfiguration durante o tempo de execução. A nova configuração entra em vigor imediatamente, mas afeta apenas as novas sessões processadas pelo serviço de roteamento.  
  
 Para as etapas usadas para implementar o roteamento dinâmico, consulte [como: atualização dinâmica](how-to-dynamic-update.md).
  
### <a name="multicast"></a>Multicast  
 Ao rotear mensagens, geralmente você encaminha cada mensagem para um ponto de extremidade de destino específico.  No entanto, ocasionalmente você pode precisar encaminhar uma cópia da mensagem para vários pontos de extremidade de destino. Para executar o roteamento multicast, as seguintes condições devem ser verdadeiras:  
  
- A forma de canal não deve ser solicitação-resposta (embora possa ser unidirecional ou duplex) porque a solicitação-resposta exige que apenas uma resposta possa ser recebida pelo aplicativo cliente em resposta à solicitação.  
  
- Vários filtros devem retornar **true** ao avaliar a mensagem.  
  
 Se essas condições forem atendidas, cada ponto de extremidade de destino associado a um filtro que retorna true receberá uma cópia da mensagem.  
  
### <a name="protocol-bridging"></a>Ponte de protocolo  
 Ao rotear mensagens entre protocolos SOAP diferentes, o serviço de roteamento usa APIs WCF para converter a mensagem de um protocolo para outro. Isso ocorre automaticamente quando os pontos de extremidade de serviço expostos pelo serviço de roteamento usam um protocolo diferente dos pontos de extremidade do cliente para os quais as mensagens são roteadas. É possível desabilitar esse comportamento se os protocolos em uso não forem padrão; no entanto, você deve fornecer seu próprio código de ponte.
  
### <a name="error-handling"></a>Tratamento de erros  
 Em um ambiente distribuído, não é incomum encontrar falhas transitórias de rede ou comunicação. Sem um serviço intermediário como o serviço de roteamento, a carga de lidar com essas falhas cai no aplicativo cliente. Se o aplicativo cliente não incluir uma lógica específica para tentar novamente no caso de falhas de rede ou de comunicação e o conhecimento de locais alternativos, o usuário poderá encontrar cenários em que uma mensagem deve ser enviada várias vezes antes de ser processada com êxito pelo serviço de destino. Isso pode levar à insatisfação do cliente com o aplicativo, pois ele pode ser percebido como não confiável.  
  
 O serviço de roteamento tenta corrigir esse cenário fornecendo recursos robustos de tratamento de erros para mensagens que encontram falhas relacionadas à rede ou à comunicação. Ao criar uma lista de pontos de extremidade de destino possíveis e associar essa lista a cada filtro de mensagem, você remove o ponto único de falha incorrida por ter apenas um destino possível. Em caso de falha, o serviço de roteamento tentará entregar a mensagem para o próximo ponto de extremidade na lista até que a mensagem tenha sido entregue, uma falha de não comunicação ocorrerá ou todos os pontos de extremidade tenham sido esgotados.  
  
 Para as etapas usadas para configurar o tratamento de erros, consulte [como: tratamento de erros](how-to-error-handling.md).
  
### <a name="in-this-section"></a>Nesta seção  
 [Como controlar a versão de serviço](how-to-service-versioning.md)  
  
 [Como particionar dados de serviço](how-to-service-data-partitioning.md)  
  
 [Como atualizar dinamicamente](how-to-dynamic-update.md)  
  
 [Como fazer para tratar erros](how-to-error-handling.md)  
  
## <a name="see-also"></a>Confira também

- [Introdução ao roteamento](routing-introduction.md)
