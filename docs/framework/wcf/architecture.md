---
title: Arquitetura do Windows Communication Foundation
description: Saiba mais sobre as principais camadas da arquitetura de Windows Communication Foundation, incluindo contratos, tempo de execução de serviço, mensagens e ativação & hospedagem.
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation [WCF], architecture
- WCF [WCF], architecture
- architecture [WCF]
ms.assetid: a3bcb0a1-56ea-4ba6-9736-d260d90dade5
ms.openlocfilehash: a07d5c4be2e36b8123e39a0a04d841797e34212b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245564"
---
# <a name="windows-communication-foundation-architecture"></a>Arquitetura do Windows Communication Foundation
O gráfico a seguir ilustra as principais camadas da arquitetura do Windows Communication Foundation (WCF).  
  
## <a name="wcf-architecture"></a>Arquitetura do WCF  
 ![A arquitetura do WCF](./media/wcf-architecture.gif "WCF_Architecture")  
  
### <a name="contracts-and-descriptions"></a>Contratos e descrições  
 Os contratos definem vários aspectos do sistema de mensagens. O contrato de dados descreve todos os parâmetros que compõem cada mensagem que um serviço pode criar ou consumir. Os parâmetros de mensagem são definidos por documentos XSD, permitindo que qualquer sistema que entenda o XML processe os documentos. O contrato de mensagem define partes de mensagem específicas usando protocolos SOAP e permite um controle mais refinado sobre partes da mensagem, quando a interoperabilidade exige tal precisão. O contrato de serviço especifica as assinaturas de método real do serviço e é distribuído como uma interface em uma das linguagens de programação com suporte, como Visual Basic ou Visual C#.  
  
 Políticas e associações determinam as condições necessárias para se comunicar com um serviço.  Por exemplo, a associação deve (no mínimo) especificar o transporte usado (por exemplo, HTTP ou TCP) e uma codificação. As políticas incluem requisitos de segurança e outras condições que devem ser atendidas para se comunicar com um serviço.  
  
### <a name="service-runtime"></a>Tempo de execução do serviço  
 A camada de tempo de execução de serviço contém os comportamentos que ocorrem somente durante a operação real do serviço, ou seja, os comportamentos de tempo de execução do serviço. A limitação controla quantas mensagens são processadas, o que pode ser variado se a demanda do serviço aumentar para um limite predefinido. Um comportamento de erro especifica o que ocorre quando ocorre um erro interno no serviço, por exemplo, controlando quais informações são comunicadas ao cliente. (Muitas informações podem dar a um usuário mal-intencionado uma vantagem na montagem de um ataque.) O comportamento dos metadados controla como e se os metadados são disponibilizados para o mundo exterior. Comportamento da instância especifica quantas instâncias do serviço podem ser executadas (por exemplo, um singleton especifica apenas uma instância para processar todas as mensagens). O comportamento da transação permite a reversão de operações transacionadas se ocorrer uma falha. O comportamento de expedição é o controle de como uma mensagem é processada pela infraestrutura do WCF.  
  
 A extensibilidade permite a personalização de processos de tempo de execução. Por exemplo, a inspeção de mensagem é o recurso para inspecionar partes de uma mensagem, e a filtragem de parâmetros permite que ações predefinidas ocorram com base em filtros que atuam em cabeçalhos de mensagens.  
  
### <a name="messaging"></a>Mensagens  
 A camada de mensagens é composta de *canais*. Um canal é um componente que processa uma mensagem de alguma forma, por exemplo, Autenticando uma mensagem. Um conjunto de canais também é conhecido como uma *pilha de canais*. Os canais operam em mensagens e cabeçalhos de mensagens. Isso é diferente da camada de tempo de execução de serviço, que se preocupa principalmente com o processamento do conteúdo de corpos de mensagens.  
  
 Há dois tipos de canais: canais de transporte e canais de protocolo.  
  
 Os canais de transporte lêem e gravam mensagens da rede (ou de algum outro ponto de comunicação com o mundo exterior). Alguns transportes usam um codificador para converter mensagens (que são representadas como XML InfoSets) de e para a representação de fluxo de bytes usada pela rede. Exemplos de transportes são HTTP, pipes nomeados, TCP e MSMQ. Exemplos de codificações são XML e binários otimizados.  
  
 Os canais de protocolo implementam protocolos de processamento de mensagens, muitas vezes lendo ou gravando cabeçalhos adicionais na mensagem. Exemplos desses protocolos incluem WS-Security e WS-fiabilidade.  
  
 A camada de mensagens ilustra os formatos possíveis e os padrões de troca dos dados. O WS-Security é uma implementação da especificação WS-Security que habilita a segurança na camada de mensagens. O canal de mensagens WS-Reliable permite a garantia de entrega de mensagens. Os codificadores apresentam uma variedade de codificações que podem ser usadas para atender às necessidades da mensagem. O canal HTTP especifica que o protocolo de transporte de hipertexto é usado para entrega de mensagens. De modo semelhante, o canal TCP especifica o protocolo TCP. O canal de fluxo de transações governa padrões de mensagens transacionadas. O canal de pipe nomeado permite a comunicação entre processos. O canal MSMQ permite a interoperação com aplicativos MSMQ.  
  
### <a name="hosting-and-activation"></a>Hospedagem e ativação  
 Em sua forma final, um serviço é um programa. Assim como outros programas, um serviço deve ser executado em um executável. Isso é conhecido como um serviço *hospedado internamente* .  
  
 Os serviços também podem ser *hospedados*ou executados em um executável gerenciado por um agente externo, como o WAS (serviço de ativação do Windows ou IIS). O WAS permite que aplicativos WCF sejam ativados automaticamente quando implantados em um computador que executa o WAS. Os serviços também podem ser executados manualmente como executáveis (arquivos. exe). Um serviço também pode ser executado automaticamente como um serviço do Windows. Os componentes COM+ também podem ser hospedados como serviços WCF.  
  
## <a name="see-also"></a>Veja também

- [O que é o Windows Communication Foundation](whats-wcf.md)
- [Conceitos fundamentais do Windows Communication Foundation](fundamental-concepts.md)
