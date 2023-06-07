---
title: Cenários síncronos utilizando HTTP, TCP ou pipe nomeado
ms.date: 03/30/2017
ms.assetid: 7e90af1b-f8f6-41b9-a63a-8490ada502b1
ms.openlocfilehash: 662067fc5564c9421ce24b28b291d06690b129ea
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589178"
---
# <a name="synchronous-scenarios-using-http-tcp-or-named-pipe"></a>Cenários síncronos utilizando HTTP, TCP ou pipe nomeado
Este tópico descreve as atividades e transferências para diferentes cenários de solicitação/resposta síncronas, com um cliente de thread único, usando HTTP, TCP ou pipe nomeado. Consulte [cenários assíncronos usando http, TCP ou pipe nomeado](asynchronous-scenarios-using-http-tcp-or-named-pipe.md) para obter mais informações sobre solicitações multi-threaded.  
  
## <a name="synchronous-requestreply-without-errors"></a>Solicitação/resposta síncrona sem erros  
 Esta seção descreve as atividades e transferências para um cenário de solicitação/resposta síncrona válido, com um cliente de thread único.  
  
### <a name="client"></a>Cliente  
  
#### <a name="establishing-communication-with-service-endpoint"></a>Estabelecendo comunicação com o ponto de extremidade de serviço  
 Um cliente é construído e aberto. Para cada uma dessas etapas, a atividade de ambiente (A) é transferida para uma atividade "cliente de construção" (B) e "Open Client" (C), respectivamente. Para cada atividade sendo transferida para o, a atividade de ambiente é suspensa até que haja uma transferência de volta, ou seja, até que o código de ServiceModel seja executado.  
  
#### <a name="making-a-request-to-service-endpoint"></a>Fazendo uma solicitação para o ponto de extremidade de serviço  
 A atividade ambiente é transferida para uma atividade "processAction" (D). Nessa atividade, uma mensagem de solicitação é enviada e uma mensagem de resposta é recebida. A atividade termina quando o controle retorna ao código do usuário. Como essa é uma solicitação síncrona, a atividade de ambiente suspende até que o controle seja retornado.  
  
#### <a name="closing-communication-with-service-endpoint"></a>Fechando a comunicação com o ponto de extremidade de serviço  
 A atividade de fechamento do cliente (I) é criada a partir da atividade ambiente. Isso é idêntico ao novo e aberto.  
  
### <a name="server"></a>Servidor  
  
#### <a name="setting-up-a-service-host"></a>Configurando um host de serviço  
 As atividades novas e abertas do ServiceHost (N e O) são criadas a partir da atividade de ambiente (M).  
  
 Uma atividade de ouvinte (P) é criada a partir da abertura de um ServiceHost para cada ouvinte. A atividade do ouvinte aguarda para receber e processar dados.  
  
#### <a name="receiving-data-on-the-wire"></a>Recebendo dados na conexão  
 Quando os dados chegam na transmissão, uma atividade "ReceiveBytes" será criada se ainda não existir (Q) para processar os dados recebidos. Essa atividade pode ser reutilizada para várias mensagens em uma conexão ou fila.  
  
 A atividade ReceiveBytes inicia uma atividade de ProcessMessage (R) se tiver dados suficientes para formar uma mensagem de ação SOAP.  
  
 Na atividade de R, os cabeçalhos de mensagem são processados e o cabeçalho ActivityId é verificado. Se esse cabeçalho estiver presente, a ID da atividade será definida como a atividade processAction; caso contrário, uma nova ID será criada.  
  
 As atividades processAction são criadas e sendo transferidas para o, quando a chamada é processada. Essa atividade termina quando todo o processamento relacionado à mensagem de entrada é concluído, incluindo a execução do código do usuário (T) e o envio da mensagem de resposta, se aplicável.  
  
#### <a name="closing-a-service-host"></a>Fechando um host de serviço  
 A atividade de fechamento do ServiceHost (Z) é criada a partir da atividade de ambiente.  
  
 ![Diagrama mostrando cenários síncronos: HTTP, TCP ou pipes nomeados.](./media/synchronous-scenarios-using-http-tcp-or-named-pipe/synchronous-scenario-http-tcp-named-pipes.gif)  
  
 No \<A: name> , `A` é um símbolo de atalho que descreve a atividade no texto anterior e na tabela 3. `Name`é um nome abreviado da atividade.  
  
 Se `propagateActivity` = `true` , ação de processo no cliente e no serviço tiverem a mesma ID de atividade.  
  
## <a name="synchronous-requestreply-with-errors"></a>Solicitação/resposta síncrona com erros  
 A única diferença com o cenário anterior é que uma mensagem de falha SOAP é retornada como uma mensagem de resposta. Se `propagateActivity` = `true` , a ID da atividade da mensagem de solicitação será adicionada à mensagem de falha SOAP.  
  
## <a name="synchronous-one-way-without-errors"></a>Unidirecional síncrono sem erros  
 A única diferença no primeiro cenário é que nenhuma mensagem é retornada ao servidor. Para protocolos baseados em HTTP, um status (válido ou erro) ainda é retornado ao cliente. Isso ocorre porque HTTP é o único protocolo com uma semântica de solicitação-resposta que faz parte da pilha de protocolo do WCF. Como o processamento TCP está oculto do WCF, nenhuma confirmação é enviada para o cliente.  
  
## <a name="synchronous-one-way-with-errors"></a>Síncrono unidirecional com erros  
 Se ocorrer um erro durante o processamento da mensagem (p ou posterior), nenhuma notificação será retornada ao cliente. Isso é idêntico ao cenário "síncrono unidirecional sem erros". Você não deve usar um cenário unidirecional se quiser receber uma mensagem de erro.  
  
## <a name="duplex"></a>Duplex  
 A diferença com os cenários anteriores é que o cliente atua como um serviço, no qual ele cria as atividades ReceiveBytes e ProcessMessage, semelhante aos cenários assíncronos.
