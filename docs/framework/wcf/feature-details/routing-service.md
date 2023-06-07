---
title: Serviço de roteamento
ms.date: 03/30/2017
ms.assetid: ca7c216a-5141-4132-8193-102c181d2eba
ms.openlocfilehash: 833c824e17d70a982a2f7bb13fe388b9b2b0dec1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590443"
---
# <a name="routing-service"></a>Serviço de roteamento

O serviço de roteamento é um intermediário SOAP genérico que atua como um roteador de mensagem. A funcionalidade principal do serviço de roteamento é a capacidade de rotear mensagens com base no conteúdo da mensagem, o que permite que uma mensagem seja encaminhada a um ponto de extremidade do cliente com base em um valor dentro da própria mensagem, no cabeçalho ou no corpo da mensagem.

O <xref:System.ServiceModel.Routing.RoutingService> é implementado como um serviço de Windows Communication Foundation (WCF) no <xref:System.ServiceModel.Routing> namespace. O serviço de roteamento expõe um ou mais pontos de extremidade de serviço que recebem mensagens e, em seguida, roteia cada mensagem para um ou mais pontos de extremidade do cliente com base no conteúdo da mensagem. O serviço fornece os seguintes recursos:

- Roteamento baseado em conteúdo

  - Agregação de serviço

  - Controle de versão de serviço

  - Roteamento de prioridade

  - Configuração dinâmica

- Ponte de protocolo

- Processamento de SOAP

- Tratamento de erro avançado

- Pontos de extremidade de backup

Embora seja possível criar um serviço intermediário que realiza uma ou mais dessas metas, muitas vezes, essa implementação é vinculada a um cenário ou solução específica e não pode ser aplicada prontamente a novos aplicativos.

O serviço de roteamento fornece um intermediário SOAP autônomo e autônomo, que é compatível com os modelos de serviço e canal do WCF e permite que você execute o roteamento baseado em conteúdo de mensagens baseadas em SOAP.

> [!NOTE]
> Atualmente, o serviço de roteamento não dá suporte ao roteamento de serviços REST do WCF.  Para rotear chamadas REST, considere usar <xref:System.Web.Routing> ou [Application Request Routing](https://go.microsoft.com/fwlink/?LinkId=164589).

## <a name="content-based-routing"></a>Roteamento baseado em conteúdo

O roteamento baseado em conteúdo é a capacidade de rotear uma mensagem com base em um ou mais valores contidos na mensagem. O serviço de roteamento inspeciona cada mensagem e a encaminha para o ponto de extremidade de destino com base no conteúdo da mensagem e na lógica de roteamento que você cria. O roteamento baseado em conteúdo fornece a base para agregação de serviço, controle de versão de serviço e roteamento de prioridade.

Para implementar o roteamento baseado em conteúdo, o serviço de roteamento depende das <xref:System.ServiceModel.Dispatcher.MessageFilter> implementações usadas para corresponder valores específicos dentro das mensagens a serem roteadas. Se um **MessageFilter** corresponder a uma mensagem, a mensagem será roteada para o ponto de extremidade de destino associado ao **MessageFilter**.  Os filtros de mensagem são agrupados em filtrar tabelas ( <xref:System.ServiceModel.Routing.Configuration.FilterTableCollection> ) para construir uma lógica de roteamento complexa. Por exemplo, uma tabela de filtros pode conter cinco filtros de mensagens mutuamente exclusivos que fazem com que as mensagens sejam roteadas para apenas um dos cinco pontos de extremidade de destino.

O serviço de roteamento permite configurar a lógica usada para executar o roteamento baseado em conteúdo, bem como atualizar dinamicamente a lógica de roteamento em tempo de execução.

Por meio do agrupamento de filtros de mensagem em tabelas de filtro, a lógica de roteamento pode ser construída, permitindo que você manipule vários cenários de roteamento, como:

- Agregação de serviço

- Controle de versão de serviço

- Roteamento de prioridade

- Configuração dinâmica

Para obter mais informações sobre filtros de mensagens e tabelas de filtro, consulte [introdução ao roteamento](routing-introduction.md) e [filtros de mensagens](message-filters.md).

### <a name="service-aggregation"></a>Agregação de serviço

Usando o roteamento baseado em conteúdo, você pode expor um ponto de extremidade que recebe mensagens de aplicativos cliente externos e, em seguida, roteia cada mensagem para o ponto de extremidade interno apropriado com base em um valor dentro da mensagem. Isso é útil para oferecer um ponto de extremidade específico para uma variedade de aplicativos de back-end, e também apresentar um ponto de extremidade de aplicativo para os clientes ao mesmo tempo em que o aplicativo é composto por uma variedade de serviços.

### <a name="service-versioning"></a>Controle de versão de serviço

Ao migrar para uma nova versão da sua solução, talvez seja necessário manter a versão antiga em paralelo para atender aos clientes existentes. Geralmente, isso requer que os clientes que se conectam à versão mais recente precisem usar um endereço diferente ao se comunicar com a solução. O serviço de roteamento permite expor um ponto de extremidade de serviço que atende a ambas as versões de sua solução Roteando mensagens para a solução apropriada com base nas informações específicas da versão contidas na mensagem. Para obter um exemplo de tal implementação, consulte [como: controle de versão do serviço](how-to-service-versioning.md).

### <a name="priority-routing"></a>Roteamento de prioridade

Ao fornecer um serviço para vários clientes, você pode ter um SLA (contrato de nível de serviço) com alguns parceiros que exijam que todos os dados desses parceiros sejam processados separadamente dos outros clientes. Usando um filtro que procura informações específicas do cliente contidas na mensagem, você pode facilmente rotear mensagens de parceiros específicos para um ponto de extremidade que foi criado para atender aos seus requisitos de SLA.

## <a name="dynamic-configuration"></a>Configuração dinâmica

Para dar suporte a sistemas críticos, onde as mensagens devem ser processadas sem interrupções de serviço, é vital que você possa modificar a configuração de componentes no sistema em tempo de execução. Para dar suporte a essa necessidade, o serviço de roteamento fornece uma <xref:System.ServiceModel.IExtension%601> implementação, o <xref:System.ServiceModel.Routing.RoutingExtension> , que permite a atualização dinâmica da configuração do serviço de roteamento em tempo de execução.

Para obter mais informações sobre a configuração dinâmica do serviço de roteamento, consulte [introdução ao roteamento](routing-introduction.md).

## <a name="protocol-bridging"></a>Ponte de protocolo

Um dos desafios em cenários intermediários é que os pontos de extremidade internos podem ter diferentes requisitos de transporte ou de versão de SOAP do que as mensagens recebidas. Para dar suporte a esse cenário, o serviço de roteamento pode ligar protocolos, incluindo o processamento da mensagem SOAP para o <xref:System.ServiceModel.Channels.MessageVersion> exigido pelos pontos de extremidade de destino. Dessa forma, um protocolo pode ser usado para comunicação interna, enquanto outro pode ser usado para comunicação externa.

Para dar suporte ao roteamento de mensagens entre pontos de extremidade com transportes diferentes, o serviço de roteamento usa associações fornecidas pelo sistema que habilitam o serviço a ligar protocolos distintas. Isso ocorre automaticamente quando o ponto de extremidade de serviço exposto pelo serviço de roteamento usa um protocolo diferente dos pontos de extremidades do cliente para os quais as mensagens são roteadas.

## <a name="soap-processing"></a>Processamento de SOAP

Um requisito de roteamento comum é a capacidade de rotear mensagens entre pontos de extremidade com diferentes requisitos de SOAP. Para dar suporte a esse requisito, o serviço de roteamento fornece um <xref:System.ServiceModel.Routing.SoapProcessingBehavior> que cria automaticamente uma nova **MessageVersion** que atende aos requisitos do ponto de extremidade de destino antes que a mensagem seja roteada para ele. Esse comportamento também cria uma nova **MessageVersion** para qualquer mensagem de resposta antes de retorná-la ao aplicativo cliente solicitante, para garantir que a **MessageVersion** da resposta corresponda à da solicitação original.

Para obter mais informações sobre o processamento de SOAP, consulte [introdução ao roteamento](routing-introduction.md).

## <a name="error-handling"></a>Tratamento de erros

Em um sistema composto por serviços distribuídos que dependem de comunicações de rede, é importante garantir que as comunicações em seu sistema sejam resistentes a falhas de rede transitórias.  O serviço de roteamento implementa o tratamento de erros que permite que você manipule muitos cenários de falha de comunicação que, de outra forma, podem resultar em uma interrupção do serviço.

Se o serviço de roteamento encontrar um <xref:System.ServiceModel.CommunicationException> durante a tentativa de enviar uma mensagem, ocorrerá o tratamento de erros.  Essas exceções normalmente indicam que um problema foi encontrado durante a tentativa de comunicação com o ponto de extremidade do cliente definido, como um <xref:System.ServiceModel.EndpointNotFoundException> , <xref:System.ServiceModel.ServerTooBusyException> ou <xref:System.ServiceModel.CommunicationObjectFaultedException> .  O código de tratamento de erros também detectará e tentará tentar enviar novamente quando um **TimeoutException** ocorrer, que é outra exceção comum que não é derivada de **CommunicationException**.

Para obter mais informações sobre o tratamento de erros, consulte [introdução ao roteamento](routing-introduction.md).

## <a name="backup-endpoints"></a>Pontos de extremidade de backup

Além dos pontos de extremidade do cliente de destino associados a cada definição de filtro na tabela de filtro, você também pode criar uma lista de pontos de extremidade de backup para os quais a mensagem será roteada no caso de uma falha de transmissão. Se ocorrer um erro e uma lista de backup for definida para a entrada de filtro, o serviço de roteamento tentará enviar a mensagem para o primeiro ponto de extremidade definido na lista. Se essa tentativa de transmissão falhar, o serviço tentará o próximo ponto de extremidade e continuará esse processo até que a tentativa de transmissão seja bem-sucedida, retorna um erro não relacionado à transmissão ou todos os pontos de extremidade na lista de backup retornaram um erro de transmissão.

Para obter mais informações sobre pontos de extremidade de backup, consulte [introdução ao roteamento](routing-introduction.md) e [filtros de mensagens](message-filters.md).

## <a name="streaming"></a>Streaming

O serviço de roteamento pode transmitir mensagens com êxito se você definir a associação para dar suporte ao streaming.  No entanto, há algumas condições sob as quais as mensagens podem precisar ser armazenadas em buffer:

- Multicast (buffer para criar cópias de mensagens adicionais)

- Failover (buffer no caso de a mensagem precisar ser enviada para um backup)

- System. ServiceModel. Routing. RoutingConfiguration. RouteOnHeadersOnly é false (buffer para apresentar o MessageFilterTable com um MessageBuffer para que os filtros possam inspecionar o corpo)

- Configuração dinâmica

## <a name="see-also"></a>Confira também

- [Introdução ao roteamento](routing-introduction.md)
- [Contratos de roteamento](routing-contracts.md)
- [Filtros de mensagem](message-filters.md)
