---
title: O que é o Windows Communication Foundation
description: Saiba mais sobre o Windows Communication Foundation, que é uma estrutura para a criação de aplicativos orientados a serviços.
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], technology overview
- technology overview [WCF]
- WCF [WCF], technology overview
ms.assetid: 40e1009d-ef15-450b-9848-62eabe5e5738
ms.openlocfilehash: 84cb45d62409769a79fa6a401fdb1aa6934c4099
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245603"
---
# <a name="what-is-windows-communication-foundation"></a>O que é o Windows Communication Foundation
O Windows Communication Foundation (WCF) é uma estrutura para a criação de aplicativos orientados a serviços. Usando o WCF, você pode enviar dados como mensagens assíncronas de um ponto de extremidade de serviço para outro. Um ponto de extremidade de serviço pode ser parte de um serviço disponível continuamente hospedado pelo IIS, ou pode ser um serviço hospedado em um aplicativo. Um ponto de extremidade pode ser um cliente de um serviço que solicita dados de um ponto de extremidade de serviço. As mensagens podem ser simples como um único caractere ou uma palavra enviada como XML ou complexo como um fluxo de dados binários. Alguns cenários de exemplo incluem:

- Um serviço seguro para processar transações comerciais.

- Um serviço que fornece dados atuais para outros, como um relatório de tráfego ou outro serviço de monitoramento.

- Um serviço de chat que permite que duas pessoas se comuniquem ou troquem dados em tempo real.

- Um aplicativo do painel que pesquisa um ou mais serviços para dados e os apresenta em uma apresentação lógica.

- Expõe um fluxo de trabalho implementado usando o Windows Workflow Foundation como um serviço WCF.

- Um aplicativo do Silverlight que pesquisa um serviço para os feeds de dados mais recentes.

Embora a criação desses aplicativos fosse possível antes da existência do WCF, o WCF facilita ainda mais o desenvolvimento de pontos de extremidade. Em resumo, o WCF foi projetado para oferecer uma abordagem gerenciável para a criação de serviços Web e clientes de serviço Web.

## <a name="features-of-wcf"></a>Recursos do WCF

O WCF inclui o seguinte conjunto de recursos. Para obter mais informações, consulte [detalhes de recursos do WCF](./feature-details/index.md).

- **Orientação de serviço**

     Uma consequência de usar os padrões do WS é que o WCF permite que você crie aplicativos *orientados a serviços* . A arquitetura orientada a serviços (SOA) é a confiança nos serviços Web para enviar e receber dados. Os serviços têm a vantagem geral de serem fracamente acoplados em vez de embutidos no código de um aplicativo para outro. Uma relação fracamente acoplada significa que qualquer cliente criado em qualquer plataforma pode se conectar a qualquer serviço contanto que os contratos essenciais sejam atendidos.

- **Interoperabilidade**

     O WCF implementa padrões modernos da indústria para interoperabilidade de serviço Web. Para obter mais informações sobre os padrões com suporte, consulte [interoperabilidade e integração](./feature-details/interoperability-and-integration.md).

- **Vários padrões de mensagem**

     As mensagens são trocadas em um dos vários padrões. O padrão mais comum é o padrão de solicitação/resposta, onde um ponto de extremidade solicita dados de um segundo ponto de extremidade. O segundo ponto de extremidade responde. Há outros padrões como uma mensagem unidirecional em que um único ponto de extremidade envia uma mensagem sem nenhuma expectativa de resposta. Um padrão mais complexo é o padrão de troca duplex onde dois pontos de extremidade estabelecem uma conexão de dados e enviar para frente e para trás, semelhante a um programa de mensagens instantâneas. Para obter mais informações sobre como implementar diferentes padrões de troca de mensagens usando o WCF, consulte [contratos](./feature-details/contracts.md).

- **Metadados de serviço**

     O WCF dá suporte à publicação de metadados de serviço usando formatos especificados em padrões do setor, como WSDL, esquema XML e WS-Policy. Esses metadados podem ser usados para gerar e configurar automaticamente clientes para acessar serviços WCF. Os metadados podem ser publicados em HTTP e HTTPS ou usando o padrão Metadata Exchange do Serviço Web. Para obter mais informações, consulte [metadados](./feature-details/metadata.md).

- **Contratos de dados**

     Como o WCF é criado usando o .NET Framework, ele também inclui métodos amigáveis de código para fornecer os contratos que você deseja impor. Um dos tipos universais de contratos é o contrato de dados. Essencialmente, à medida que você codifica o serviço usando Visual C# ou Visual Basic, a maneira mais fácil de manipular dados é criando classes que representam uma entidade de dados com as propriedades que pertencem à entidade de dados. O WCF inclui um sistema abrangente para trabalhar com dados dessa maneira fácil. Assim que você tiver criado as classes que representam dados, o serviço gera automaticamente os metadados que permitem que os clientes sejam compatíveis com os tipos de dados que você criou. Para obter mais informações, consulte [usando contratos de dados](feature-details/using-data-contracts.md).

- **Segurança**

     As mensagens podem ser criptografadas para proteger a privacidade e você pode exigir que os usuários se autentiquem antes de terem permissão de receber mensagens. A segurança pode ser implementada usando padrões conhecidos como SSL ou WS-SecureConversation. Para saber mais, consulte [Segurança](./feature-details/security.md).

- **Vários transportes e codificações**

     As mensagens podem ser enviadas de qualquer um dos vários protocolos e codificações internos de transporte. O protocolo e a codificação mais comuns são enviar mensagens SOAP codificadas para texto usando o protocolo HTTP para uso no World Wide Web. Como alternativa, o WCF permite que você envie mensagens por TCP, pipes nomeados ou MSMQ. Essas mensagens podem ser codificadas como texto ou usando um formato binário otimizado.  Os dados binários podem ser enviados com eficiência usando o padrão MTOM. Se nenhum dos transportes ou codificações fornecidos atender às suas necessidades, você poderá criar seu próprio transporte ou codificação personalizado. Para obter mais informações sobre transportes e codificações com suporte do WCF, consulte [transportes](./feature-details/transports.md).

- **Mensagens confiáveis e na fila**

     O WCF dá suporte à troca de mensagens confiável usando sessões confiáveis implementadas em mensagens WS-Reliable e usando o MSMQ. Para obter mais informações sobre o suporte a mensagens confiáveis e enfileiradas no WCF [, consulte filas e sessões confiáveis](./feature-details/queues-and-reliable-sessions.md).

- **Mensagens duráveis**

     Uma mensagem é durável porque nunca é perdida devido a um rompimento na comunicação. As mensagens em um padrão durável são sempre salvas em um banco de dados. Se um rompimento ocorrer, o banco de dados permitirá que você retome a troca de mensagens quando a conexão for restaurada. Você também pode criar uma mensagem durável usando o Windows Workflow Foundation (WF). Para obter mais informações, consulte [Workflow Services](./feature-details/workflow-services.md).

- **Transações**

     O WCF também dá suporte a transações usando um dos três modelos de transação: WS-AtomicTransactions, APIs no <xref:System.Transactions> namespace e Microsoft coordenador de transações distribuídas. Para obter mais informações sobre o suporte a transações no WCF, consulte [Transações](./feature-details/transactions-in-wcf.md).

- **Suporte AJAX e REST**

     REST é um exemplo de uma tecnologia Web 2.0 em evolução. O WCF pode ser configurado para processar dados XML "simples" que não são encapsulados em um envelope SOAP. O WCF também pode ser estendido para dar suporte a formatos XML específicos, como ATOM (um padrão RSS popular) e até mesmo formatos não XML, como JavaScript Object Notation (JSON).

- **Extensibilidade**

     A arquitetura do WCF tem vários pontos de extensibilidade. Se o recurso adicional for necessário, há inúmeros pontos de entrada que permitem que você personalize o comportamento de um serviço. Para obter mais informações sobre pontos de extensibilidade disponíveis, consulte [estendendo o WCF](./extending/index.md).

## <a name="wcf-integration-with-other-microsoft-technologies"></a>Integração WCF com outras tecnologias da Microsoft

O WCF é uma plataforma flexível. Devido a essa flexibilidade extrema, o WCF também é usado em vários outros produtos da Microsoft. Ao compreender os conceitos básicos do WCF, você terá uma vantagem imediata se também usar qualquer um desses produtos.

A primeira tecnologia a ser emparelhada com o WCF foi a Windows Workflow Foundation (WF). Os fluxos de trabalho simplificam o desenvolvimento de aplicativos encapsulando as etapas no fluxo de trabalho como "atividades". Na primeira versão do Windows Workflow Foundation, um desenvolvedor tinha que criar um host para o fluxo de trabalho. A próxima versão do Windows Workflow Foundation foi integrada ao WCF. Que permitia que qualquer fluxo de trabalho fosse facilmente hospedado em um serviço WCF. Você pode fazer isso escolhendo automaticamente o tipo de projeto WF/WCF no Visual Studio 2012 ou posterior.

O Microsoft BizTalk Server R2 também utiliza o WCF como uma tecnologia de comunicação. O BizTalk é criado para receber e transformar dados de um formato padronizado para outro. As mensagens devem ser entregues a sua caixa de mensagens central onde a mensagem pode ser transformada usando um mapeamento restrito ou usando um dos recursos de BizTalk como o mecanismo de fluxo de trabalho. O BizTalk agora pode usar o adaptador de linha de negócios (LOB) do WCF para entregar mensagens à caixa de mensagem.

O Microsoft Silverlight é uma plataforma para criar aplicativos Web interoperáveis, aplicativos Web avançados que permitem que os desenvolvedores criem sites voltados para mídia (como a streaming de vídeo). A partir da versão 2, o Silverlight incorporou o WCF como uma tecnologia de comunicação para conectar os aplicativos do Silverlight aos pontos de extremidade do WCF.

Os recursos de hospedagem do servidor de aplicativos do Windows Server AppFabric foram projetados especificamente para implantar e gerenciar aplicativos que usam o WCF para comunicação. Os recursos de hospedagem incluem ferramentas avançadas e opções de configuração projetadas especificamente para aplicativos habilitados para WCF.

## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel>
- [Conceitos fundamentais do Windows Communication Foundation](fundamental-concepts.md)
- [Arquitetura do Windows Communication Foundation](architecture.md)
- [Diretrizes e práticas recomendadas](guidelines-and-best-practices.md)
- [Tutorial de Introdução](getting-started-tutorial.md)
- [Guia da documentação](guide-to-the-documentation.md)
- [Programação básica do WCF](basic-wcf-programming.md)
- [Exemplos do Windows Communication Foundation](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751514%28v=vs.90%29)
