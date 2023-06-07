---
title: Filtragem
ms.date: 03/30/2017
ms.assetid: 4002946c-e34a-4356-8cfb-e25912a4be63
ms.openlocfilehash: d04141e4720320784bc92c332a3f0b96a7b1ac92
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595460"
---
# <a name="filtering"></a>Filtragem
O sistema de filtragem do Windows Communication Foundation (WCF) pode usar filtros declarativos para corresponder mensagens e tomar decisões operacionais. Você pode usar filtros para determinar o que fazer com uma mensagem examinando parte da mensagem. Um processo de enfileiramento, por exemplo, pode usar uma consulta XPath 1,0 para verificar o elemento Priority de um cabeçalho conhecido para determinar se uma mensagem deve ser movida para a frente da fila.  
  
 O sistema de filtragem é composto por um conjunto de classes que podem determinar com eficiência qual de um conjunto de filtros se `true` destina a uma mensagem específica do WCF.  
  
 O sistema de filtragem é um componente fundamental das mensagens do WCF; Ele foi projetado para ser extremamente rápido. Cada implementação de filtro foi otimizada para um determinado tipo de correspondência em relação a mensagens do WCF.  
  
 O sistema de filtragem não é thread-safe. O aplicativo deve lidar com qualquer semântica de bloqueio. No entanto, ele dá suporte a um único gravador e a vários leitores.  
  
## <a name="where-filtering-fits"></a>Onde a filtragem se ajusta  
 A filtragem é executada Depois que uma mensagem é recebida e faz parte do processo de expedição da mensagem para o componente de aplicativo apropriado. O design do sistema de filtragem aborda os requisitos de vários subsistemas do WCF, incluindo mensagens, roteamento, segurança, manipulação de eventos e gerenciamento do sistema.  
  
## <a name="filters"></a>Filtros  
 O mecanismo de filtro tem dois componentes principais, filtros e tabelas de filtro. Um filtro faz decisões booleanas sobre uma mensagem com base em condições lógicas especificadas pelo usuário. Os filtros implementam a <xref:System.ServiceModel.Dispatcher.MessageFilter> classe.  
  
 Os <xref:System.ServiceModel.Dispatcher.MessageFilter.Match%2A> métodos são usados para determinar se uma mensagem atende a um filtro. Um dos métodos testa o cabeçalho da mensagem, mas não pode inspecionar o corpo da mensagem. O outro método usa um *buffer de mensagens* como um parâmetro de entrada e pode inspecionar o corpo da mensagem.  
  
 Os filtros geralmente não são testados individualmente, mas como parte de uma tabela de filtro, que é uma classe genérica que o <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601.CreateFilterTable%2A> método cria.  
  
 Os vários tipos de filtros especializados em correspondência em um determinado tipo de condição booliana. Depois de construir um filtro, você não pode alterar os critérios que um filtro usa; para modificar os critérios de um filtro, construa um novo e exclua o filtro existente.  
  
### <a name="action-filters"></a>Filtros de ação  
 O <xref:System.ServiceModel.Dispatcher.ActionMessageFilter> contém uma lista de cadeias de caracteres de ação. Se qualquer uma das ações na lista do filtro corresponder ao cabeçalho da ação no buffer da mensagem ou da mensagem, o `Match` método retornará `true` . Se a lista estiver vazia, o filtro será considerado um filtro de correspondência-todos e qualquer mensagem ou buffer de mensagem corresponder e `Match` retornará `true` . Se nenhuma das ações na lista do filtro corresponder ao cabeçalho de ação na mensagem ou no buffer de mensagens, o `Match` retornará `false` . Se não houver nenhuma ação na mensagem e a lista do filtro não estiver vazia, o `Match` retornará `false` .  
  
### <a name="endpoint-address-filters"></a>Filtros de endereço do ponto de extremidade  
 O <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> Filtra mensagens e buffers de mensagens com base em um endereço de ponto de extremidade, conforme representado em sua coleção de cabeçalhos. Para que uma mensagem passe tal filtro, as seguintes condições devem ser atendidas:  
  
- O endereço do filtro Uniform Resource Identifier (URI) deve ser o mesmo que o cabeçalho da mensagem para.  
  
- Cada parâmetro de ponto de extremidade no endereço do filtro ( `address.Headers` coleção) deve localizar um cabeçalho na mensagem para mapear. Cabeçalhos extras na mensagem ou no buffer de mensagens são aceitáveis para que a correspondência permaneça `true` .  
  
### <a name="prefix-endpoint-address-filters"></a>Prefixo dos filtros de endereço do ponto de extremidade  
  
1. As <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> funções são assim como o <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> filtro, exceto que a correspondência pode estar em um prefixo do URI da mensagem. Por exemplo, um filtro que especifica o endereço `http://www.adatum.com` corresponde às mensagens endereçadas para `http://www.adatum.com/userA` .  
  
### <a name="xpath-message-filters"></a>Filtros de mensagem XPath  
 Um <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> usa uma expressão XPath para determinar se um documento XML contém elementos específicos, atributos, texto ou outras construções sintáticas XML. O filtro é otimizado para ser extremamente eficiente para um subconjunto estrito do XPath. A linguagem de caminho XML é descrita na [especificação W3C XML Path Language 1,0](https://www.w3.org/TR/xpath/all/).  
  
 Normalmente, um aplicativo usa um <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> em um ponto de extremidade para consultar o conteúdo de uma mensagem SOAP e, em seguida, executa a ação apropriada com base nos resultados dessa consulta. Um processo de enfileiramento, por exemplo, pode usar uma consulta XPath para inspecionar o elemento Priority de um cabeçalho conhecido para decidir se deve mover uma mensagem para a frente da fila.  
  
## <a name="filter-tables"></a>Filtrar tabelas  
 As tabelas de filtro são usadas para armazenar pares chave-valor, onde um filtro é a chave e alguns dados associados são o valor. Os dados de filtro podem ser usados para indicar as ações a serem executadas se uma mensagem corresponder ao filtro e o tipo dos dados do filtro for o parâmetro genérico para a classe da tabela de filtro. Os dados de filtro podem consistir em regras de roteamento, estado de segurança da sessão, ouvintes em um canal e assim por diante. Os dados podem ser usados onde o controle de fluxo de dados é necessário.  
  
 As tabelas de filtro implementam a interface genérica <xref:System.ServiceModel.Dispatcher.IMessageFilterTable%601> .  
  
 As tabelas de filtro têm vários métodos que correspondem a uma mensagem em relação a todos os filtros na tabela e retornam uma coleção não ordenada de filtros ou dados correspondentes. Alguns dos métodos de correspondência são de várias correspondências e retornam todos os itens correspondentes. Outros são correspondência única, retornando apenas um item e lançam um <xref:System.ServiceModel.Dispatcher.MultipleFilterMatchesException> se mais de um filtro corresponder.  
  
### <a name="message-filter-table"></a>Tabela de filtro de mensagem  
 O <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601> é a implementação mais geral do <xref:System.ServiceModel.Dispatcher.IMessageFilterTable%601> . Você pode armazenar filtros de todos os tipos na tabela.  
  
 Você pode atribuir prioridades numéricas a filtros, em que a prioridade mais alta é representada pelo número mais alto. Vários tipos de filtros podem ter a mesma prioridade. Um tipo específico de filtro pode aparecer em mais de um nível de prioridade.  
  
 A correspondência é feita a partir da prioridade mais alta e, uma vez que os filtros correspondentes são encontrados com uma determinada prioridade, nenhum filtro com prioridades inferiores é examinado. Portanto, se você estiver usando um método de correspondência de filtro único e mais de um filtro corresponder a uma mensagem, mas cada filtro correspondente tiver uma prioridade diferente, nenhuma exceção será lançada e o filtro com a prioridade mais alta será retornado. Da mesma forma, um método de correspondência de vários filtros retorna apenas os filtros correspondentes com a prioridade mais alta.  
  
### <a name="xpath-message-filter-table"></a>Tabela de filtro de mensagem XPath  
 O <xref:System.ServiceModel.Dispatcher.XPathMessageFilterTable%601> é otimizado para filtros XPath declarativos, portanto, a chave de tabela é um <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> .  
  
 A <xref:System.ServiceModel.Dispatcher.XPathMessageFilterTable%601> classe otimiza a correspondência para um subconjunto de XPath que abrange a maioria dos cenários de mensagens e também dá suporte à gramática completa do xpath 1,0. Ele tem algoritmos otimizados para uma correspondência paralela eficiente.  
  
 Esta tabela tem vários `Match` métodos especializados que operam em um <xref:System.Xml.XPath.XPathNavigator> e um <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> . Um <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> estende a <xref:System.Xml.XPath.XPathNavigator> classe adicionando uma <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator.CurrentPosition%2A> propriedade. Essa propriedade permite que posições dentro do documento XML sejam salvas e carregadas rapidamente sem a necessidade de clonar o navegador, uma alocação de memória cara que o <xref:System.Xml.XPath.XPathNavigator> exige para essa operação. O mecanismo XPath do WCF sempre deve registrar a posição do cursor no decorrer da execução de consultas em documentos XML, de modo que o <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> fornece uma otimização importante para o processamento de mensagens.  
  
## <a name="customer-scenarios"></a>Cenários de clientes  
 Você pode usar a filtragem sempre que desejar enviar uma mensagem para diferentes módulos de processamento, dependendo dos dados contidos na mensagem. Dois cenários típicos são rotear uma mensagem com base em seu código de ação e Desmultiplexar um fluxo de mensagens com base no endereço do ponto de extremidade das mensagens.  
  
### <a name="routing"></a>Roteamento  
 O ouvinte de um ponto de extremidade escuta mensagens que têm um ou mais códigos de ação no cabeçalho SOAP da mensagem. Implemente isso criando um <xref:System.ServiceModel.Dispatcher.ActionMessageFilter> passando uma matriz que contém os códigos de ação para seu construtor. Ele usa esse filtro para se registrar no `ListenerFactory` , portanto, somente as mensagens cuja ação corresponde a uma delas no filtro obtêm esse ponto de extremidade específico.  
  
### <a name="de-multiplexing"></a>Desmultiplexação  
 Quando vários pontos de extremidade são `ServiceListener` desligados do mesmo cabo, a única maneira de Desmultiplexar mensagens e saber se elas pertencem a um determinado endereço de ponto de extremidade, é usar <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> s, que selecionam mensagens para os pontos de extremidades registrados, executando uma pesquisa sobre as informações armazenadas nos cabeçalhos. Nesses filtros, somente as mensagens que passam têm todos os cabeçalhos necessários que correspondem a ambos:  
  
- O URI no `EndpointAddress` .  
  
- O restante dos parâmetros de ponto de extremidade no `EndpointAddress` conforme especificado no <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> .  
  
## <a name="see-also"></a>Consulte também

- [Serialização e transferência de dados](data-transfer-and-serialization.md)
