---
title: Visão geral de sessões confiáveis
ms.date: 03/30/2017
ms.assetid: a7fc4146-ee2c-444c-82d4-ef6faffccc2d
ms.openlocfilehash: a85a34c5e2ec7928c01586e4b01cdf5e90e896a7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601082"
---
# <a name="reliable-sessions-overview"></a>Visão geral de sessões confiáveis

O Windows Communication Foundation (WCF) mensagens confiáveis SOAP fornece a confiabilidade de transferência de mensagens de ponta a ponta entre pontos de extremidade SOAP. Ele faz isso em redes que não são confiáveis, superando falhas de transporte e falhas no nível de mensagem SOAP. Em particular, ele fornece entrega ordenada baseada em sessão, única e (opcionalmente) para mensagens enviadas entre intermediários de transporte ou SOAP. A entrega baseada em sessão fornece o agrupamento de mensagens em uma sessão com a ordenação opcional das mensagens.

Este tópico descreve as sessões confiáveis, como e quando usá-las e como protegê-las.

## <a name="wcf-reliable-sessions"></a>Sessões confiáveis do WCF

As sessões confiáveis do WCF são uma implementação de mensagens confiáveis do SOAP, conforme definido pelo protocolo WS-ReliableMessaging.

O WCF SOAP Reliable Messaging fornece uma sessão confiável de ponta a ponta entre dois pontos de extremidade, independentemente do número ou tipo de intermediários que separam os pontos de extremidade de mensagens. Isso inclui os intermediários de transporte que não usam SOAP (por exemplo, proxies HTTP) ou intermediários que usam SOAP (por exemplo, roteadores ou pontes baseados em SOAP) que são necessários para que as mensagens fluam entre os pontos de extremidade. Um canal de sessão confiável dá suporte à comunicação *interativa* para que os serviços conectados por tal canal sejam executados simultaneamente e troquem e processem mensagens em condições de baixa latência, ou seja, dentro de intervalos de tempo relativamente curtos. Esse acoplamento significa que esses componentes fazem o progresso juntos ou que falham juntos, portanto, não há isolamento fornecido entre eles.

Uma sessão confiável mascara dois tipos de falhas:

- Falhas no nível de mensagem SOAP, que incluem mensagens perdidas ou duplicadas e mensagens que chegam em uma ordem diferente da ordem em que foram enviadas.

- Falhas de transporte.

Uma sessão confiável implementa o protocolo WS-ReliableMessaging e uma janela de transferência na memória para mascarar falhas de nível de mensagem SOAP e restabelece conexões no caso de falhas de transporte.

Uma sessão confiável fornece mensagens SOAP que o TCP fornece para pacotes IP. Uma conexão de soquete TCP fornece uma transferência em ordem singular de pacotes IP entre nós. O canal confiável fornece o mesmo tipo de transferência confiável, mas difere da confiabilidade do soquete TCP das seguintes maneiras:

- A confiabilidade está no nível de mensagem SOAP, não para um pacote de bytes de tamanho arbitrariamente.

- A confiabilidade é de transporte neutro, não apenas para transferência por TCP.

- A confiabilidade não está vinculada a uma sessão de transporte específica (por exemplo, a sessão que uma conexão TCP fornece) e pode usar várias sessões de transporte simultaneamente ou sequencialmente ao longo do tempo de vida da sessão confiável.

- A sessão confiável está entre os pontos de extremidade SOAP do remetente e do receptor, independentemente do número de conexões de transporte necessárias para a conectividade entre eles. Em suma, a confiabilidade de TCP termina onde a conexão de transporte termina, enquanto uma sessão confiável fornece confiabilidade de ponta a ponta.

## <a name="reliable-sessions-and-bindings"></a>Associações e sessões confiáveis

Como mencionado anteriormente, uma sessão confiável é um transporte neutro. Além disso, você pode estabelecer uma sessão confiável em vários padrões de troca de mensagens, como solicitação-resposta ou duplex. Uma sessão confiável do WCF é exposta como uma propriedade de um conjunto de associações.

Use uma sessão confiável nos pontos de extremidade que usam:

- Associações padrão de transporte baseadas em HTTP:

  - `WsHttpBinding`e expõem contratos de solicitação-resposta ou unidirecionais.

  - Ao usar uma sessão confiável em uma solicitação-resposta ou um contrato de serviço unidirecional simples.

  - `WsDualHttpBinding`e expõem os contratos duplex, solicitação-resposta ou unidirecional.

  - `WsFederationHttpBinding`e expõem contratos de solicitação-resposta ou unidirecionais.

- Associações padrão de transporte baseadas em TCP:

  - `NetTcpBinding`e expõem duplex, resposta de solicitação ou contratos unidirecionais.

Use uma sessão confiável em qualquer outra associação criando uma associação personalizada, como HTTPS (para obter mais informações sobre problemas, consulte <a href="#reliable-sessions-and-security">segurança e sessões confiáveis</a>) ou uma associação de pipe nomeado.

Você pode empilhar uma sessão confiável em diferentes tipos de canal subjacentes e a forma de canal de sessão confiável resultante varia. No cliente e no servidor, o tipo de canal de sessão confiável com suporte depende do tipo de canal subjacente usado. A tabela a seguir lista os tipos de canais de sessão com suporte no cliente como uma função do tipo de canal subjacente.

| Tipos de canal de sessão confiável com suporte&#8224; | `IRequestChannel` | `IRequestSessionChannel` | `IDuplexChannel` | `IDuplexSessionChannel` |
| ----------------------------------------------- | :---------------: | :----------------------: | :--------------: | :---------------------: |
| `IOutputSessionChannel`                         | Sim               | Sim                      | Sim              | Sim                     |
| `IRequestSessionChannel`                        | Sim               | Sim                      | Não               | Não                      |
| `IDuplexSessionChannel`                         | Não                | Não                       | Sim              | Sim                     |

&#8224;os tipos de canal com suporte são os valores disponíveis para o `TChannel` valor do parâmetro genérico que é passado para o <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.BuildChannelFactory%60%601%28System.ServiceModel.Channels.BindingContext%29> método.

A tabela a seguir lista os tipos de canais de sessão com suporte no servidor como uma função do tipo de canal subjacente.

| Tipos de canal de sessão confiável com suporte&#8225; | `IReplyChannel` | `IReplySessionChannel` | `IDuplexChannel` | `IDuplexSessionChannel` |
| ----------------------------------------------- | :-------------: | :--------------------: | :--------------: | :---------------------: |
| `IInputSessionChannel`                          | Sim             | Sim                    | Sim              | Sim                     |
| `IReplySessionChannel`                          | Sim             | Sim                    | Não               | Não                      |
| `IDuplexSessionChannel`                         | Não              | Não                     | Sim              | Sim                     |

&#8225;os tipos de canal com suporte são os valores disponíveis para o `TChannel` valor do parâmetro genérico que é passado para o <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.BuildChannelListener%60%601%28System.ServiceModel.Channels.BindingContext%29> método.

## <a name="reliable-sessions-and-security"></a>Segurança e sessões confiáveis

A proteção de uma sessão confiável é importante para garantir que as partes de comunicação (serviço e cliente) sejam autenticadas e que as mensagens trocadas na sessão não sejam violadas. Além disso, é importante garantir a integridade de cada sessão confiável individual. Uma sessão confiável é protegida ligando-a a um contexto de segurança que é representado e gerenciado por um canal de sessão de segurança. O canal de segurança fornece uma sessão de segurança. Os tokens de segurança trocados durante o estabelecimento da sessão são usados para proteger as mensagens na sessão confiável.

Quando uma sessão confiável é sobre TCP-S, a sessão TCP é vinculada à sessão confiável. Portanto, a segurança de transporte garante que a segurança também esteja vinculada à sessão confiável. Nesse caso, o restabelecimento da conexão é desativado.

A única exceção é ao usar HTTPS. A sessão protocolo SSL (SSL) não está associada à sessão confiável. Isso impõe uma ameaça porque as sessões que estão compartilhando um contexto de segurança (a sessão SSL) não são protegidas umas das outras; Isso pode ou não ser uma ameaça real, dependendo do aplicativo.

## <a name="using-reliable-sessions"></a>Usando sessões confiáveis

Para usar sessões confiáveis do WCF, crie um ponto de extremidade com uma associação que ofereça suporte a uma sessão confiável. Use uma das associações fornecidas pelo sistema que o WCF fornece com a sessão confiável habilitada ou crie sua própria associação personalizada que faz isso.

As associações definidas pelo sistema que dão suporte e habilitam uma sessão confiável por padrão incluem:

- <xref:System.ServiceModel.WSDualHttpBinding>

As associações fornecidas pelo sistema que dão suporte a uma sessão confiável como opção, mas não habilitam uma por padrão incluem:

- <xref:System.ServiceModel.WSHttpBinding>

- <xref:System.ServiceModel.WSFederationHttpBinding>

- <xref:System.ServiceModel.NetTcpBinding>

Para obter um exemplo de como criar uma associação personalizada, consulte [como: criar uma associação de sessão confiável personalizada com HTTPS](how-to-create-a-custom-reliable-session-binding-with-https.md).

Para obter uma discussão sobre as associações do WCF que dão suporte a sessões confiáveis, consulte [associações fornecidas pelo sistema](../system-provided-bindings.md).

## <a name="when-to-use-reliable-sessions"></a>Quando usar sessões confiáveis

É importante entender quando usar sessões confiáveis em seu aplicativo. O WCF dá suporte a sessões confiáveis entre pontos de extremidade ativos e ativos ao mesmo tempo. Se seu aplicativo exigir que um dos pontos de extremidade esteja indisponível por um período de tempo, use filas para obter confiabilidade.

Se o cenário exigir dois pontos de extremidade conectados via TCP, o TCP poderá ser suficiente para fornecer trocas de mensagens confiáveis. Embora não seja necessário usar uma sessão confiável, como o TCP garante que os pacotes cheguem em ordem e apenas uma vez.

Se o seu cenário tiver qualquer uma das características a seguir, você deve considerar seriamente o uso de uma sessão confiável.

- Intermediários SOAP, como roteadores SOAP

- Intermediários de proxy ou pontes de transporte

- Conectividade intermitente

- Sessões via HTTP

## <a name="see-also"></a>Consulte também

- [Usando associações para configurar serviços e clientes](../using-bindings-to-configure-services-and-clients.md)
- [Sessão confiável de WS](../samples/ws-reliable-session.md)
