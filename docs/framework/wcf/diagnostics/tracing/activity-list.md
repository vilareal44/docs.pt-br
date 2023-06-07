---
title: Lista de atividades
ms.date: 03/30/2017
ms.assetid: 5540e185-ce8e-4db3-83b0-2b9f5bf71829
ms.openlocfilehash: 8d43cc878d54efbd4908f92c3405bef2c7956f94
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602161"
---
# <a name="activity-list"></a>Lista de atividades
Este tópico lista todas as atividades definidas por Windows Communication Foundation (WCF).  
  
> [!NOTE]
> Você também pode definir atividades programaticamente para agrupar rastreamentos de usuário. Para obter mais informações, consulte [emitindo rastreamentos de código do usuário](emitting-user-code-traces.md).  
  
## <a name="servicemodel-activities"></a>Atividades de ServiceModel  
 A tabela a seguir lista todas as atividades para os principais cenários de uso.  
  
|Label|Nome da atividade|Tipo de atividade|Descrição|  
|-----------|-------------------|-------------------|-----------------|  
|A, M|Atividade de ambiente|N/A (isso não é controlado pelo ServiceModel)|A atividade cuja ID é definida em TLS antes de qualquer chamada para o código de ServiceModel (lado do cliente ou servidor).<br /><br /> Exemplo: uma atividade em que Open é chamada no cliente WCF ou serviceHost. Open é chamado.|  
|B|Constructo<br /><br /> ChannelFactory. Tipo de contrato: ' [type] '.|Constructo||  
|C|Aberto<br /><br /> [ClientBase&#124;ChannelFactory]. Tipo de contrato: ' [type] '.|Aberto||  
|I|Feche [ClientBase&#124;ChannelFactory]. Tipo de contrato: ' [type] '.|Fechar||  
|M|Construir ServiceHost. ServiceType: ' [type] '.|Constructo||  
|N|Abra o ServiceHost. ServiceType: ' [type] '.|Aberto||  
|Z|Feche o ServiceHost. ServiceType: ' [type] '.|Fechar||  
|O|Escute em ' [endereço] '.|ListenAt|Essa e a próxima atividade são específicas de transporte. A atividade ListenAt representa o conteúdo que mapeia para o endereço onde o ouvinte de canal escuta. No caso do MSMQ, é a própria fila, uma vez que a fila é mapeada para um endereço. Essa atividade escuta conexões de entrada no caso de transportes orientados a conexões, para mensagens MSMQ no caso do MSMQ. Essa atividade é criada durante ServiceHost. Open () e contém os rastreamentos relacionados à criação e à descartação do ouvinte, bem como a transferência para todas as atividades ReceiveBytes.|  
|P|Bytes de recebimento na conexão ' [endereço] '. Receber mensagem MSMQ.|ReceiveBytes|Nessa atividade, os dados que eventualmente receberão uma mensagem do WCF serão processados. Os bytes de entrada são esperados no caso de transporte orientado a conexão ou http. Para TCP/named-pipe, o tempo de vida dessa atividade é o tempo de vida da conexão, pois ela é criada quando a conexão é criada. Para http, é o tempo de vida de uma solicitação de mensagem e é criado quando a mensagem é enviada. Essa atividade contém os rastreamentos relacionados à criação e ao descarte da conexão, se aplicável, e transferências para todas as atividades de processamento de mensagem (objeto).<br /><br /> No caso do MSMQ, é a atividade em que a mensagem MSMQ é recuperada.|  
|Q|Processar mensagem [número]. (Observação: [número] é um valor que aumenta de forma monotônico, que começa em 1.)|ProcessMessage|Processar uma mensagem de entrada. Essa atividade é iniciada quando todos os dados (bytes, mensagem MSMQ) são recebidos para formar um objeto de mensagem do WCF. Os rastreamentos dentro dessa atividade lidam com o processamento de cabeçalho.<br /><br /> Depois que uma mensagem que pode ser expedida é formada, a atividade processAction do ServiceHost é alternada para depois de Pesquisar a ID da atividade correspondente.|  
|D, S|Processar ação ' [ação] '.|Processaraction|Processe a mensagem por meio da pilha de transporte/segurança/RM para expedir a mensagem para o código do usuário no recebimento e na ordem inversa em enviar.<br /><br /> No servidor, essa atividade usa a ID de atividade propagada se for enviada no cabeçalho da mensagem por meio de "propagação da atividade"; caso contrário, um novo GUID será criado.<br /><br /> A mensagem de resposta para contratos de solicitação/resposta também é processada nessa atividade.|  
|T|Execute ' [IContract. Operation] '.|ExecuteUserCode|Execute o código do usuário após a expedição no lado do serviço. Essa atividade fornece um limite para delinear o código ServiceHost do código fornecido pelo usuário.|  
  
## <a name="security-activities"></a>Atividades de segurança  
 A tabela a seguir lista todas as atividades relacionadas à segurança.  
  
|Nome da atividade|Tipo de atividade|Descrição|  
|-------------------|-------------------|-----------------|  
|Configurar sessão segura|SetupSecurity|Existe somente no lado do cliente. Contém todas as trocas de RST */SCT para autenticação e a definição do contexto de segurança. Se `propagateActivity` = `true` , essa atividade será mesclada com as atividades RST/SCT de ação de processo correspondente do serviço \* .|  
|Fechar sessão segura|SetupSecurity|Existe no lado do cliente. Contém a troca de mensagens de cancelamento para fechar a sessão segura. Se `propagateActivity` = `true` , essa atividade será mesclada com a ação de processo "Cancelar" do serviço.|  
  
 A tabela a seguir lista todas as atividades relacionadas ao COM+.  
  
|Nome da atividade|Tipo de atividade|Descrição|  
|-------------------|-------------------|-----------------|  
|Criar instância COM+|TransferToCOMPlus|1 instância de atividade para cada chamada COM+ do código WCF|  
|Executar COM+\<operation>|TransferToCOMPlus|1 instância de atividade para cada chamada COM+ do código WCF|  
  
## <a name="wmi-activities"></a>Atividades do WMI  
 A tabela a seguir lista todas as atividades relacionadas ao WMI.  
  
|Nome da atividade|Tipo de atividade|Descrição|  
|-------------------|-------------------|-----------------|  
|Obter WMI|WMIGetObject|O usuário está recuperando dados do WMI.|  
|Colocar WMI|WmiPutInstance|O usuário está atualizando dados com o WMI.|
